# Performance VS Scalability

After I read these 2 articles: [A Word on Scalability](https://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html) and [Performance and Scalability](https://www.allthingsdistributed.com/2006/04/performance_and_scalability.html), they makes me confused. I think they are the same thing and I do not think scalability will impact performance. I am quite surprised when I read this article [Performance Vs Scalability](https://blog.professorbeekums.com/performance-vs-scalability/) and it introduce these two concepts more intuitively. 

Let’s get definitions out of the way. **Scalability** is being able to handle large amounts of users/data/traffic. **Performance** is about speed. While the speed of light may be constant, software system speed is far from.

A useful analogy is checking out at the supermarket. Let’s say a cashier can checkout one shopper every minute. Assuming no more than one shopper goes to the cashier every minute, then a shopper can checkout in one minute.

![img](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/one_cash_register.png)

What happens when 2 shoppers attempt to go to the cashier every minute? One shopper will be checked out in 1 minute, but the second shopper will take 2 minutes.

![img](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/short_queue.png)

The **performance** we care about is not how fast the cashier can checkout a single person. It is how long the **average** shopper is waiting. Performance for a single cashier works for us well enough when there aren’t very many people. The performance **drops** quickly when we add shoppers at a faster rate than a single cashier can handle. 

![img](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/performance.png)

**Scalability** is making sure that the **performance is constant** no matter now many people are coming in. This may mean creating things that lower performance when there aren’t very many people **(low scale)**. Ideally our business is successful and we’re more concerned about performance with a greater number of people **(high scale)**.

![img](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/scalability.png)

In the discussion above, we can find for a single unit, we cannot gain performance and scalability both. Then I will introduce some methods for a scalable system.

One tactic used is to add **caching**. We can save popular data in a persistent cache and requests will first check in the cache memory. If it hits, then responds directly. It will save a lot of time. Otherwise, if it misses, then we should do the normal process.

The other thing is to add **more servers** and spread requests among them. But there are some issues: what if a server is down, what if a job is time consuming and makes jobs behind it waiting all the time. Thus, we introduce a **message queue** to solve it. A message represents one request. Messages are added to a message queue. Software processes called workers take messages from this queue and process them much like cashiers checkout shoppers.

![img](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/messagequeue.png)

I hope this has illustrated what some of those techniques look like. Also the result will be more complicated code that runs slower on a developer machine so it may seem like a pointless effort at first. The effort will pay off though when you have a successful high scale business.