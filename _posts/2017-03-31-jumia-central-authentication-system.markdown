---
layout: post
title:  "User Authentication: Look ma, no servers!"
date:   2017-03-31 10:18:00 +0000
author: "Daniel Loureiro & Tiago Caxias"
thumbnail: /img/posts/ShutdownYourServers-Jumia-thumbnail.jpg
categories: [engineering-quality, security, data-engineering]
tags: [devops, dynamodb, security]
permalink: /jumia-central-authentication-system/
---



<center>
	<img src="/img/posts/ShutdownYourServers-Jumia.jpg" style="width: 100%;" />
</center>
    
*This is part of a guest post on [Amazon Web Services Compute Blog](https://aws.amazon.com/blogs/compute/a-serverless-authentication-system-by-jumia).


**Want to secure and centralize millions of user accounts across Africa? Shut down your servers!**

*Jumia is an ecosystem of nine different companies operating in 22 different countries in Africa. Jumia employs 3000 people and serves 15 million users/month.*

Jumia unified and centralized customer authentication on nine digital services platforms, operating in 22 (and counting) countries in Africa, totaling over 120 customer and merchant facing applications. All were unified into a custom Jumia Central Authentication System (JCAS), built in a timely fashion and designed using a serverless architecture.


# The challenge

A initiative was started to centralize authentication for all Jumia users in all countries for all companies. But it was impossible to unify the operational databases for the different companies. Each company had its own user database with its own technological implementation. Each company alone had yet to unify the logins for their own countries. The effects of deduplicating all user accounts were yet to be determined but were considered to be large. Finally, there was no team responsible for managing this new project, given that a new dedicated infrastructure would be needed.


With these factors in mind, we decided to design this project as a serverless architecture to eliminate the need for infrastructure and a dedicated team. AWS was immediately considered as the best option for its level of service, intelligent pricing model, and excellent serverless services.


The goal was simple. For all user accounts on all Jumia websites:

* Merge duplicates that might exist in different companies and countries
* Create a unique login (username and password)
* Enrich the user profile in this centralized system
* Share the profile with all Jumia companies

# Requirements

We had the following initial requirements while designing this solution on the AWS platform:


* [Secure by design](https://en.wikipedia.org/wiki/Secure_by_design)
* Highly available via multi-master replication</span></li>
* Single login for all platforms/countries</span></li>
* Minimal performance impact</span></li>
* No admin overhead</span></li>

# Chosen technologies

We chose the following AWS services and technologies to implement our solution.  

**Amazon API Gateway**

[Amazon API Gateway](https://aws.amazon.com/api-gateway/) is a fully managed service, making it really simple to set up an API. It integrates directly with AWS Lambda, which was chosen as our endpoint. It can be easily replicated to other regions, using Swagger import/export.

**AWS Lambda**

[AWS Lambda](https://aws.amazon.com/lambda/) is the base of serverless computing, allowing you to run code without worrying about infrastructure. All our code runs on Lambda functions using Python; some functions are called from the API Gateway, others are [scheduled like cron jobs](http://docs.aws.amazon.com/lambda/latest/dg/tutorial-scheduled-events-schedule-expressions.html).

**Amazon DynamoDB**

[Amazon DynamoDB](http://docs.aws.amazon.com/amazondynamodb/latest/gettingstartedguide/quick-intro.html) is a highly scalable, NoSQL database with a good API and a clean pricing model. It has great scalability as well as high availability and fits the serverless model we aimed for with JCAS.

**AWS KMS**

[AWS KMS](https://aws.amazon.com/kms/) was chosen as a key manager to perform envelope encryption. It’s a simple and secure key manager with multiple encryption functionalities.


**Envelope encryption**

Oversimplifying envelope encryption is when you encrypt a <i>key</i> rather than the data itself. That encrypted key, which was used to encrypt the data, may now be stored with the data itself on your persistence layer since it doesn’t decrypt the data if compromised. For more information, see [How Envelope Encryption Works with Supported AWS Services](https://docs.aws.amazon.com/kms/latest/developerguide/workflow.html).    
Envelope encryption was chosen given that master keys have a 4 KB limit for data to be encrypted or decrypted.

**Amazon SQS**

[Amazon SQS](https://aws.amazon.com/sqs/) is an inexpensive queuing service with dynamic scaling, 14-day retention availability, and easy management. It’s the perfect choice for our needs, as we use queuing systems only as a fallback when saving data to remote regions fails. All the features needed for those fallback cases were covered.


**JWT**

We also use [JSON web tokens](https://jwt.io/) for encoding and signing communications between JCAS and company servers. It’s another layer of security for data in transit.


**Data structure**

Database design is somehow simple in this scenario. DynamoDB records can be accessed by primary key and allow access using secondary indexes as well. We created a UUID for each user on JCAS which is used as a primary key on DynamoDB. Most data must be encrypted so we’ll use a single field for those data. On the other hand, there’s data that needs to be stored in separate fields because they need to be accessed from the code without decryption. The user Id, phone number, email, the [ctime](https://www.quora.com/What-is-the-difference-between-mtime-atime-and-ctime) for the document, the user’s status and the user’s passwords are all out of the main data field. Users existed on the companies’ systems before JCAS came along so they already had passwords created. These are stored in a Map for the rare cases where JCAS needs to authenticate the user with one of those passwords. For faster accesses, we decided to hold the user’s password for JCAS on a separate field considering that most authentications will be performed against this field.

* Id: UUID generated by this system
	* Purpose: Primary Key, unique identification for Jumia companies
* mec, mpn: Main email and phone number respectively hashed
	* Purpose: Searchable fields used when a company doesn’t have an Id
* old_hashes: DynamoDB Map containing legacy password hashes
	* Purpose: This Map contains the password hashes for each pair (company, country) on the companies’ systems – Example on image below
* revision: Timestamp for last item changes
	* Purpose: Avoids invalid operations and reduces a number of API calls to Key Management Service (KMS)
* Stat: Active/Inactive toggle
	* Purpose: Defines whether user data should be considered valid and worth decrypting or just skip it to save a KMS call and Lambda CPU time
* Info: Map containing encrypted data and its KMS key for decryption
	* Purpose: Read on for more details on multiple KMS keys usage
* Secret: User’s password in bcrypt
	* Purpose: Hold the user’s password in JCAS. When set it prevails over any password in old_hashes.

<center>
	<img src="/img/posts/data_at_rest.png" style="width: 100%;" />
</center>

# Security

**Security is like an onion, it needs to be layered and that’s what we did when designing this solution**


Embedded in each layer of this solution, our design makes all of our data unreadable whilst easing our compliance needs.

The field data stores all personal information from our customers that is encrypted using **AES256-CBC** with a key generated and managed by AWS KMS – A new data key is used for each transaction.

The field keys stores the Ciphertextblob generated by KMS and it only can retrieve the keys to decipher the data field above.

Our solution uses KMS from multiple regions, therefore data can be decrypted within any region that is present in keys field.

KMS supports two kinds of keys — master keys and data keys. Master keys can be used to directly encrypt and decrypt up to 4 kilobytes of data and can also be used to protect data keys. The data keys are then used to encrypt and decrypt customer data.

Master keys only allow 4kB of data to be encrypted/decrypted, this was not good for us and also making it future proof for growth needs, we used Data keys to encrypt our customer data, this process is called envelope encryption

Here’s a snippet off code to perform envelope encryption. We just need to encrypt the key with the KMS in that region.

	#Generate new key
	k = kms.generate_data_key(KeyId=CONFIG['kmsKeyA'], KeySpec='AES_256')
	#Cipher plaintext of Region A with key from Region B
	kmsB = sessionB.client('kms')
	kB = kmsB.encrypt(KeyId=CONFIG['kmsKeyB'], Plaintext=k['Plaintext'])
	#Random IV/Salt, even though we use a new datakey per row
	iv = Random.new().read(AES.block_size)
	#Padding the message for the correct block size
	secret = pad(secret)
	#Generate ciphered text with AES256 CBC Mode 
	cipher = AES.new(k['Plaintext'], AES.MODE_CBC, iv )
	#Data to save in the field data
	csecret = iv + cipher.encrypt(secret)
	del secret #Garbage collect secret ASAP
	del k #Release plaintext key from memory



Here’s what is happening:

1 - Ask KMS in the current region to generate a [Data Key](http://docs.aws.amazon.com/kms/latest/APIReference/API_GenerateDataKey.html)

2 - Use the plaintext key to cipher the data field with **AES256-CBC**

3 - Request KMS from another region to **encrypt** the plaintext used to cipher the data (here we use a Master key from other regions)


If one the KMS is unavailable the process will not be affected by the key field will be left empty for the time being. The trigger we have in place that helps us achieve cross-region replication will check if any of the KMS key fields are empty, if true it will write the customer Id to an SQS queue called KMS.

If KMS on EU-CENTRAL-1 is offline, write customer Id to KMS SQS on all regions

<center>
	<img src="/img/posts/kms_failure-1.png" style="width: 100%;" />
</center>

A cron style lambda function will get the customer Id from the SQS queue, check if the needed KMS regions are available, encrypt the data and store the new keys in all the regions. The second region will do exactly the same, again last write wins in case of race condition. (for the sake of diagram clarity arrows were left out) In case that during this process other KMS region need is down or when writing the to DynamoDB is unsuccessful the customer Id will stay in the SQS KMS queue.

Schedule Lambda function will check if KMS SQS have data on all regions, check which regions we have empty in the key field and try to contact KMS, if successful, update data and keys in DynamoDB on all regions

<center>
	<img src="/img/posts/kms_recover-1.png" style="width: 100%;" />
</center>

For communication between our companies and the API we use API Keys, TLS and JWT in the body to ensure the post is signed and verified.



# Data Flow

**Our second requirement on JCAS is system availability**

Companies always call the JCAS API when they authenticate a user. Despite this, business always comes first so if JCAS is down or inaccessible the company’s systems have to continue working just as well, even if in disconnected mode.

We have no reason to believe that an entire AWS region may be down at some point but that’s something we couldn’t risk, therefore we wanted a multi-region system.

Given our requirements for multi-master replication, we decided to use a system where last write wins, taking advantage of the revision field and have it to control all writes. With this in mind, we designed a system that replicates data and accepts writes in all regions.

Design-wise our systems are [CAP-available](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html). Replication to other regions happens synchronously but falls back to asynchronous should the former fails.

In order to provide low latency to the application, we decided not to do replication in the AWS Lambda function that answers to client requests. Instead, we use DynamoDB Streams to start the whole process.

A Lambda function called trigger is responsible for replicating writes across regions via [DynamoDB Streams](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.Lambda.html). This Lambda function will then check the revision field beforehand on the destination (last-write-wins), should this write fails though we fall back to asynchronous mode.

The Lambda function will write the item to a SQS queue named SQS_&lt;region where this sqs is&gt;_&lt;region that failed the write&gt;, for example if it failed a write on DynamoDB in region B we would write to two SQS named SQSAB and SQSBB, and will succeed if at least one SQS queue accepts the write. A cron-style lambda consumes from these SQS queues in all regions and writes to DynamoDB also using check-and-set on revision.

In order to be available across all regions, info field also contains the KMS keys needed to decrypt the user’s data on each region with its own KMS. The same queueing paradigm applies to KMS key reference contained in the info field on the DynamoDB table.


# Wrapping all up

We have 3 Lambda functions and 2 SQS queues where:

1) A trigger Lambda function for DynamoDB stream – Upon changes it tries to write directly to another DynamoDB table in the second region * We insert the full item into 2 SQS queues, SQS_A_B and SQS_B_B (if we are present in only two regions, with the third it would add another one SQS_C_B) * This allows us to have the data available for 14 days against only 24h of the DynamoDB stream

<center>
	<img src="/img/posts/fig1.png" style="width: 100%;" />
</center>


2) A scheduled Lambda (cron-style) that will check a SQS queue for items and try writing them on the DynamoDB that potentially failed

* This way we can replicate all DynamoDB data making it consistent,
* Unless DynamoDB on the replica region is down for more than 14 days

<center>
	<img src="/img/posts/fig2.png" style="width: 100%;" />
</center>


3) The last Lambda function is another cron-style that will check the SQS queue called KMS for any items and fix the issue


<center>
	<img src="/img/posts/fig3.png" style="width: 100%;" />
</center>



**Full infrastructure (for diagram clarity, missing only KMS recover topic #3 above)**


<center>
	<img src="/img/posts/CAS.png" style="width: 100%;" />
</center>


# Integration


In JCAS we authenticate all our customers and only save shareable data between our companies like emails, phone numbers and addresses.

JCAS is the **Single Source of Truth** so our companies have to update their local information based on the information provided by JCAS. Each company will have a set of specific information that is stored locally.

In the event of JCAS unavailability, these very same companies can still store data locally and subsequently call JCAS async API bulk sync with latest changes when JCAS is back up.

Regardless of how data is updated in JCAS, the highest timestamp will always prevail and although it greatly simplifies the system it may cause some discrepancies in user data flow – Here’s a situation to exemplify this:

1 - A user, disconnected from JCAS, connects to **companyA** and change his password  
2 - He might be able to connect to **companyB** using old credentials stored in JCAS because **companyB** may not have synced yet  
3 - When **companyA** manages to bulk sync its local DB to JCAS, the password previously set on companyA will be updated on JCAS, therefore, affecting all companies  
4 - This new password is stored in old_hashes clearing the secret as it was an asynchronous process and we will not send the plaintext password in the bulk sync method. We can match any password hash from our companies  
5 - Upon next login, JCAS will match the password in old_hashes, but since this time it’s synchronous we take the plaintext password create a new hash store it in secret and clear the old_hashes  
6 - Some corner cases are created by this design but distributed systems are all about tradeoffs.  


# Results

Upon going live we noticed a minor impact in our response times – Note the **brown** legend in the images below

<center>
	<img src="/img/posts/lat_1.png" style="width: 100%;" />
</center>

This was a cold start, as the infrastructure started to get hot, response time started to converge. On 27 April we were almost at the initial 500ms

<center>
	<img src="/img/posts/lat_2.png" style="width: 100%;" />
</center>

It kept steady on values before JCAS went live (≈500ms)

<center>
	<img src="/img/posts/lat_3.png" style="width: 100%;" />
</center>

As of the writing of this article, our response time kept improving (dev changed the method name and we changed subdomain name)


<center>
	<img src="/img/posts/lat_4.png" style="width: 100%;" />
</center>


Customer login used to take ≈500ms and it still takes ≈500ms with JCAS. Those times have improved as other components changed inside our code.


# Lessons Learned

* Instead of the standard, creating your own DynamoDB cross-region replication might be a better fit for you, if your data and applications allow it
* Take some time to tweak the Lambda runtime memory, remember it’s billed per 100ms, it will save you money if you have it run near a round number
* KMS takes away the problem of key management with great security by design, it really simplifies your life
* Always check the timestamp before operating on data, if it’s invalid save the money by skipping further KMS, DynamoDB and Lambda calls. You’ll love your systems even more
* Embrace the serverless paradigm, even if it looks complicated at first it will save your life further down the road when your traffic bursts or you want to find an engineer who knows your whole system


# Next steps

We are going to leverage everything we’ve done so far to implement SSO in Jumia. For a future project, we are testing OpenID connect with DynamoDB as a backend.

# Conclusion

We did a mindset revolution in many ways.
Not only we went completely serverless as we started storing critical info on the cloud but we also moved all user data to be decoupled between local systems and central auth silo.
Managing all of these critical systems became far more predictable and less cumbersome than we thought possible.
For us this is the proof that good and simple designs are the best features to look out for when sketching new systems.

If we were to do this again, we would do it in exactly the same way.