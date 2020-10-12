---
layout: post
title: The decision to move away from Google Analytics
date: 2020-08-21T09:04:59.379Z
description: First, let’s go back in time to the early 2000s when companies
  first began to understand the importance of digital marketing. The first
  challenge was simple, but the hardest to overcome—how to get maximum return on
  your investment? When Google introduced its Analytics software in 2005 it was
  a real saviour that helped analyse which marketing campaigns were converting
  and the ones that needed to be reviewed. It quickly became the most widely
  used web analytics service.
image: /assets/uploads/ga-banner-image.jpg
author:
  - value: Vivek
    label: Vivek Varughese
tags:
  - database
---
***History***

First, let’s go back in time to the early 2000s when companies first began to understand the importance of digital marketing. The first challenge was simple, but the hardest to overcome—how to get maximum return on your investment? When Google introduced its Analytics software in 2005 it was a real saviour that helped analyse which marketing campaigns were converting and the ones that needed to be reviewed. It quickly became the most widely used web analytics service.

Google Analytics monitors the behaviour of websites in terms of session length, pages per session, bounce rate, number of visitors and traffic source information. Google Ads integrates with Analytics, enabling users to build and review online campaigns through the quality monitoring of landing pages and conversions (goals). It includes high-level dashboard details for users and, more detailed in-depth custom reports if required.

![How google analytics works ](/blog/assets/uploads/ga-pic.png)

***The concern with user privacy***

Google Analytics has some privacy concerns because to assess a user's geographical location, Google monitors the user's IP address. Its goal is to obtain maximum information from people, and from as many pages as possible and as a result, it supports them directly with Google Analytics. This allows them to target a user using Google ads which are very well collaborated with GA. We also tried the free product for some time as the team was engineering heavy and it was a default for most new websites. Though we quickly realised that it’s a vicious cycle and we didn’t want to contribute to it. In the spirit of an open-source company that is focussed on transparency to help in promoting, sharing and collaboration of data we decided that although it was important for us to understand the user flow of a potential user it was also necessary that unwanted information was not collected by a large conglomerate such as Google. This collection and monitoring of data did not match our principles and what we stand for, hence we began the process of finding a partner more aligned with our value systems.

***Improving transparency***

This decision wasn’t easy, as most other analytics software are not well tested or well-integrated with Google or Reddit ads etc. It would also become hard for us to check which traffic is performing better from a certain channel. This made it very hard for us to calculate the ROI of campaigns. This can become quite expensive for an early-stage start-up such as ours. But user data privacy is paramount and we tried software’s such as [Fathom](https://usefathom.com/), [Simple Analytics](https://simpleanalytics.com/) and finally narrowed down on [Matomo](https://matomo.org/) as they do not use cookie tracking and give full control on data ownership and privacy control. It also helped that they had a free trial for 21 days without requiring your card details which was adequate time for us to test the product.

Unlike GA, Matomo is GDPR and CCPA compliant, costs a fraction of what google charges, open-source which is important to us and their email support is quite prompt. As an added bonus, we also removed Hotjar from our website to be completely free from cookie tracking. Matomo has well-collaborated heatmaps which were also included in their plan. Few additional features were the ability to see the different levels of user interactions on the website which was very important to us. They also let you make custom reports which make it easier to view the exact data that is most suited to the company’s goals.

Although Matomo seems to work great for now, it is yet to help us understand the individual user funnel attribution, but we can live with that for now. Although down the line as traffic increases we might need to shift to using something similar to GA—which needs cookies to understand individual user behaviours—so as to improve our user funnel and how we optimise our website. If and when the time comes, this will be done with complete transparency.

The concern about privacy for consumers remains real and valid. As such, the balance between tracking consumer activity and respect for their privacy is paramount to us. This can be done by understanding precisely what information is collected, by taking action to anonymize the information and how you can opt-out. We are very open to any suggestions on how to best optimise as per user needs, without compromising on their privacy.