---
title: Product Firmware
columns: two
template: guide.hbs
order: 4
---

# Product Firmware

If you are building a product using Particle, firmware management will
be a critical component of both your development process as well as
overseeing a fleet of devices out in the field.

Having a mechanism to seamlessly deploy over-the-air updates to
your devices will allow you to iterate on the software side
of your product, continuously improving the end-users' experience far
after the physical units have been shipped.

In addition, over-the-air (OTA) firmware updates can provide you
additional flexibility in the manufacturing process. Specifically,
you may continue to develop firmware between the time of manufacturing
and shipping your product, and send the latest firmware
to the device on setup.

During the development phase of the product, you can use Particle's
firmware management tools to test out large-scale rollouts of firmware,
and get comfortable with the process. At scale, you will be able to
roll out firmware upgrades safely and efficiently to any number of
devices that make up your fleet.

## Your Product ID

When you created your product, a unique numeric ID was assigned to it.
This ID will be used by the Particle cloud to identify which devices
should be treated as your product, and subsequently it is what empowers you to
manage firmware running on those devices *en masse*. This key is
*very important* to you as a product creator, and it will be used
countless times during the development and manufacturing process for
your product.

### Where to find my Product ID

You will be able to find your product's ID at any time in your
Dashboard's navigation bar when viewing information about your product:

![A new product](/assets/images/product-id.png)
<p class="caption">Your product ID is marked with a key icon</p>

### Representations of Product ID

There are **2** representations of a device's Product ID that both have
significance:
- <i class="ion-cloud"></i> **Cloud**: The Particle cloud keeps a record
  of which product ID a device is associated with
- <i class="im-devices-icon"></i> **Device**: The device itself will
  report a product ID when coming online based on the firmware running
on the device

Your goal as a product creator is to ensure that both of these Product
IDs match for the devices that should behave as your product.
When both of these IDs match, the Particle cloud can proceed with confidence
in treating the device as part of the product. If these IDs *don't*
match, the device will be limited in its privileges as a member of the
product.

The table below shows the three possible combinations of product IDs
between the cloud and the device. In any case when the product ID of the
device does not match the product ID saved in the cloud, you should
expect limited product functionality.

| <i class="ion-cloud"></i> Cloud | <i class="im-devices-icon"></i> Device | Status                                                  |
|---------------------------------|----------------------------------------|---------------------------------------------------------|
| 1                               | 1                                      | <i class="ion-checkmark"></i> OK                        |
| 1                               | 2                                      | <i class="ion-alert-circled"></i> Limited Privileges    |
| 2                               | 1                                      | <i class="ion-alert-circled"></i> Limited Privileges    |

<p class="caption">The device's firmware product ID should match the product ID
of the device in the cloud</p>

## Adding Devices to a Product

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

### Product Privileges

You've successfully added devices to your product. But what does that
really mean in terms of behavior?

A device that has been successfully added to a product will be granted
special privileges. These privileges include (but are not limited to):

- Receiving over-the-air (OTA) firmware updates from this product
- Publishing events that will appear in this product's event stream
- Claiming by a customer of your organization
- Triggering product webhooks

