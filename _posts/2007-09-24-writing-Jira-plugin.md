---
layout: post
title: "Writing JIRA Plugin"
date: 2007-09-24
comments: true
tags: [jira, development]
---

After taking a long leave for my M.Sc. exams, I had to come back to office for one week halfway through my leave. The reason was to extend the leave by one week at the end as two of my exams were postponed. Returning to the office, I found that I lost my seat, my CPU was in the kitchen and a newcomer was using my monitor. With the kind help of all, I placed my one week post at a corner in the midst of the testing team. (That was not a bad place at all, except the daily scrum of testing team when I felt so much in a wrong place). In that time, our project manager Mehrin Shahed was kind enough to assign me a work and that was writing a JIRA plugin.

<!--break-->

The goal of the Jira plugin is to affect the parent task\'s remaining estimate and time spent fields when corresponding fields of subtasks got updated. I did the initial setup and learning to write Jira plugin that week. And after finally returning from leave and after the hustle bustle of voter ID project, I finally completed developing the plugin. In subtask type issues, remaining estimate and time spent can be get updated via two ways - either by \'log work done\' or by \'edit\' issue. JIRA has a powerful and flexible extension mechanism. All I needed is to catch an ISSUE_WORKLOGGED or ISSUE_UPDATE type event and update the corresponding fields of parent task according to the subtask values. Actually a listener class (listeners are custom classes which are called when any specific event occur in JIRA, custom listeners can be registered via JIRA administration panel) would suffice for this, but I wrote it in a plugin style for convenience (after all when there exists a nice JIRA plugin development kit and Maven-Atlassian-Idea plugin to setup the plugin project in our so familiar IntelliJ Idea IDE.) For using the JIRA plugin development kit you just need to have JDK and Apache Maven.

As I said above, extending JIRA is easy and visit [this](https://developer.atlassian.com/jiradev/getting-started) if you are interested.