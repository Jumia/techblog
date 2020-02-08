---
layout: post
title:  "Software testing: the seven truths you have to deal with!"
author: "Helena Rodrigues"
date:   2020-02-01
thumbnail: /img/posts/software-testing-the-seven-truths-you-have-to-deal-with/software-testing.jpeg
categories: [engineering-quality]
tags: [testing]
permalink: /software-testing-the-seven-truths-you-have-to-deal-with/
---

<center>
    <img alt="Software testing: the seven truths you have to deal with" src="/img/posts/software-testing-the-seven-truths-you-have-to-deal-with/software-testing.jpeg" style="width: 90%; margin-bottom: 10px;" />
</center>

## Introduction

Humans develop software and they do it with a purpose. Software will do what it is programmed to do, through coding. However, with the increasing complexity of the systems developed, testing activities have become inevitable and an essential piece in software development.

Testing is not a new activity, it is a creative and intellectual challenging task like code programming and they should walk side by side.

From real practice and research for testers it was written **“The seven fundamental principles of software testing”**. They are intended to be the main guidelines for you (tester!) or person that performs testing (that is not the tester!). They help to effectively use your time and effort to discover defects on a software which is under testing.

As an experienced tester, you will understand every principle as true and, certainly, will recognize yourself in them on your daily job. Sometimes, you might get trapped on testing in some way. Refreshing and learning new concepts will help you to do better your job.

Each time you revisit these principles you tend to see them with your acquired experience and learn something new. I invite you to follow me on this article where we will visit the **“The seven fundamental principles of software testing”**, context them with my experience and learn how to deal with them on your daily job!

## 1. Testing shows presence of bugs

Testing activity has two main purposes:

- check if code built by developer makes what the requirements asked by stakeholders;
- check that what was developed did not break the expected behaviors already implemented.

On this process, one of our goals while testers (there are too many, I can write another article about this topic) is to find bugs, report them, so they can be fixed before software’s release. However, not finding bugs does not tell that software is bug-free.

Imagine we have a website which has two features implying popup display. One opens an informative popup; the other one shows a newsletter subscription popup each time no one subscribed users make three actions on our page. These two features were tested without any issue to report. Some weeks later, after some exploratory testing, we have found that if the two popups are shown simultaneously the user is not able to close the first popup. Here, we have an example that the fact that bugs are not found doesn’t mean our software is bug-free.

## 2. Exhaustive testing is impossible

The second principle tells us that it is impossible to perform exhaustive testing. But what is exhaustive testing? Exhaustive testing means that you will test all the functionalities using all valid and invalid inputs and preconditions. So, is this easy to happen on your daily job? Yes, for the trivial cases; not for the majority.

I give you an example: imagine you have to test the email’s field on an e-commerce registration’s form. To perform an exhaustive testing you would have to test with many combinations like: latin/chinese/cyrillic/other letters, with @, without @, special characters like “,). #$%&/()=, and so on. If you want to go deeper on this topic there is an organization called [IETF (Internet Engineering Task Force)](https://ietf.org/) that develops and maintains the Request for Comments (RFC). These documents specify the standards that will be implemented and used through the Internet. For this specific case of email address validation, the RFC is the [RFC 5322](https://tools.ietf.org/html/rfc5322).

Even if you have some available time, it would not be possible to validate all possibilities because of:

- project deadlines
- available time
- human resources
- the amount of outputs
- too complex design

To overcome the exhaustive testing, your tests should be designed and executed according to the involved risk, priorities, time and business context. Imagine, on previous example, that the software is going to be used in the United Kingdom. Some of the combinations for email address input form will be discarded like Chinese and Cyrillic characters and testing will be less exhaustive. Be effective, brief and precise, and in the end, imagine that you are an end-user of your software.

## 3. Early Testing

More than only testing, after developer delivers his job, you can make something more valuable: to anticipate incoherence/failure/information missing.

