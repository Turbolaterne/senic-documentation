.. _main_index:

*********************************
Senic Hub Technical Documentation
*********************************

:Author: Senic GmbH
:Version: |version|


What is the Senic Hub
=====================

The Senic Hub is a **Bluetooth, Bluetooth Low Energy and Wi-Fi-enabled smart home hub** that allows a user to connect to their smart devices (such as Sonos, Philips Hue etc).
It also works together with the `Senic Nuimo <https://www.senic.com/en/nuimo>`_, our very own BLE controller for smart devices and significantly extends its usefulness by eliminating the need to having it connected to a smart phone or tablet.

.. pull-quote::

    The software stack powering the hub is not only entirely built on many great open source projects (surprise!) but we also decided to open up our own stack from the very start -- and this here is its documentation.



Why We Built the Senic Hub
==========================

The `Senic Hub <http://blog.senic.com/posts/what-were-building-next>`_ is the first major step in Senic’s vision to make technology that is `not focused on ‘stickiness’ <http://blog.senic.com/posts/the-problem-of-attention>`_ but on `wellbeing for the human <http://blog.senic.com/posts/design-for-wellbeing>`_.
At Senic, we see a big problem in companies creating apps and products that try to maximize the time that their users spent engaged with them.
Instead we would like to promote wellbeing by creating connected devices, experiences, interfaces and systems that provide users with seamless technological experiences, without constantly demanding or even just encouraging their attention or focus.


System Overview
===============

.. image:: hub-software-components.png
   :scale: 80 %


Decisions, decisions...
=======================


When initially brainstorming how to build such a product we knew we would need to make it unobtrusive and easy to use but also easy to develop for and easy to keep up-to-date so we were confronted very early on with many important technical decisions.
Some of these decisions were pretty clear or self-evident from the beginning, while others required a thorough understanding of both what our smart home users actually need but also what we are able to build with the fairly limited resources of a small startup.
So before we dive into the details of *how* we ended up doing these things, we would like to take the time and outline our reasoning behind some of these decisions and key insights, requirements and learnings.


Hardware Platform: NanoPi Neo
-----------------------------

.. image:: nanopi-neo.png
   :align: right
   :width: 103 px
   :height: 92 px

The Senic Hub is powered by the `NanoPi Neo <http://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO>`_, a tiny (4x4 cm) but powerful single-board computer equipped with an Allwinner H3 Quad-core 1.2GHz CPU and 512 MB DDR3 RAM.

One of the most decisive factors in favour of this board was its low price.
The other is the fact that it is actually *designed to be included in a product* -- unlike, say, more commonly known, *fruit-flavoured boards* such as the raspberry PI etc. which are explicitly targetted at hobbyists and students to experiment with.

However, we found that satisfactory runtime stability, heat dissipation etc. were basically not achievable with those offerings.

We ship it with a 2 GB high-speed memory card that stores the operating system, software stack and user data.
More importantly, we extend it with `carefully chosen <https://github.com/getsenic/wifi-ble-link-quality-benchmark>`_ high class **Wi-Fi, Bluetooth 3.0 and Bluetooth 4.0 dongles** to provide the best possible wireless connectivity within the given physical restraints.


Operating System: Linux
-----------------------

.. image:: linux-tux.png
   :align: right
   :width: 100 px
   :height: 118 px

For the Senic Hub, we wanted to use open-source components as much as possible, not just because we ourselves are avid users of and even contributors to `Free and Open-Source Software <https://en.wikipedia.org/wiki/Free_and_open-source_software>`_.
We also wouldn't be able to stand behind a product that runs 24/7 in the homes and workplaces of our users and which contains proprietary code whose actual workings could not be verified by ourselves or third parties.

.. note::

    During our evaluation phase we also considered using one of the many BSD flavours, specifically `FreeBSD <https://www.freebsd.org/>`_ because it has a proven track record in the area of stability and security plus a long history of running on the tiniest of platforms long before the term "Internet of Things" was even coined.
    We did however find that while the `H3 is pretty well supported <https://wiki.freebsd.org/FreeBSD/arm/Allwinner>`_, support for bluetooth is lagging behind and in the case of BLE currently non-existent and so we abandoned that approach.

This pretty much left us with Linux which offers a much broader support for such types of boards, most notably including BLE.

The NanoPi itself comes with `various flavours of Linux <http://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO#Software_Features>`_, however, even the "light" versions weigh in with 400Mb and they all are geared toward end users who wish to use this platform as their personal development or experimentation field.

In the end we opted for building our own custom distribution using the well-established `yocto project <https://www.yoctoproject.org/>`_.
This allows us to create a fine-tuned distribution without having to start from scratch and importantly we can still benefit from upstream mainline updates, be they security related, performance wise or new features.

Also, reducing the amount of code on the system as well as the number of running processes significantly reduces the attack surface for malware -- we definitely want to do all we can to avoid the Senic Hub becoming part of the next botnet!


Programming Language: Python
----------------------------

.. image:: python.png
   :align: right
   :width: 115 px
   :height: 112 px

Why did we decide that Python was the right programming language for the Hub?
Actually, a better question might be: Why *shouldn’t* we use Python?
If you take a deep dive into the open-source smart home world, you will find a number of do-it-yourself projects and Python is often the language of choice for these DIYers.
This adoption of Python is primarily due to the sheer number of ready-made libraries and Python’s availability for many different operating systems.

In addition, Python is an easy to learn and extremely powerful and expressive as a programming language.
The Python programming language is currently (as of June 2017) the `fourth most popular programming language in the world <https://www.tiobe.com/tiobe-index/>`_.
In the end it was important for us, to not just select a programming language that *we* were comfortable with but also for a significant amount of other people who then could contribute or simply hack the device or easily learn how to do so.


Home Controlling and Automation: Home Assistant
-----------------------------------------------

.. image:: home-assistant.png
   :align: right
   :width: 90 px
   :height: 90 px

Despite being a relatively new market, the *smart home* is already seeing tons of different connected devices such as speakers, light, switches, thermostats or electronic door locks from thousands of companies.
The elephant in the room, though, is that each of these devices is using different communication channels and protocols.
Keeping up with the sheer volume of new smart devices being launched is nearly impossible without a strong developer community.

`Home Assistant <https://home-assistant.io>`_ has exactly that.
It supports a massive number of devices and has established a large community of developers who contribute and improve the support for various smart devices.

One of the reasons it *can* support such a large number is its extremely well thought-out *modular structure*.
One of its core modules is a sophisticated *event model* and *state machine* that we can conveniently use for our own needs without having to re-invent the wheel.

Oh, and it, too, is written in Python.


Details, details
================

Below you can find links to the nitty-gritty details

.. toctree::
    :maxdepth: 3

    senic-hub/index
    senic-os/README
