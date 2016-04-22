---
title: Your Particle Dashboard
columns: two
template: guide.hbs
order: 500
---

# {{title}}



<div class="dashboard-teaser">
  ![Particle Dashboard](/assets/images/dashboard-teaser.png)
  <p class="caption">The Particle Dashboard has a suite of tools to make your life as a product creator easier</p>
</div>

## Introducing your dashboard

When you begin the process of manufacturing and distributing your product in
large quantities, you will likely start to ask questions like:

- _How many_ of my devices are online right now?
- _Who_ of my customers are using their devices, and who isn't?
- _How_ are my customers using their devices?
- _Which_ firmware version is running on each device?
- _What_ errors and exceptions are the devices generating?

Without the right tools to answer these questions quickly and accurately, the
health of your product and the relationship you have with your customers will be
put at risk.

Luckily, the Particle dashboard is designed to give you full visibility into the
state of your fleet, and provide a centralized control panel to change how
devices are functioning.

The first step to get started is understanding the differences between your
individual developer dashboard and the organization dashboard.

## Adding team members

Now that you have created an organization successfully, it's time to add your team members that are collaborating with you on your IoT product. Adding a team member will give them full access to your organization's dashboard.

To do this, click on the *team icon* (<i class="ion-person-stalker"></i>) on the sidebar of your organization's dashboard. This will direct you to the team page, where you can view and manage team members of your organizaiton. Right now, your username should be the only one listed as a member of the organization. To add a new team member, just click the *Invite team member* button pictured below:

![Team page](/assets/images/team-page.png)

Clicking this button will open up a modal where you can invite a team member by email address. Before inviting a new team member, **make sure that they already have a Particle account with the email address you will be using to invite them to the organization**. At this time, you are only allowed to belong to one organization. As such, the person you are trying to invite should not already belong to another organization.

![Invite team member](/assets/images/invite-team-member.png)
<p class="caption">The invite team member modal</p>

Once your team member is successfully invited, they will receive an email notifying them of their invitation. The next time they log into their dashboard, they will have the option of accepting or declining the invitation. Remember that after your free trial, Particle will begin charging $49 per team member per month.

Nice! Now you have an organization with a team.




## Adding Devices

Adding devices will assign them to your product in the Particle cloud, and allow you to start managing these devices within your product dashboard.

