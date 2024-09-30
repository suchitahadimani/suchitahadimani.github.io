---
layout: page
title: Xpense
description: Maximize your credit card rewards!
img: assets/img/xpense.jpg
importance: 2
category: hackathon
related_publications: true
---

## XPense
Tired of remembering which cards to use when to maximize your rewards? Don't worry, XPense will automatically choose the best card for you.

### Our timeline —----------------

#### Friday —

We arrived at the venue at around 8 PM. By the time we attended the opening ceremony and grabbed dinner at the FIU dining (which they claim to be the #1 best dining hall), it was around 10PM. We walked around thinking of ideas for our project. I really wanted to do something related to Google Maps and location data. However, we decided to go with automatically determining the credit card to use based on the type of purchase being made. 

I was honestly a little tired and nauseous so I slept around 11 PM. The rest of the team did too.

#### Saturday —

We woke up around 6-7 AM and got ready. I was still feeling a little bummed out for not using the google maps API, but then I realized one possible use case would be to use location data to determine what card to use. Essentially, if we detected that the user was near a gas station, we could send alerts telling them that a certain credit works better for buying gas. It won’t always be perfect though, of course. It could just be that the user is taking a rest stop or dropping by the gas station stores, but it would be a best guess notification service.

We grabbed breakfast and walked around campus a little bit. The more we walked around campus, the more it felt like a resort. I’m not very used to seeing palm trees all the time, everywhere. And lizards :(( So, it felt a bit strange to me. In fact, FIU’s buildings also felt very mall-like compared to other colleges I’ve been to. 

After our walk, we actually went to quite a few workshops. I usually don’t go to many workshops at hackathons as I prefer working on the project. I went to the Assurant, NVIDIA, Etherium, Google, and EmpowerHer workshops. Here’s a very brief summary of all the talks:

Assurant → Discussed TerraForm as IaaS. I joined half-way through so I don’t know the initial set, but I do recall making resource groups, resource plans, and using “terraform apply” to run our files.

NVIDIA → Discussed omniverse and using 3D modeling for simulations. Our group won the raffle at the NVIDIA workshop and got some pretty cool merch. I claimed a nice NVIDIA tote bag for myself. 

Etherium → Did a quick run-down on using Solidity to create a smart contract. The concepts were honestly rather confusing and I still don’t understand how blockchain works or what made it special.

EmpowerHer → Met some other girls at the hackathon and discussed building careers. It was super fun talking to everyone. 

Google → Did a quick leetcode problem and discussed technical interview prep.


We grabbed lunch and took another short walk before actually beginning the project.


—- 

We were struggling to figure out how to get NFC data. We knew there were apps which allowed for our phones to read NFC cards, but we were struggling to determine how to get it to our laptop. We were unable to find any APIs which would be able to carry the information from the phone to our web app. We almost abandoned the idea entirely. 

Somehow, we found a random github repo with a NFC reader through the web. This function allowed us to read NFC data on our phones from using Google Chrome on an Android phone. We quickly decided to deploy our website and do the demo from our phones.

Also, this is something we had quite a problem with when using external repositories in our project and then pushing to git. The following two lines became essential in removing the subdirectory with git on it.

> rm -rf /path/to/submodule.git

> git rm --cached path/to/submodule

Now having a basic app, we were still missing a lot of basic functionalities. I started off by setting up Auth0, which was supposed to be easy, but I had the route.ts set up in the wrong directory, which led to me sitting there for an hour confused and going insane. I, till now, prefer Clerk out of both PropelAuth and Auth0. 

We went out for dinner.

After setting up the authentication, I worked on getting the database set up. Since I already had played around with MongoDB, this time I worked with Firebase. After doing some initial setup, I went to sleep. I was originally planning on staying up all night and working on it, but doing back-to-back hackathons every weekend had taken its toll.

#### Sunday —

Woke up + got ready at 8 AM

The rest of the team had set up Supabase as the database instead. I wanted to work on the perplexity API calls, but it just wasn’t working for some reason and I couldn’t figure it out. 

It was frustrating having to constantly commit to github just so to test it on the web version on my phone. 

Using API keys → I didn’t actually use .env.local files last time and directly called the api keys in my files, but obviously that’s not good practice, so this time I used the .env file. I had some trouble figuring out how to get the key. I thought doing process.env.API_KEY would work, and spent a long time debugging things that weren’t even an issue. I forgot to add environment variables to Vercel. For some reason, I still couldn’t figure it out.

By now, I had realized I had made some serious mistakes with Next.js. I had set it up using app routers using both page routers, because different guides offered different solutions. I had just trusted them without realizing that it wasn’t just the same solution with different names, it was actually just two different ways to establish routing in Next.js. I didn't have this problem last time because I used the flask framework. 

The faulty routing created a lot of problems with retrieving user information after they had logged in to create the profile page. There were layout.js and layout.jsx files which were also conflicting. Sahil remade the entire website with proper formatting and then we were ready for judging. 

Judging went pretty well. Same as PennApps, the judging went on for longer than predicted. Closing ceremony was pushed back an hour, a little sad because we were looking forward to going to the beach for sunset. Alas.

Overall, very different vibe hackathon primarily due to the number of big tech companies. The workshops were thorough and companies were focused on recruiting while judging projects. Start-up companies tend to focus on how teams use and interact with their product and tend to hold more informal workshops. 

—---------

Overall, a very fun hackathon experience!
