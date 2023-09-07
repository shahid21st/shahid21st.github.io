---
layout: post
title: "Writing JIRA Plugin"
date: 2007-09-24
comments: true
tags: [jira, development]
---

After taking an extended leave for my M.Sc. exams, I had to return to the office for one week halfway through my break. I extended the rest of the leave by one week as two of my exams were postponed. Returning to the office, I lost my seat, my CPU was in the kitchen, and a newcomer was using my monitor. With the kind help of all, I placed my one-week post in a corner amid the testing team. (That was not a bad place, except for the daily scrum of the testing team when I felt so much in the wrong place). At that time, our project manager Mehrin Shahed was kind enough to assign me to work, and that was writing a JIRA plugin.

<!--break-->

The goal of the Jira plugin is to affect the parent task's remaining estimate and time spent fields when corresponding fields of subtasks are updated. I did the initial setup and learned to write the Jira plugin that week. After finally returning from leave and after the hustle and bustle of the voter ID project, I finally completed developing the plugin. In subtask-type issues, the remaining estimate and time spent can be updated in two ways - either by 'log work done' or by 'edit' issue. JIRA has a powerful and flexible extension mechanism. I only needed to catch an ISSUE_WORKLOGGED or ISSUE_UPDATE type event and update the corresponding fields of the parent task according to the subtask values. A listener class (listeners are custom classes that are called when any specific event occurs in JIRA; custom listeners can be registered via the JIRA administration panel) would suffice for this, but I wrote it in a plugin style for convenience (after all, when there exists a nice JIRA plugin development kit and Maven-Atlassian-Idea plugin to setup the plugin project in our so familiar IntelliJ Idea IDE.) You need JDK and Apache Maven to use the JIRA plugin development kit.

As I said above, extending JIRA is easy, and visit [this](https://developer.atlassian.com/jiradev/getting-started) if you are interested.