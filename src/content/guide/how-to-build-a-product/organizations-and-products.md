---
title: Organizations and Products
columns: two
template: guide.hbs
order: 300
---

# {{title}}

So you're building a product! It's an exciting time: your idea is now coming
to life at scale. However, you may quickly realize that managing a "fleet" of
devices brings a new set of challenges to the table requiring specialized
tooling to help you provide the best possible experience for end-users of your
product.

Fear not! Particle is here to bring you the tools you need to scale up
from your 1 prototype device all the way to millions of units out in the
field.

For product creators like you with the intent to scale, Particle offers
a slightly different back-end architecture that introduces the concepts
of _products_ and _organizations_.

## Organizations vs. Individuals

Up until now, you've been an individual user of Particle. Your devices belong to
you, and you can only act upon one device at a time.

When you create an **organization**, you'll have a few additional important concepts available to you: **products**, **devices**, and **customers**.

![Organization architecture](/assets/images/organization-structure.png)

First, you'll set up an **organization**, the overarching group responsible for the
development of your Internet of Things products.

Your organization can have multiple **products**. Defining a product is what unifies a
group of devices together, and your product can be configured to function exactly how you
envision.

Each product has its own fleet of associated **devices**. Either a P0, P1,
Photon, or an Electron, could be used inside of each device.

**Customers** own a device, and have permissions to control
their device. You will define the extent of their access to the device when you
configure your product.

Your organization has **team members** with access to the dashboard.

It is important to note that *team members* and *customers* have different levels of access. For instance, only *team members* will typically be able to send an over-the-air firmware update, while *customers* may have the ability to control the product. These access levels will be controlled through the dashboard.

## Setting up an organization

To get started, log into your Particle Dashboard at [dashboard.particle.io](https://dashboard.particle.io) and click on the "New Organization" button that appears in the navigation:

![Create a New Organization](/assets/images/new-organization-button.png)
<p class="caption">Click the new organization button to start the process of setting up your organization</p>

If you have been granted access into the private beta, you should see a modal appear asking you to give us some basic information about your organization. You'll notice that this modal includes fields for adding a credit card. Don't worry! **Your card will not be charged**. Members of the private beta will have a 30-day trial period to use your organization dashboard. When your trial is up, we will begin charging $49 per team member per month.

![Create Organization Modal](/assets/images/new-organization-modal.png)
<p class="caption">Setting up your organization. Your credit card won't be charged until the end of your free trial</p>

Fill out all fields in the modal, and click **CREATE**. Congratulations! You now are the proud owner of a shiny new organization on Particle!

## Defining a product

Now that you have an organization, it's time to create a product.

Our cloud platform thinks that all devices are *Photons*, *Electrons*, or *Cores* — unless it's told otherwise. Now's the time to define your own product within the platform and tell us a bit about how that product should behave.

*Photons* are development kits. They're designed to be easy to reprogram and run a variety of software applications that you, our users, develop.

*Your product* is (probably) not a development kit. While some of the characteristics of the development kits will carry over, you're going to want to make a bunch of changes to how your product works. These include:

- Limiting access (e.g. only certain people can reprogram them)
- Collecting bulk data, events, errors, and logs from all of your devices
- Distributing firmware updates in a controlled fashion

Once you have an organization set up in the dashboard, you will be able to add a product and you will be walked through these decisions.

To create a product, return to your organization's products page and click on the **New Product** button.

![Your organization's product page](/assets/images/products-page.png)

This will open up a modal where you can add basic details about your product:

![A new product](/assets/images/new-product-modal.png)

You now have your very first Particle product! Woot!

After successfully creating your product, you will be directed to your product's configuration page.

## Configuring Your Product

As a product creator, there are some key decisions you will need to make before devices are shipped to customers. Your configuration page will walk you through key questions that you should be thinking about during the development process. **You don't need to know the answers to all of these questions right now.** You are always able to return to your configuration page to answer outstanding questions, or change your answers. However, you **must** answer all questions before you can start manufacturing.

It's also worth mentioning that some of the questions asked on the configuration page have tangible impacts on how your product will function within the Particle ecosystem, and others are simply educational to encourage you to be thinking strategically about what needs to happen before your product goes to manufacturing.

There are four main sections to the configuration page: *Overview*, *Working with Particle*, *Customers*, and *Firmware*. A few questions to highlight here:

* **Authentication/Logging in with Particle**: Thinking about how you would like to handle authentication is one of the earlier decisions you should be make as a product creator. There are three options for authentication: *simple auth*, *two-legged auth*, and *login with Particle (oAuth)*. Each option is explained in detail [later](#managing-customers). Picking an authentication method will likely depend on whether/how much you would like Particle to be hidden from your customers, as well as your development's team appetite for complexity.

* **Private Beta**: Do you only want a select group of people to use your product, inviting them as part of a private beta? This is likely a good idea if you would like to run a controlled test for your product. As a manager of a private beta, you will import a list of customers you would like to participate, and each one will be assigned a 4-character activation code that they will need to claim their device during setup.

* **Programming the product during manufaturing**: You can either program each device while they are on the manufacturing line, or send an OTA update to the device on customer setup. The main advantage of programming on the line is that the device will function immediately, instead of requiring the customer to be in range of Wi-Fi when they unbox the device. However, programming on the line will require your binary to be locked down and finalized before manufacturing begins. Programming the device on setup will allow you to continue developing the firmware for your product in between manufacturing and customer unboxing, providing additional flexibility. But, the device will not function properly until the customer connects the device to the internet and receives the OTA.

![Configuration page](/assets/images/configure-page.png)
<p class="caption">The configuration page will identify key decisions you will need to make before manufacturing</p>

