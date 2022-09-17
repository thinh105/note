---
id: pn7d793ekr5h28ajhykwj5y
title: Successful Stories
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
title_imported: Successful Stories
updated_imported: '2022-05-08T16:24:22.000Z'
created_imported: '2022-05-08T04:32:31.000Z'
---

[TOC]

# https://phong.medium.com/how-i-get-an-offer-from-facebook-eb3a1ce65939

# [My Approach to System Design](https://www.teamblind.com/post/My-Approach-to-System-Design-V4SJARdx)

Facebook  CygnusX1
  155 Comments
It is not an exhaustive list and might not work for everyone. Also it is not a one size fits all thing but I hope it helps you to draft a plan and figure out how to tackle the sys design.

Resources:
1. https://github.com/donnemartin/system-design-primer
2. https://github.com/binhnguyennus/awesome-scalability
3. Youtube InfoQ channel - https://www.youtube.com/user/MarakanaTechTV
4. Youtube SDE Skills channel - https://www.youtube.com/channel/UCPumyEKs86w-GtWDd2XQYtg
5. Amazon DynamoDB - https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf
6. Grokking the sys design interview - https://www.educative.io/courses/grokking-the-system-design-interview

Process:
- I started with reading Amazon DynamoDB paper. This is a very practical paper to understand consistent hashing.
- I also started spending time on Donne Martin Sys Design (resource 1) and tried to go through each section. I went through this page 2-3 times entirely to develop some understanding (my philosophy is to read one book 10 times than reading ten books 1 time). I was not just reading it page but making my notes summarizing each building block, its usage, common systems where it is used and its drawbacks.
- For each component (whether it is a caching, messageQ, DBs) I searched for Youtube videos and gathered more info on it. I penned it down in my notes, copied any diagram I found its usage in, stored links to videos or blogs I saw mentioned something useful about it.
- I started reading cloud design pattern - https://docs.microsoft.com/en-us/azure/architecture/patterns/ - It has some good info on what the nomenclature is and how some components are typically used.
- I started going over practical system designs - Read mostly from Uber blog, Facebook blog and Yelp architecture.
- I read about few system design practical questions and analyzed what sort of things I need to address there - Grokking the sys design and Donne Martin has some good examples on these.
- Before each interview I only referred to my notes on various components/building blocks instead of researching again.
- Finally I made sure I practiced on whiteboard with solving 2-3 sys design problems. Idea was to complete them in 45 mins (more on this below).

## My 9 Step approach

1. Gather requirements(use case, who is customer, why is this needed etc.)
2. Discuss system constraints (any limitations, what is allowed vs what is not)
3. Do capacity estimation, specifically
- traffic estimation (read request per sec, write requests per sec)
- storage estimation (storage needed to store worth 3 yrs of stored 'object')
- bandwidth estimate (#of bytes/sec system should handle for incoming and outgoing traffic)
- cache estimate (memory needed to cache some of the hot read responses, 80-20 rule)

4. Define System APIs - Rest style mostly (read about Rest vs Soap)
5. Draw top level system diagram (client, web servers, platform, database, worker services)
6. Discuss database design choice (schema, SQL or no-SQL)
7. Perfect your design for a single user -> get a Minimum Viable Product
8. Discuss scaling
- find bottlenecks and single point of failures (put load balancer, caching, replication, Message queues, Asynchronous workers)
9. Test and Review your design (Treat this one as same what you do in coding interview)
- walk through your system and see if we met each customers need
- did we provide APIs for each customer ask
- did we walk over failover scenarios (not just vanilla passing case)
- did we draw system boundaries/or blocks to explain different parts of systems

If you manage to get all these steps in 45 mins you've probably addressed most of the interviewer concerns :)

Some practical examples (see if above 9 steps are followed here):
Design Tiny URL: http://tinyurl.com/jlg8zpc

Design Instagram: https://www.educative.io/collection/page/5668639101419520/5649050225344512/5673385510043648

Some tricks you'll need:
1. You need to know some common sys design patterns. If you tell I'll solve this by using 'Consistent Hashing' you don't have to waste your time and explain this whole thing on whiteboard. Interviewer can also see you know the common industry stuff and where to use it. Just say the name and keep moving.
2. When interviewer interrupts you in your design and asks a question, don't be defensive and start proving what you've done is correct. You might be correct or you are being asked to suggest alternatives for the design choice you made - knowing what building blocks exist helps here.
3. Try to reach to step 8 in the 45 mins discussion. Have some rough diagram on board like below to convey what you are saying (not just doing hand wavy stuff)

What didn't work out for me:
In my last Google(L5) interview the 9 step process I listed above didn't work out. First the interview was a virtual one so it was getting very difficult for me to draw the diagram on screen. Secondly the interviewer was getting anxious and kept interrupting me to get to the scaling part in step 5 itself. I kept reminding him I'll answer this in just a moment. The final feedback I got from recruiter was Interviewer gave me hints to solve the problem :)

What worked for me:
In my FB sys design I just followed these 9 steps and interviewer didn't interrupt me anytime. He probably only spoke handful of words apart from his intro during the whole interview :)

#swe #interview #software

