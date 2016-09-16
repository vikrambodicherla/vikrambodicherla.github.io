---
layout: page
title: The Protean UI
---

The institution of family is the nucleus of society. For a civilization to succeed, it is crucial that families succeed. How and why families succeed has been the subject of a lot of research over many decades. And while successful families display multiple qualities, spending high quality time together finds it’s way on more or less every list. But today's life with all its complexities doesn't leave a lot of time for a family to spend together. This doesn’t bode well for young children, especially in the earlier ages, because kids spending quality time with their parents is essential to their growth and well being. 

Optimizing one’s time to have more family time is a multi-faceted topic and I am interested in one facet: smartphone distractions. All members of the family, including young kids, are constantly leashed to their smartphones, reaching out to their devices the moment one has a spare second. Smartphones also constantly distract people from what they are doing via notifications. The smartphone is taking away heavily from one’s presence at family time, thereby adding to the increasing divide in the family. 

I am interested in understanding and exploring ways of reducing smartphone distractions in young families by optimizing the smartphone operating system (OS) user interface (UI) for this purpose. I don’t believe that changing the software will fix the problem entirely. We need a buy-in from the user. But I do believe that there are some optimizations that can have a great impact.

## Nature of smartphone interruptions
The smartphone ecosystem is one of constant interruptions and distractions. Research says that their very presence tends to distract a user [[1]](http://www.ncbi.nlm.nih.gov/pubmed/26121498), even if the user doesn’t interact with the phone. There are two ways in which smartphone sessions start: 

1. Session initiated by the user: this happens for a variety of reasons from seeking specific information to  avoiding people around you to boredom.
2. Sessions initiated by the smartphone: smartphones call for attention using a variety of notifications including sound, colored lights etc. This is usually done to bring the user’s attention to data update within an application.

The sessions starts with the intended action - either accessing a service or responding to a notification - and often results in sessions going beyond serving the original intention. As applications get more and more sophisticated, they do a better job of surfacing content that users would enjoy next. It's safe to assume that for the duration of the session, the user is much less engaged by the real world, the people around him. And in the context of our problem, the family.

I think the smartphone OS can play a better role in stopping these run-away sessions, specifically when the user is in the company of his or her family and all the more if he or she is actively engaged with the family. The smartphone OS can act as a personal assistant of the user, encouraging interactions only when the benefit of the session outweighs the lost family time. Applications will now need to petition to the smartphone OS to gain access to the user's attention.

Such options exist today: many smartphone providers provide a no-disturbance mode. But turning this mode on and off is fairly manual and the effect is, by default, all or nothing. Configuring the option per application is pretty laborious. We want to build a UI that evolves and morphs as the user goes about his or her day. 

## The Protean UI
I propose building an Android OS UI, named the Protean UI, which aims to minimize distractions to maximize time spent with one's family.

The Protean UI, based on the 
Gate notifications: notifications, when blocked appropriately, result in lesser sessions
Modify the home screen and the list of all applications: not all applications need to be available handily at all times. Think of it like health-conscious buffets which use smaller plates and have the desserts at the place with the least visibility and access
Educate users on their usage trends: we can't force such changes on to a user, we'll need a buy-in from them. And this means educating the user

It aims to do this with the knowledge of what members of the family are doing at a given point in time I.e. their User context.

User context is defined as

* location: GPS and indoor positioning
* activity: physical activity (is the user moving or stationary? Watching TV? Talking to a family member?) and the activity on a smartphone (which app is the user using?)
* company: is the user near another family member?

### Capturing user context
There are signals being generated from many sensors and sources around the user. Some are strong and explicit, like the GPS and accelerometer and some are weak and implicit, like a calendar invite to the user or a television turned on. Other users and devices in the user's surroundings (virtual and physical) generate signals about the user as all. All these sensors can help paint the picture of User context.

The system we need to build

* should be able to process multiple streams of data and combinations of streams of data from a variety of sensors, either from the user and otherwise, emitting at different rates in real-time
* is adaptable: streams can be added or removed as they become available or unavailable

I propose employing a realtime stream processing (RTSP) cluster like Spark residing on the Internet to do this. We will have one cluster per family, consuming data streams and deriving user contexts for all members of the family.

![GitHub Logo]({{ site.baseurl }}/public/protean_rtsp.png)

All user will have an Android application installed on their phone that acts as a transmitter of sensor data from the smartphone. Sensors installed in the environment will also transmit data to the RTSP cluster.

This stream processor will derive the user context and keep updating it as it receives information. It will also preserve a time series of user context information for all users.

We will have a secure API that can be query for user context information for a given user for a given range or point in time.

Beyond what I have described here, there are many design and implementation details. The biggest thing would be designing a taxonomy that can be used to describe events so the relevant processing units can register interest in it. There is also the ever-looming issue of privacy and security of the user and the family given the nature of the data. These are details that need to be worked out.

### Using user context

#### Gating notifications
We use the data collected via the API and start gating notifications based on our understanding of what’s important to a user in that context. The rules employed can either be either based on heuristics or can be derived by identifying notifications that the user responds to that results in the most amount of time taken. The exact details need to be worked out.

#### Modify home screen and list of applications
Based on the genre of an application and the user's usage pattern, we can figure out if it's worth displaying an application up-front on the UI or if it can take a backseat. Likewise for widgets, based on the user context and their importance, they can either show or hide. 

#### Education users
We can use the application installed on the user's smartphone to deliver the educational tips. Using the user context time series of the family (available via the API), we can find instances in time where the family could have spent time together, but ended up not doing so because they were engaged with the smartphone. And using this data, we build up visualizations on the smartphone app installed on the user’s phone.

When applications have access to this data, they can function better within their arena as well: they can personalize better. This is however complex given that there will be tracking and security concerns. 

## Project plan
There will be three stages, development, testing and productization.

* Development will involve building up the infrastructure along with the ability to push software updates OTA. The system will be built in a generic way so we can deploy sensors and test various data streams as they come in.
* Testing will include testing out various sensors and processing streams and deriving context information. This will likely partly be in the lab and partly in the field. 
* Deployment will involve pushing out a product and testing it on the field and collecting data to validate the concept: are we really making a difference?

At the end of this effort, I hope to have a platform that will be scalable and extendible. And that it will lead to building software which knows how not to interrupt us from having real world experiences. And will hopefully lead us to better engaged families.