There are two main types of devices you should add to your product:
- [Test Devices](#test-devices): Your team's Particle development kits (i.e. a Photon) that you are
  using to get comfortable with how you will manage firmware at scale.
- [Production Devices](#production-devices): These are the real-deal devices, the ones that
  will be out in the field and/or in customers' hands.

**In either case, you should shoot to add all devices to your product
_before_ they are flashed with a firmware binary containing your
Product's ID.**

### Importing via the Dashboard

To add devices to your product, visit your [Particle
Dashboard](https://dashboard.particle.io), and
navigate to your Product's view.

Click on the Devices icon in your product sidebar, then click on the "Import" button.

![Your product's devices](/assets/images/devices-page.png)

To allow you to import devices in bulk, you can upload a file
containing multiple device IDs. Create a `.txt` file that contains all
of the IDs of devices that you would like to import into your product,
one on each line. [Not sure what your device ID
is](http://localhost:8080/reference/cli/#particle-identify)?

*You cannot register devices that have already been 'claimed' by someone outside of your team; all of these devices must either belong to a team member or belong to no one*. The file should look something like this:

```
55ff6d04498b49XXXXXXXXXX
45f96d06492949XXXXXXXXXX
35ee6d064989a9XXXXXXXXXX
```

Where each line is one Device ID. Once you have your file ready, drop it onto the file selector in the import devices modal. Before clicking import, note the checkbox that says *Force imported devices to switch to this product*.

Checking this checkbox will signal to the Particle cloud that regardless
of which Product ID is reported by the device's firmware when it comes
online next, it should be treated as your product. Your test devices are
likely photons that do not have your new Product ID compiled into its firmware. If this is the case, go ahead and **check this box**.

When you do a real manufacturing run and import those devices into the
dashboard, you will not need to check this box. This is because your
devices will receive firmware with your Product ID directly on the manufacturing line.

## Managing Customers

Now that you have set up an organization, your customers will be able to create accounts on the Particle platform that are registered to your organization. When properly implemented, your customers will have no idea that Particle is behind the scenes; they will feel like they are creating an account with *ACME, Inc.*.

There are three ways you can authenticate your customers:

- **Simple authentication**. Your customers will create an account with Particle that is registered to your organization. You do not need to set up your own authentication system, and will hit the Particle API directly.
- **Two-legged authentication**. Your customers will create an account on your servers using your own authentication system, and your web servers will create an account with Particle for each customer that is paired to that customer. Your servers will request a scoped access token for each customer to interact with their device. This is a completely white-labeled solution.
- **Login with Particle**. Your customers will create a Particle account and a separate account on your website, and link the two together using OAuth 2.0. Unlike the other authentication options, this option will showcase Particle branding. This is most useful when the customer is aware of Particle and may be using Particle's development tools with the product.

When you create your product in the dashboard, you will be asked which authentication method you want to use. Implementation of these methods are covered in detail in the [How to build a web app](/guide/how-to-build-a-product/web-app/) section of this guide.

As customers are created for your product, they will begin to appear on your Customers (<i class="ion-user"></i>) page. For each customer, you will be able to see their username and associated device ID. Note that the device ID column will not be populated until the customer goes through the claiming process with their device.

## Monitoring Product Logs

The logs page (<i class="icon-terminal"></i>) is also available to product creators! Featuring the same interface as what you are used to with the [developer version of the dashboard](/guide/tools-and-features/dashboard/), the logs will now include events from any device identifying as your product. Use this page to get a real-time look into what is happening with your devices. In order to take full advantage of the logs page, be sure to use `Particle.publish()` in your firmware.

## Managing your subscription

To see all billing related information, you can click on the billing icon in your organization's sidebar (<i class="ion-card"></i>). This is the hub for all billing-related information and actions.

### How billing works

When you signed up for Device Management, you selected a plan. Which plan you selected determines the amount and frequency at which you are billed. For instance, selecting the Team Plan (annually) enrolls you for a $49.00 per team member per month plan, paid annually. This means if you have 1 team member, you will be billed $588 once a year.

It is important to note that charges made to your account are for <em>future billing periods</em>, not <em>previous billing periods</em>. In other words, getting charged extends your organization's access to the end of the <em>upcoming</em> billing period. When that billing period ends, a new charge will occur, once again extending your team's access.

If you update your subscription (i.e adding or removing team members, switching plans) in the middle of the month, we will automatically calculate the proper adjustments to be made, and apply them to your next payment.

### Updating your credit card

You can update your credit card from the billing page by clicking on the "EDIT" button next to the credit card on file. Whenever your credit card on file expires or no longer is valid, be sure to update your credit card info here to avoid any disruptions in your service.

![Update your credit card](/assets/images/edit-card.png)

### Failed Payments

If we attempt to charge your credit card and it fails, we do not immediately prevent you or your team from accessing your Device Management dashboard. We will continue to retry charging your card once every few days <strong>for a maximum of 3 retries</strong>. You will receive an email notification each time an attempt is made and fails. When you receive this notification, the best thing to avoid any interruption in service is to <a href="#updating-your-credit-card">update your credit card</a>.

After we have unsuccessfully tried to charge your card 3 times, your Device Management account will be locked. <strong>Your organization, products, and all data will not be deleted</strong>. After re-activating your subscription, you will be able to access your Organization's dashboard once again.

### Cancelling a subscription

You can cancel your subscription by clicking on the "Cancel subscription" link on your organization's billing page, under the "Manage subscription" header. After cancellation, you and your team are still able to access Device Management until the end of the current billing period.

It is important to note that after cancellation, <strong>your data is not deleted</strong>. Existing devices and products that encompass your organization will still function as normal. After the current billing period has ended, your account will be put in an inacive state. You can re-activate your subscription at any time to resume access to Device Management.

### Re-activate a subscription

If you have cancelled your subscription but are interested in re-activation, fear not! The process is very simple and will have you building again in no time. As discussed in the previous section, your organization's data is not destroyed when you cancel your subscription.

There are two slightly different ways to re-activate, depending on the status of your subscription. If you have cancelled your subscription but are still within the current billing period, you will still have access to your dashboard and can re-activate from your organization's billing page. The manage subscription section should now be replaced with information about your subscription cancellation, and will have a button allowing you to re-activate.

![Reactivate subscription](/assets/images/reactivate-subscription.png)
<p class="caption">You can easily re-activate your subscription from your billing page</p>

If re-activating a subscription that has not been fully cancelled yet, you will remain on the same billing cycle and will be charged at the beginning of the next billing period.

If your subscription has already been marked as inactive (three failed charge attempts, or after the end of the current billing period when cancelled), re-activation is slightly different. Upon login to the dashboard, your team will be directed to an "Inactive Subscription" page, letting you know that you no longer have access to your organization's Device Management dashboard. On this page, there is also a "re-activate subscription" button. In this case, you will need to provide a valid credit card to resume your subscription.

![Inactive subscription](/assets/images/inactive-subscription.png)
<p class="caption">Re-activate your subscription after it has been marked as inactive</p>

If re-activating a subscription that is currently inactive, you will be re-enrolled in the plan you had signed up for. Your credit card will be charged immediately for the next billing period, and will begin a new billing cycle based on the day that you re-activated.

## What's next?

Congratulations! You have a grasp on how to take advantage of your organization's dashboard. Next up, you will learn how to [build your own web app](/guide/how-to-build-a-product/web-app/) for your product.
