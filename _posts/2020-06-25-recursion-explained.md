---
layout: post
title:  "Recursion explained using a ticket queue problem"
categories: [Coding]
---
The concept of Recursion has always been very fascinating to me ever since I heard about it back in my early days of programming. As beginners, we are always taught to call other functions defined somewhere else from within the scope of the current function. The reason for this could be to make the code more modular, remove code duplication or to simply make the existing function more readable. These are all valid scenarios but then we are introduced to Recursion. The very idea of a function calling itself seems baffling and you would wonder why in the world would you do want to do that? Of course, we also immediately think of the movie Inception (dream within a dream within a dream). The plot of the movie was quite confusing (though enjoyable 🤘) and surely recursion seems the same. However, like with Inception, if we break down the meaning of recursion through relatable real life examples, it would help us appreciate the concept even more.

So here is the premise of this post. I am going to try to explain the concept of recursion through the problem of buying a movie ticket. Imagine you are in the year 1993 and Jurrasic Park movie has just released. You really want to watch that movie so you decide to go to the movie theater. Remember this is the pre-internet time where one would not be able to book movie tickets online via their phones. (You would have to stand in queues to get tickets) Upon reaching the theater, you realize that the queue to purchase the tickets is really huge and you run to quickly get to the back of the line. As soon as you reach your spot in the queue, other people also come behind you.

![](/assets/images/recursion-ticket-counter/ticket_recursion1.png)

Now here is the catch. As you are waiting for your turn, you realize that this would be a waste of time, if you don't get the ticket till you reach the entrance of the ticketing window. You also don't want risk losing your spot in the queue by heading up to the front of the counter to enquire about remaining tickets 😟

![](/assets/images/recursion-ticket-counter/ticket_recursion2.png)

So you devise an idea and decide to ask the person in front of you about the remaining tickets. Since the person in front of you is also unaware of the answer to this question, he/she decides to ask the person in front of him/her. This story continues till the person at the front of the queue finally asks the ticket vendor at the counter and gets the answer. Now that person could had shouted the answer out loud so that everyone in the area hears about it but to remain civil, the person sends the message back in the same way they originally had been requested. Soon the person in front of you tells you the count of the remaining tickets and since you can visually gauge that you would be able to get the ticket, you continue staying in the line and enjoy the movie later.🙂

These are the series of events for you knowing about the tickets remaining.

![](/assets/images/recursion-ticket-counter/ticket_recursion3.png)
![](/assets/images/recursion-ticket-counter/ticket_recursion4.jpg)
![](/assets/images/recursion-ticket-counter/ticket_recursion5.jpg)
![](/assets/images/recursion-ticket-counter/ticket_recursion6.jpg)

The solution described now maps to the concept of recursion if you see the below code. Imagine you have a queue of users and only the user that is currently served by the ticketCounter can enquire about the remaining tickets.

{% highlight javascript %}
let queue: [User] = [User1, User2 ...., You, User10, User 11....];
function checkTicketsRemaining(userPosition) { // your position is 9th in the queue (0 based indexing)
  const user = queue[userPosition];
  if (ticketCounter.isServingUser(userPosition)) {
    return user.ticketsRemaining();
  }
  return checkTicketsRemaining(userPosition - 1);
}
{% endhighlight %}

### Conclusion
The situation described in this post is over simplified and just for the purpose of understanding recursion. By no means, it covers the complete beauty, sophistication and complexity of the topic of recursion. However, I hope readers find this example a bit more intuitive than the regular fibonacci series example which in my opinion is not too relatable to understand.