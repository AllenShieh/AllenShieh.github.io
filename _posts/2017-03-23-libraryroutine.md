---
layout: post
title: Library and Go to the Gym
tags: [diary]
image:
  feature: yourname.png
---

I think going to the gym has been a part of my life!

Recently I have been working on my graduation project. This project is basically an extension of my paper ([PDF](http://allenshieh.github.io/publications/icpads2016drone.pdf)), but to be more specific, my paper only ran simulations on the computer to simulate a virtual drone's flying process and find the virtual optimal relay location, and my graduation project would let this happen in real life, but with a 2D space, for I could borrow the clean robot from the lab.

Tons of problem rise up when it comes to the combination of software and hardware. I have to get familiar with every possible APIs I am gonna use. The algorithm I used in my paper is in MATLAB and the controlling of the robot is in Python, and I spent almost one week to find a potential way to call them in my C++ solution. The reason I use C++ as my basic implementation platform is that, in order to set up a network of video transmission, I would probably use socket library in Windows, and I have some experience in it when I implemented a simple chatter and the course exercise.

But what confused most was the choice of way to implement the three-end transmission. Bluetooth was to slow for video transmission and public network cannot meet the project's needs. I tried to search for WIFI-direct solution, but what is strange is that the sample codes from Microsoft were unable to run, actually, unable to build. Pfff, no comment on that, and feel sorry for my time and brain on working on it.

Finally, I used local hotspot, trying connecting the other computer to the central computer as the relay. It seems plausible, and it would probably satisfy the needs of the experiments. I am still working on it.

I find it useful to run in the gym if not in a good mood. :)
