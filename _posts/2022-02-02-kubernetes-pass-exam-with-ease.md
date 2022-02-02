---
layout: post
title: "How to pass Kubernetes exams with ease*"
categories: certification
author: Vic Perdana
---
There are numerous articles / blog posts providing countless tips to pass your CKA / CKAD exam. A quick Internet search "tips to pass CKA CKAD exam" will render a total of 39,100 results under 0.45 seconds.  There are many awesome tips you can find to pass the exams and I am sharing my personal journey achieving these certs as well as understanding the "why" of these exams which helped me appreciate the varous concepts introduced in Kubernetes. 

I recently passed CKAD and CKA exam with score of ~90 on a 1st pass :raised_hand: and while they're not perfect, I am glad to have achieved the scores with relative ease and I'd like to share how I prepared to get there; spoiler alert, preparation takes time and grit; especially if you're new to the world of containers.  

Side note: Even though I haven't taken the CKS exam, I would highly suspect you can use this advice too... and if it changes, I'll update the post :wink:

* TOC
{:toc}

> # Begin with the end in mind

If your objective is to only pass the exams and move on, you can do lots of practices and go ahead and do the search above. While you can pass Kubernetes exams this way, you may do yourself a bit of disservice given the amount of time and effort you've invested to achieve these certifications.  If not, continue reading :smile:


# So, what's the point of this post

The purpose of this post is to encourage you to get intimate with the various concepts introduced as part of the exams which in turn will make your exam experience less daunting - also, passing the exams should not be the end goal, but rather a stepping stone for you to become a subject matter expert in Kubernetes.  

There is a reason why Kubernetes (shortened to K8s) is so popular. Based on the 2020 CNCF [survey](https://www.cncf.io/wp-content/uploads/2020/11/CNCF_Survey_Report_2020.pdf), the use of containers in production has increased by a whopping 300% since 2016.  And 91% of respondents report using K8s with 83% of them in Production. With this massive growth, there must be a reason why it's such a popular choice for orchestrating the ever growing containers.

## Containers

![Containers](/assets/images/containers.jpeg){: width="800"}

K8s will not be born without Containers and likewise, containers would not be as popular without the use of K8s.  With profileration of complex apps - very rarely you'd see container is operating in isolation.  Apps have many components consisting of 2-3 or more tiers with their unique requirements. 

Starting from Physical servers, this creates minimal efficiency, every server is designated for a single operating system which can host possibly handful of applications and more often than not it's for a single application - just imagine, one whole physical unit which takes time to rack and stack is only designated for a single web server.  What about 3-tier application with highly available requirements. 

virtualization brings in additional optimization where you split physical server resources into logical units called Virtual machines.  Each virtual machine hosts its own operating system.  The main benefit with this approach is that there's a little barrier of entry to traditional deployment since you can bring the entire OS with its application and transition them to a deployable unit of virtual machine aka. P2V - Physical to Virtual.

Moving forward, the world's IT infrastructure is filled with (lots of) virtual machines on-premises and the cloud and given the popularity of the cloud, it triggers many companies to break in and compete in the cloud space.  One technology that came about was containers, popularised by Docker. Containers in some ways are similar to VMs, but they increase the level abstraction by sharing properties of operating system to host the application.  Similar to VM, containers have their own filesystem, allocated CPU and memory, and others. One of the benefits with this approach is that containers are much more portable than VM as they have independent properties from the underlying infrastructure.  Finally, considering how lightweight containers are, they allow infrastructure operators to achieve much higher density.

<figure><a href="https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/"> <img src="https://d33wubrfki0l68.cloudfront.net/26a177ede4d7b032362289c6fccd448fc4a91174/eb693/images/docs/container_evolution.svg" alt="What is Kubernetes" width="500"/> </a>
<figcaption>Source: Kubernetes official documentation: What is Kubernetes</figcaption>
</figure>

## Orhestration to the rescue

Remember the early days of Virtual machines, apart from the greater flexibility it provides, there's a lot of hype about how you do capacity management correctly - or otherwise, oversubscription occurs!  This is not too dissimilar to container world, except that things are a bit complicated.

<figure><a href="https://docs.microsoft.com/en-us/dotnet/architecture/cloud-native/leverage-containers-orchestrators#what-are-the-benefits-of-containers-and-orchestrators"> <img src="https://docs.microsoft.com/en-us/dotnet/architecture/cloud-native/media/cloud-native-design.png" alt="Microservices architecture" width="500"/> </a>
<figcaption>Source: Microsoft docs: What are the benefits of containers and orchestrators</figcaption>
</figure>

Welcome to microservices, key aspect of this architecture is that you break down a big monolith archiecture into multiple single-purpose component.  While this allows you to decouple a big singular architecture, it creates interesting dilemma, things can be more complex due to 1) number of components (and containers) that is increasing and 2) the need for these containers (in different microservices) to communicate and 3) the need to manage all of these!

