## 3 Must haves for Merge Requests for swift Code Review

One huge part of Code Collaboration is Merge Requests (MR).

Here you can tell other members of the Repository about your Code Solution.

Let's make sure that the Reviews on your Merge Requests is as swift as possible. 

Not only for you, *but for the reviewers*

## 1. Title

The title is very important for a good MR. A good title should explain what the MR is about.

Think of it as the SEO Title of your WebPage.

If it is misleading, the reviewers would have to struggle hard to get the Core Idea or reason for the Merge Request.

*For example:*

**Bad:** `Refactoring Components`

**Good:** `ABC-123: Refactor Login Flow for Better Error Handling`

It tells me that:
- Ticket is present for this change
- What parts of UI should expect some change
- Reason for Change

---

## 2. Brief Description

Of course, the title can not have everything.

That's why there is the description.

It should briefly touch base on Major changes done to achieve the title.

For the above title of MR, the following could be a good description:

```
- Removed unnecessary Props
- Adopted composition of Component and reduced Prop Drilling
- UI Error reporting is unified for a form via the `useErrors` hook
- Inputs are now logic-less allowing better testing 
```

Now with the above description, I would be expecting the File changes and pay more attention to the Code in certain areas.

---

### 3. Related Ticket/Issue

A change should be introduced to Code with some Ticket.

This ticket can be Bug, Feature/Story, Task etc.

Presence of ticket allows+maintains the Documentation and Reasoning of Change outside the Repository.

With things outside the code, it allows collaboration from non-code participants.

In some of the cases, Ticket and MR are in the same system.

For instance, Issues from GH are tickets for PR

---

## Conclusion


I think that the above points allow the reviewer to prepare for what they are going to review.

> What would you need as a Reviewer as a must-have?

Let me know through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/intent/follow?screen_name=patel_pankaj_)  and/or  [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack) 
If you find this article helpful, please share it with others 🗣
Subscribe to the blog to receive new posts right to your inbox.