These privileges are only guaranteed when there are [no
mismatches](#mismatched-product-ids) between the Product ID of the
device in the cloud and the Product ID reported by the device in firmware.

## Flashing Devices with Product Firmware

In order to ensure that proper Product ID matching, your devices should
run your product's firmware, complete with your Product ID compiled in
the binary.

### Preparing a binary

To get started, click the Firmware icon in the left sidebar of your
product's dashboard to get started. This will direct you to your product's
firmware page, your centralized hub for viewing and managing firmware for your
product's devices. If you haven't yet uploaded any firmware for this product,
your page will look like this:

![Firmware page](/assets/images/firmware-page.png)

If you have been using the Web IDE or Particle Build to develop firmware, you are used to the process of writing, compiling, and then flashing firmware. You will follow the same high-level process here, but altered slightly to work with a group of devices. The first thing you'll need to do is compile a *firmware binary* that you will upload to your dashboard.

Unlike compiling a binary for a single device, it is critical that the
**Product ID** and a **firmware version** are included in the compiled
binary. Specifically, you must add `PRODUCT_ID([your Product ID])` and `PRODUCT VERSION([version])` into the application code of your firmware. This is documented fully [here](https://github.com/spark/firmware/blob/develop/docs/build.md#product-id).

You can add these two macros anywhere in your application code, but it's
likely easiest to add them to the top of your main `.ino` file. Remember
that your [Product ID](#your-product-id) can be found in the navigation of your dashboard. The firmware version must be an integer that increments each time a new binary is uploaded to the dashboard. This allows the Particle cloud to determine which devices should be running which firmwares.

Here is an example of Blinky with the correct product and version details:

```
PRODUCT_ID(94);
PRODUCT_VERSION(1);

int led = D0;  // You'll need to wire an LED to this one to see it blink.

void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  digitalWrite(led, HIGH);   // Turn ON the LED pins
  delay(300);               // Wait for 1000mS = 1 second
  digitalWrite(led, LOW);    // Turn OFF the LED pins
  delay(300);               // Wait for 1 second in off mode
}
```

### Compiling Binaries

If you are in the Web IDE, you can easily download a compiled binary by
clicking the Code icon (<i class="ion-code"></i>) in your sidebar. You
will see the name of your app in the pane, along with a download icon.
Click the download icon to compile and download your current binary.

![Compile firmware](/assets/images/ide-compile.png)
<p class="caption">Compile and download a product binary from the web IDE</p>

If you are using Particle Dev, clicking on the compile icon (<i class="ion-checkmark-circled"></i>) will automatically add a `.bin` file to your current working directory if the compilation is a success. **Note**: Make sure that you have a device selected in Particle Dev before compiling.

![Compile firmware](/assets/images/particle-dev-icon.png)
<p class="caption">Particle Dev will automatically add a compiled binary to your working directory after you compile</p>

Once you have a binary ready to go, it's time to upload it to the dashboard!

### Uploading firmware

Back on the firmware page of the Dashboard, click on the **Upload** button in the top-right corner of the page. This will launch the upload firmware modal:

![Upload firmware](/assets/images/upload-firmware.png)

A few things to keep in mind here:

* The firmware version that you enter into this screen **must match** what you just compiled into your binary. Madness will ensue otherwise!
* You should give your firmware a distinct title that concisely describes how it differs from other versions of firmware. This name will be important in how firmware is rolled out to devices
* Attach your newly compiled `.bin` file in the gray box

Click upload. Congrats! You've uploaded your first version of product firmware! You should now see it appear in your list of firmware versions.

### Flashing firmware via the dashboard

On the devices page, find one of your test devices in the list of devices and click on the row. A dropdown will appear, populated with each of the firmware versions available for that product. For now, this dropdown may only have one available option (the firmware you just uploaded). Select your firmware from the list.

There are two action buttons available: **Lock and flash now**, and **Lock and flash on reset**. Both options involve "locking" a device to a firmware version. This will force the device to download and run the desired firmware version. Once the device receives and runs that firmware, it will not receive any more OTA updates even if a new firmware version is released. **Lock and flash now** will trigger an immediate OTA of the device to the desired firmware version (only available if the device is currently online). **Lock and flash on reset** will only trigger the OTA the next time the device comes online. If you do not have physical access to the device, it may be a good idea to flash on reset to avoid disrupting any current firmware running on the device.

![Lock a device](/assets/images/lock-firmware-version.png)

Once at least one device is successfully running your new firmware, you
will now have the ability to [release](#releasing-firmware) that version of firmware back on the Firmware page. Get into the habit of following this process as you continue to iterate and prepare new versions of firmware for your product!

![Product firmware version](/assets/images/product-firmware.png)
<p class="caption">Your firmware version now appears in your list of available binaries</p>

## Releasing firmware

Time to flash that shiny new binary to some devices! Notice that when you hover over a version of firmware, you have the ability to **Release firmware** (<i class="ion-star"></i>). *Releasing* firmware sets that binary as the **preferred firmware version** for all devices reporting as your product. Unless set individually, any device that does not report this released version of firmware will **automatically download and run it** next time it comes online.

Releasing firmware is the mechanism by which any number of devices can receive a single version of firmware without being individually targeted. This is incredibly valuable: imagine identifying a bug in your firmware and pushing out a fix to thousands of devices that are out in the field. Or, consider the possibility of continuing to build new features that can be introduced to customers, even after they have purchased your product and are actively using it. Amazing! This is the power of the Internet of Things.

However, releasing firmware also presents tremendous risk. The last thing you would want as a product creator is to break existing functionality for your customers, detracting from their experience with your product. Fear not! Specific safeguards are in place to help you avoid unintended regressions in firmware quality. Namely, **a firmware version must be successfully running on at least one device before it can be released to all devices.**

![Unable to release firmware](/assets/images/unable-to-release.png)
<p class="caption">Releasing a firmware version is disabled until it is running on at least one device</p>

### Recommended development flow


To get the firmware running on a device, head to your devices page by
clicking on the devices icon in the sidebar (<i
class="im-devices-icon"></i>). Before flashing your device, it's
important to first understand the recommended development flow for
managing firmware for a product. This flow is designed to minimize risk
when deploying new firmware to devices. You should start each cycle of
firmware rollout by flashing your firmware to a group of *test devices*.

Your test devices will serve as the guinea pigs for new versions of
product firmware. You should get into the habit of uploading a new
version of firmware to your product, and flashing it to your test group
to ensure your code is working as expected before rolling the firmware
out to the masses. Your test devices should be physically available to
you and/or your team for testing purposes.

Once you have thoroughly tested the new firmware on your test group and fixed any bugs, you
can then release the firmware to all other devices. This signals to the
cloud that every device should be running the new firmware, and will
trigger an auto-update to this version unless otherwise specified.

![Release firmware flow](/assets/images/release-schedule.png)
<p class="caption">The recommended flow for managing firmware</p>



## Mismatched Product IDs

There is the possiblity that the Product ID saved in the Particle cloud
for a given device does not match the self-identified Product ID
reported by the device in firmware.

These mismatches can occur from a multitude of reasons, including:
- Your team forgot to add the device to the product before directly
  flashing it with product firmware either during development or
  manufacturing
- Someone mistakenly flashed their device with firmware containing your
  Product ID
- Someone is attempting to "spoof" your product by purposefully putting
  your Product ID in their device's firmware

You as a product creator have the ability to choose how these devices
should be handled in the case of mismatched product IDs. There are two
choices for handling these kinds of devices: *Quarantining*, or *Auto
Approval*.

Ideally mismatches in Product IDs never happen, because you or a member
of your team has taken the steps to [add the device to your
product](#adding-devices-to-a-product) before the device comes online
with your product's firmware binary.

## Quarantining Devices

This is the most secure and recommmended option for handling
unrecognized devices. By default, unrecognized devices will be quarantined
unless you override the setting in your product's config page.

A device will be put into a quarantined state when it comes online and
reports a Product ID in its firmware that is different from the product
ID saved in the cloud for that device. This happens when a device was
flashed firmware containing a given product ID, but that device hasn't
been properly [added to the product](#adding-devices-to-a-product) via the dashboard.

When this happens, the device will temporarily lose all [product
privileges](#product-privileges). This is done to protect against
malicious actions from unrecognized devices.

In your product's devices hub on the Particle Dashboard, you will see a
list of your quarantined devices.

![Quarantined Devices](/assets/images/quarantined-device.png)
<p class="caption">Quarantined devices will be called out in your
product's device list</p>

Note that each quarantined device has two potential actions you can
take: *Approve* or *Deny*.

### Approving a Quarantined Device

Approving a quarantined device effectively adds it to your product and
gives it full [product privileges](#product-privileges). You should approve a quarantined
device if you recognize it as belonging to a member of your team or a
customer of your organization.

It is important to note that if your product has a released firmware
version for this product, the device may immediately update if
necessary.

### Denying a Quarantined Device

Denying a quarantined device will ensure that the Particle cloud will continue
blocking it from [product privileges](#product-privileges). You should
deny a quarantined device if you do not recognize the device or its
owner.

Denied devices will be visible in your product's device hub, but will
appear collapsed. You can re-approve a denied device at any time, in
case you do in fact want to add the device to your product.

## Auto Approving Devices

The other option for handing unrecognized devices is to automatically
approve them into your product. This is a *less secure* option, but may
be necessary if you don't have access to the list of device IDs that you
are expecting to come online and self-identify as your product.

Auto approving devices will automatically add the unrecognized device to
your product (with some restrictions) if it self-identifies as your
product without the cloud knowing about it.

For security reasons, a device will _only_ be automatically approved if
the following conditions are met:
- The device is unowned when it comes online OR
- The device is owned by a user that is a team member of your
  organization

This safeguard has been put in place to prevent a blatant attempt to
spoof your product. However, because it is possible for a device to
connect to the cloud without an owner, it is possible that an undesired
device could be automatically added to the product. This is why we
recommend that you use [quarantining](#quarantining-devices) as the
mechanism for handling unrecognized devices, unless you have a
compelling reason to use auto-approval.