With this in mind, container orchestration such as K8s provides the whole suite to manage these growing number of containers, such as how to mantain the availability of the applications, how to respond to spikes in load, secure sensitive information used by containers, etc. 

Want to find out about the history of containers and Kubernetes - check out this documentary ([part 1](https://youtu.be/BE77h7dmoQU) and [part 2](https://youtu.be/318elIq37PE)).

# Thanks for the backstory, tell me how to pass the exams!

Sure Jose, I understand you're so eager and after all, you're going to take these exams or at least thinking about it.  With any professional exams, there's always one thing called the [syllabus](https://en.wikipedia.org/wiki/Syllabus); so I would recommend to first to go ahead and review them (for both the CKA and CKAD exams).

## Review the syllabus
CNCF refers this as "curriculum", you can find these [here](https://github.com/cncf/curriculum). 

## Scoping your study
You will find that unfortunately CNCF does not provide supporting relevant links to documentation to scope the study so go ahead and review [CKA](https://github.com/leandrocostam/kubernetes-certified-administrator-prep-guide) and [CKAD](https://devopscube.com/ckad-exam-study-guide/) links.  

By the time you are reading this post, you may find they are out of date, so always refer to the official curriculum and as a ProTip: copy and paste the topics and search for the associated docs in the [official Kubernetes documentation](https://kubernetes.io/docs/home/). 

## Exam Format
If you have done some IT certifications or have been around in the industry, you may be familiar with [professional IT exams](https://www.roberthalf.com/blog/salaries-and-skills/which-it-certifications-are-most-valuable); most of the exams are usually consisting of scenarios with multiple choice answers.  Fear you not, this is different and it's entirely performance based aka. hands-on, you will be given a set of tasks or problems and access to a console with virtual environment for you to play and solve the problems under limited amount of time; so time management is critical. You are allowed to view certain resources during the exam so if you need to recall specific commands, you can refer to K8s docs to help you crack the problem. 

Refer to [candidate handbook](https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook/exam-rules-and-policies) for more information. 

## Learn at your own pace
At this point, I would take some time to learn every single topic from the curriculum.  You can use various on-demand courses such as *[the one](https://kodekloud.com/learning-path-kubernetes/)* most people are recommending (I personally use this) - which can also be accessed via UDemy.  You can get similar lessons from [Pluralsight](https://app.pluralsight.com/explore/certifications/topics/kubernetes), [ACloudGuru](https://acloudguru.com/training-library/kubernetes-training), [Linux Foundation](https://training.linuxfoundation.org/training/kubernetes-fundamentals/) etc. 

The main thing is that you spend the time exploring the concepts as well as applying those concepts in practice. 

*Expected duration: 1-2 months*

### K8s the Hard Way

If you're new to Kubernetes or simply curious about what makes up K8s, I really recommend going through [K8s the hard way](https://github.com/kelseyhightower/kubernetes-the-hard-way) *at least once* - as noted by Kelsey, whilst this is a great resource for study, you would not want to run this in production but use this as a playground to get more intimate with Kubernetes. 

Getting it right the first time is not the purpose here, in most likelihood, you may not get it right the first time (hence the title!) and the time you spend on *diagnosing, troubleshooting, and fixing* is the most valuable thing you get from this exercise. 

*Expected duration: 1-3 days*

## A few days before the D-Day

Crack open exam simulator from Killer.sh.  You should get two tries with each lasts for 36 hours. A side note, whoever came up with the name deserve a medal! The way I leveraged this simulator is as follows.

- A week before the exam, activate the first slot - this will be active for 36 hours, don't worry about time or score initially, but rather try to resolve the problems as much as you can
- Rest :sleeping:
- The next day, go for it again, this time try to time the simulation.  This is to introduce you to exam-like situation

*Expected duration: 1-3 days*

## Waiting for the result 
Previously it could take up to 72 hours, you heard me right, after finishing these intimidating exams, it seems like they deliberately after-exam suspense putting you on the edge. 

Voila! :sunglasses:

![CKA](/assets/images/CKA_cert.png){: width="500"} 

![CKAD](/assets/images/CKAD_cert.png){: width="500"}

# What's next
It's time for you to show off your achievements & relax a bit.  These certs demonstrate to your employer or prospective employer your practical ability on basic / intermediate Kubernetes concepts - but this should be seen as a stepping stone to immerse yourself in the world of K8s.  

> Just a word of warning, running and maintaining K8s can be proven complex and therefore leveraging services provided by cloud providers such as [AKS][https://azure.microsoft.com/en-au/services/kubernetes-service/#overview) and [Container Apps](https://azure.microsoft.com/en-us/services/container-apps/#overview) - check them out!