After product stakeholders and product owners, you are the person that knows your product better. With that knowledge you can anticipate incoherence/failure/information missing since the first product development phase, in other words, product owner has a clear vision about business needs. Groomings/refinements meetings are supposed to prepare and aggregate as much information as possible, refine the task’s information before developers start their activities. One incoherence/failure/missing is not anticipated here and it is only discovered after development and/or release, the cost will be highly increased. To avoid this you can:

- get in touch with the task you will test later and prepare in advance your test strategy;
- check if any existing feature can be affected by the new one;
- try to locate the lack of information about the feature;
- leave some notes to refresh the developer’s mind in what he should not forget to validate when he starts to code;
- prepare a set of test data for test designed test conditions.

So, it is always easier and cheaper to fix the issues from the beginning rather than changing the whole system if that incoherence/failure/information is missing and bugs are found later.

## 4. Defect Clustering (Aggregation)

Normally, bugs are not often distributed evenly throughout an application. Defect clustering (or aggregation) on software testing is based on [Pareto principle](https://www.investopedia.com/terms/p/paretoprinciple.asp), that tell us that 80 percent of bugs are due to 20 percent of code!

There are some facts that can contribute to this principle, such as:

- legacy code
- different teams touching the same part of code
- features that may suffer many changes
- fickle 3rd-party integration

Whatever the reason is, the whole team should always try to improve the ability to identify the defect-prone areas of the application without forgetting other areas.

A note here: on purpose, I above referred whole team because I consider that this principle evokes something I truly believe while tester: a product quality is a team’s responsibility, not only a tester’s responsibility.

## 5. Pesticide Paradox

On agriculture, when the farmer applies a pesticide he intends to combat a type of plague. However, if he always applies the same pesticide he will combat the same plague and another one will remain there or will appear new ones.

The same approach can be applied on software testing, calling “Pesticide Paradox”. If your test suite is not updated with new tests or if you repeat the same set of tests, eventually they will no longer be able to find new bugs. To overcome this “Pesticide Paradox”, you can use some tips such as:

- Regularly review and, if necessary, correct the tests already existent;
- Refresh yourself with a pause and certainly new ideas will appear on your mind to create new tests; every time you have new ideas, write them down no matter where you are; it has already happened to me, having a task on a day and, at home, appeared in my mind new ideas in order to deal with a problem;
- Ask another tester's opinion, even if he/she is a less experienced tester than you, to review your tests or ideas. He/she can give you new ideas or only talk about something that will awake and refresh your brain;
- The opposite is also valid: if a tester requests your opinion do not refuse it: you will learn together;
- In case you are a tester dedicated to a specific area of a product, dare you to explore other areas in order to discover new bugs; you will know more about the product too.

## 6. Testing is context dependent

Products and projects have different features and requirements. Hence, testers can’t apply the same test approach for different projects.

For example, a critical system software, such as medical or transports, needs more and different testing than an e-commerce website or a videogame. A critical system software will require risk-based testing, be demanding with industry regulators and specific test design techniques.

On the other hand, an e-commerce website needs to go through rigorous functional and performance testing; for example, it should not have errors on calculations on order’s placement and should load and work quickly on a promotional campaign.

Some days ago, I made an order on one popular Portuguese online store specialized in household appliances and technology products. It was not the first time that I used that online store, until now without major issues. After ordering with success,the message of the order was not appearing on the my order list. After some days without any news about it I decided to get in touch with the Help Center that told me, by unexplained reasons, the order had been lost on their order processing system. Then, it was made a refund. Imagine if this order was an airplane! The damage caused by a lost order is incomparable with a lost airplane.

## 7. Absence of errors — fallacy

Imagine you are in a project that has a rigorous and complete test procedure with all [test levels](https://www.guru99.com/levels-of-testing.html), the software is good, technically speaking, no bugs are found and it is released to the production environment. If that software doesn’t meet the user’s needs it will never be used by anyone. It is crucial to know our users and build the software based on their needs. This work should start very early, on requirements definition stage, involving the business people that knows better the user’s needs.

## Conclusion

These are **“The seven fundamental principles of software testing”** I hope this reading helped you to think about them and learn something new. Each time I visit them gathering with my experience I get a better understanding about these guidelines. I invite you to make the same exercise more often!
