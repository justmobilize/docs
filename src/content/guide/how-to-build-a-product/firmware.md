---
title: Managing Product Firmware
columns: two
template: guide.hbs
order: 700
---

# {{title}}

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




