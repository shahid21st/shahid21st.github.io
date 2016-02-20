---
layout: post
title: "Writing JIRA Plugin"
date: 2007-09-24
tags: [tag1, tag2]
---

After taking a long leave for my M.Sc. exams, I had to come back to office for one week in halfway of my leave. The reason was to extend the leave by one week at the end as two of my exams were postponed. Returning to the office, I found that I lost my seat, my CPU was in the kitchen and my monitor was used by a newcomer. With the kind help of all, I placed my one week post at a corner in the midst of the testing team. (That was not a bad place at all, except the daily scrum of testing team when I felt so much in a 'wrong place').
In that time Mehrin apu was kind enough :) to assign me a work and that was writing a JIRA plugin to affect the parent task's remaining estimate and time spent fields when corresponding fields of subtasks got updated. I did the initial setup and learning task that week. And after finally returning from leave and after the hustle bustle of voter ID project, I finally completed developing the plugin. In subtask type issues, remaining estimate and time spent can be get updated via two ways - either by 'log work done' or by 'edit' issue. JIRA has a powerful and flexible extension mechanism. All I needed is to catch an ISSUE_WORKLOGGED or ISSUE_UPDATE type event and update the corresponding fields of parent task according to the subtask values. Actually a listener class (listeners are custom classes which are called when any specific event occur in JIRA, custom listeners can be registered via JIRA administration panel) would suffice for this, but I wrote it in a plugin style for convenience (after all when there exists a nice JIRA plugin development kit and Maven-Atlassian-Idea plugin to setup the plugin project in our so familiar Idea IDE.) For using the JIRA plugin devlopment kit you just need to have JDK and Apache Maven.
As I said, extending JIRA is easy and I want to share some links if anyone is interested in extending JIRA in the future:
JIRA extension overview
JIRA plugin guide
JIRA plugin development kit
JIRA development hub - very useful place to find useful documents about JIRA development and also code sample to do basic operations with issues like ' creating and editing as issue'.
JIRA plugin library - repository of existing JIRA plugins.
Note: Don't forget to see the comments at the bottom of the JIRA confluence pages, you will always find there some solution of your deployment or setup problems to develop a plugin.
