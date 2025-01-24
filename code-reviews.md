# Code reviews at La Suite numérique

Code reviews are collaborative steps in the development of our projects.  The
purpose of code reviews is twofold. They allow both to:

* check the quality of the code and
* to communicate actively on the evolutions.

Code reviews concern all members who are involved in the development team,
regardless of their competence level.  This concept raises questions about their
effectiveness and relevance both on the side of the developer submitting the
review and on the side of the developers reviewing the code.

> When ? How? How long do we review codes?  Which focus? Which feedback to give
> to the reviewer?

## Approach of code reviews

Repository managers such as GitHub or GitLab are helpful to review codes.
Associated workflows, pull request (PR) or merge request (MR) ease the tracking
of information shared in code reviews.

We recommend considering a fine granularity of code reviews.  The benefit is
both temporal and qualitative.  By doing code reviews as frequently as possible,
the exercise is made easier and all members feel more involved in development.
By doing light and targeted code reviews on a specific topic, the review is done
with more attention because it is less complex.

A code review is an opportunity to **actively** communicate about the changes to
other developers and get feedback on the quality of the changes.  As far as
possible, a code review should be done in small changes.  In this perspective,
we advise against doing code reviews for a collection of changes that can be
analyzed independently.  However, it is important to keep in mind that what is
under review must be understandable in its software context.  Some complex
refactorings or features can not be split in small changes as they would lose
software design information.  The reviewer would then fail to comment on design
choices and be limited to a simple technical review.

When opening a PR (or MR) for a review, we recommend to assess the granularity
of the review to keep visible the algorithm and design of the feature.

## Best practices for reviewing codes

The challenge of a code review is to position oneself in the face of another
member's work production.  It is not a question of sanctioning his/her work, but
of assessing it; issuing an opinion.

Here are 5 good practices to adopt that may be beneficial for a code review.

### Engage conversation for clarification 💬

The point here is to question what has been produced.  It is not because the
reviewer has difficulty understanding the code that the code quality is bad.
Code reviews are an exercise that enhances the value of the work produced in a
team spirit.  We recommend on asking the developer why he/she did it rather than
being prescriptive or tell him/her to code in a specific way.

> I am not sure to understand. What was your point when doing this?

Also, to discuss about the choices made by the developer, we encourage to
suggest alternatives that could optimize what has been done.

> Doing it like this could solve the problem in only one step. What do you think
> of it? (WDYT?)

### Have a pragmatic state of mind ✔️

It is important to keep in mind that what is provided is functional.  External
parameters can force the developer not to provide optimal code.  He/she is not
omniscient and learns from team work too.  Having a functional code is the
paramount issue (considering that good practices of coding are respected).

### Review as often as possible 🕙

Code reviews will be relevant and beneficial if the exercise is frequent within
the team.  It is not the business of a few members but of each one of them
because such exercise contribute to the good integration and involvement of
everyone.

### Focus on the main 🧠

A code review should not be overthought.  The reviewer should focus on the
algorithm and the code.  With a problem comes a solution.  It must be possible
to be synthetic and have a global vision of what is in question.

### Communicate positively and with consideration 😃

Finally, the most important is the way to communicate during a review.  We want
to keep a warm and team spirit in the perspective of celebrating and encouraging
progress in a joyful and motivating mood.  In addition to ensuring that positive
and enthusiastic comments are always written, don't hesitate on using gifs and
emojis in PR (or MR) conversations. 😉


## References

* [On Code Reviews](https://tailordev.fr/blog/2017/02/03/on-code-reviews/)
