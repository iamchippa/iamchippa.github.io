---
layout: post
title: The Cracks in Trivia Crack
---

Since I just spun up my github blog pages, I'm moving this post over from
medium.com - original link [here](https://medium.com/@iamchippa/the-cracks-in-trivia-crack-3fac78b50f38)

==================

The cracks in Trivia Crack A little design flaw that allowed me to never lose.

My girlfriend started playing this game on iOS called Trivia Crack. Naturally, she wanted me to join, so I downloaded it to my iPad and started spinning the wheel. Fun questions and a fun game! It’s been #1 in the App Store for a bit now, and I can see why. The general idea is to answer questions from six different categories — the person that wins all six categories wins the game. Get a question correct, and you keep playing. Get a question wrong, and it’s the other players turn. Simple, straightforward, and a bit addicting.

![]({{ site.baseurl }}/images/trivia-crack/Trivia_Crack_screenshot.jpg)

The interfaces of Trivia Crack: Spinner, Dashboard, QuestionsI have a ton of fun seeing what apps are doing “under the hood”. For instance, I regularly pipe my iPad traffic through Burp, just to see what the API calls look like. Sometimes I find nothing, sometimes I find something interesting, and then sometimes I find things that make me chuckle a bit. This is one of the latter.

Refreshing the dashboard on the home screen returned a fairly large JSON response. Looking at the response, I realized that there were some keys with interesting values — the question that was about to be asked, along with the answer to said question.

![]({{ site.baseurl }}/images/trivia-crack/tc_json.png)

JSON response with the question and correct answerMy suspicion was confirmed after I spun the wheel. The question in the JSON response was the next question asked! I selected the correct answer, cracked a smile, and clicked continue. I looked back over at Burp and sent another request to the Dashboard API. Boom. New question and answer in the response.

Thinking about how funny/awesome this was (and thinking how I’d never lose again), I wanted to take it a step further and make a quick script to get the answers for me. Named “Watson”, after IBM’s famous Jeopardy contestant, the script will make a request to the Dashboard API, parse through the JSON response and spit out the answer to the next question in a nice, neat format.

![]({{ site.baseurl }}/images/trivia-crack/trivia_crack_watson_short.gif)

watson.py in action — link to the full videoWhile this was all fun and games, this design flaw is effectively game breaking. A better way to handle this would be to process the answer on the server side, and return a “correct” or “incorrect” response to the answer being submitted.

I passed this issue, and a few others, over to the Trivia Crack team to fix. I received a tweet acknowledgment that my support request was received, but haven’t been able to get a response or update since.

When I first noticed this issue, I did a very quick search to see if there were any ‘hacks’ out there for trivia crack. I noticed some for the Facebook version, but I thought they were click baits. After writing this up, I came across a few plugins that really did answer questions for you. I decided to check one out to see how worked — it was abusing the same issue:

![]({{ site.baseurl }}/images/trivia-crack/other.png)

Since plugins for this have been out since at least May, and the issue hasn’t been addressed, I felt it was safe to detail how it works (and how it can be fixed).

Just for fun: I was bored the other day and decided to upgrade Watson to just
go ahead and submit the answers for me. And if you’re wondering…[no, my
girlfriend was not amused](https://vimeo.com/115355438).

#### Timeline of Disclosure

Dec 2, 2014 — Sent request to Trivia Crack support about issue. Also tweeted to @triviacrack. Tweet response from @triviacrack acknowledging they received the request.

Dec 12, 2014 — Sent status request to Trivia Crack support, and request to write a blog post.

Dec 15, 2014 — Tweeted @triviacrack requesting that someone respond to my email to start a dialog.

Dec 16, 2014 — Tweeted @triviacrack and @etermax requesting that someone respond to my email to start a dialog.

Dec 23, 2014 — Sent another update through support. Also looked into other “trivia crack hack” plugins.
