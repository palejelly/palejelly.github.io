---
layout: post
title: Course Advice - Application Security (CS 9163)
comments: true
image: https://jonathanmann.github.io/public/img/appsec_header.png
excerpt: For those in the Computer Science / Cyber Security Masters programs at NYU, Application Security (CS 9163) can be among the most challenging as well as the most rewarding experiences of the curriculum. This post is about what to expect from the course and how to prepare so that you can get the most out of it.
---
For those in the Computer Science / Cyber Security Masters programs at NYU, Application Security (CS 9163) can be among the most challenging as well as the most rewarding experiences of the curriculum. This post is about what to expect from the course and how to prepare so that you can get the most out of it.

![APPSEC](https://jonathanmann.github.io/public/img/appsec_header.png)

### Overview
Hopefully this post will help you get through Application Security (CS 9163) with minimal bumps and bruises and also help you get the most out of the course. From a practical standpoint, it might just be the best class I've ever taken. The subject matter is extremely useful for real-world problems, and I wish every developer could internalize its lessons. The course material is subject to change from one semester to the next, but the principles should remain constant, so I'll try to write more to the concepts rather than the specific technologies. Still, you should check with the latest syllabus to try to familiarize yourself with the course technologies before signing up for the class. 

### Prerequisite Knowledge
Before signing up for the class, there are a few things you'll definitely want to be solid on. If any of the items listed below look unfamiliar, take some time to get comfortable with them. You probably don't need to know everything I've listed below, but the more you know going into the class, the less you'll have to learn as you go.

#### Programming
First, you really need to feel comfortable programming in any C-family language. You don't need to memorize all the APIs, but if you can only write a program to insert a flat file into a database in python and think it will take more than just a few minutes to write an equivalent program in C or Java, you should probably get some more experience under your belt before signing up for this class. The coding assignments are straight-forward if you know what you're doing, but you need to be able to read and understand code that someone else wrote in a variety of different languages. Getting to this level isn't as hard as it sounds, but it does take practice. Once you're comfortable with two or three languages, picking up another should be a piece of cake. If you've only ever used python, go rewrite a simple program you've done before with javascript. See, that wasn't so bad, right?

#### Software Engineering
You'll learn a lot about how to be a better software engineer during the course, but you will want to at least have a conceptual understanding of version control, CI / CD, testing, and other software engineering best practices.

#### Databases
Know the basics of how relational databases work and how to use SQL query from them. Your applications will be reading from and writing to databases through out the class and it will be critical for you to be able to check your work by writing simple queries. If you understand the syntax below, you'll probably have most of what you need:
```sql
SELECT * FROM CUSTOMERS WHERE LASTNAME = 'Mann' AND CITY = 'New York';
```
Find a tutorial and get some practice with SQLite if you've never used SQL before. All the better if you also learn about Object-relational mapping (ORM).

#### Virtualization
You don't need to know everything about virtualization, but you should probably understand what it is and how to set up a VM. If you want practice, set up a Kali Linux VM and play around with it a little bit. Having a VM with Kali will be very useful in general throughout this program.

#### OS Concepts 
It should go without saying that you absolutely need to know how to use a computer for this class. This includes, but is not limited to: navigating a file system, basic shell scripting, linux command line operations, file system permissions, and using an editor like vi when you ssh into virtual machines. You don't need to be a master, but if you don't know how to log into a remote system and edit a config file, you'll have a lot of learning to do.

#### Web Technologies
Again, you don't need to know everything here, but you'll really want to know things like how to send a POST request and how to parse HTML. A great tool to play with before taking the course would be [OWASP ZAP](https://owasp.org/www-project-zap/).

### Course Structure
The good news is that grading is assignment-based, so there are no high-pressure tests where a single night of bad sleep can shipwreck your entire semester. The bad news though is, the assignments can be very demanding so get started on them as soon as you can and reach out to the professor, TA, or your classmates when you get stuck; you can't afford to lose a week hoping the answers will come to you. Your mileage may vary, but, if you're well prepared, you should plan to spend about 40 hours of solid work on each of the four assignments (160 hours for the semester) with some taking less time and others taking more time. Budget your time accordingly. It's really not so bad if you work consistently, and the format is ideal for those who prefer learning-by-doing.

#### Assignment Structure
For each assignment, you'll need to identify the vulnerabilities within an application and then fix them and provide a write-up of what you did. It might sound straight-forward, but reading someone else's code and finding the bugs can be a challenge if you're unfamiliar with the underlying technology (or even if you are). The process reminds me a lot of CTFs. If you like those, you're going to love this class. Throughout the course, emphasis will be placed on using software engineering best practices such as version control and CI/CD. Take the time to do things the right way the first time and you'll be a better developer for your efforts.

#### Course Components
This might be different for you, but, when I took the course, it was broken out into the sections listed below: 

* Memory Management: discover vulnerabilities via memory leaks / buffer overflows as well as other common problems - (c / git / travis-ci)
* Web Security: explore frequently exploited web vulnerabilities such as XSS, CSRF, and SQLi - (python / django)
* Microservice Architecture: a great toy model of the vulnerabilities that come with app deployment at-scale - (kubernetes)
* Mobile Security: a brief survey of how your personal tracking device is used to spy on you for fun and profit - (android / kotlin)

#### Lectures, Readings, and Office Hours
The prerecorded lectures go along well with the assignments and provide a good jumping off point. The lectures help tie everything together and relate the material to real-world applications. Personally, I didn't read _ALL_ of the suggested material, but I did find some of it helpful when I got stuck, particulary around memory management and side-effects. For me, the best part of the class was office hours. Your fellow students will ask questions that will raise insights that you might have overlooked. In my section, we had a great TA who provided excellent tips, not only for working on the assignments, but also for solving similar problems in the industry using tools like [OWASP ZAP](https://owasp.org/www-project-zap/), [Valgrind](https://valgrind.org/), and [GDB](https://www.gnu.org/software/gdb/). Finally, if you have a choice in the matter, sign up for the section with Kevin Gallagher. He has the ideal pedagogical philosophy (hands-on learning), a passion for the material, and, best of all, he hasn't burned out yet. 

### Conclusion
AppSec might just be the most valuable class I've ever taken, but I had the right foundation going in and was able to get the most out of it. For those who like learning-by-doing, the course can be mind-blowing. If you're well prepared and you're willing to put the time in, I think you'll have the same experience.
