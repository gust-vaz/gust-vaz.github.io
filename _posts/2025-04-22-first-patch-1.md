---
title: Sending my first patch to the Linux Kernel [1]
date: 2025-04-22 18:00:00 +/-0300
categories: [Open Source Software Development, Linux Kernel, MAC0470]
tags: [linux, kernel, kernel-linux, patch, iio, first]
---

### The Task

To experience the process of sending a patch to the Linux Kernel, it was provided a list of suggestions of small tasks that we could work on. Me and my partner, [Francisco](https://xivor.github.io/) decided to deduplicate a code with around 90% of similarity in their functions. At the file `drivers/iio/accel/kxcjk-1013.c`:
```c
static int kxcjk1013_setup_any_motion_interrupt(struct kxcjk1013_data *data,
						bool status)
{
    /* ... */

	ret = kxcjk1013_chip_update_thresholds(data);
	if (ret < 0)
		return ret;

	/* ... */

	if (status)
		ret |= KXCJK1013_REG_CTRL1_BIT_WUFE;
	else
		ret &= ~KXCJK1013_REG_CTRL1_BIT_WUFE;

	/* ... */
}
```
```c
static int kxcjk1013_setup_new_data_interrupt(struct kxcjk1013_data *data,
					      bool status)
{
	/* ... */

    /* ... */

	if (status)
		ret |= KXCJK1013_REG_CTRL1_BIT_DRDY;
	else
		ret &= ~KXCJK1013_REG_CTRL1_BIT_DRDY;

	/* ... */
}
```

Obs.: the ellipses parts are the same in both functions.

### Approach

Before any changes, we started looking at the code and understanding when it was used. An interesting usage of both functions was this:
```c
if (data->motion_trig == trig)
		ret = kxcjk1013_setup_any_motion_interrupt(data, state);
	else
		ret = kxcjk1013_setup_new_data_interrupt(data, state);
```

This confirmed that the functions are used in the same context, and we could deduplicate them. So we create da new function that has a flag `new_data` to determine what to do if it is a new_data or any_motion interrupt:
```c
static int kxcjk1013_setup_interrupt(struct kxcjk1013_data *data,
						bool status, bool new_data)
{
	/* ... */

	if (new_data == true) {
		ret = kxcjk1013_chip_update_thresholds(data);
		if (ret < 0)
			return ret;
	}

	/* ... */

	if (new_data) {
		if (status)
			ret |= KXCJK1013_REG_CTRL1_BIT_DRDY;
		else
			ret &= ~KXCJK1013_REG_CTRL1_BIT_DRDY;
	} else {
		if (status)
			ret |= KXCJK1013_REG_CTRL1_BIT_WUFE;
		else
			ret &= ~KXCJK1013_REG_CTRL1_BIT_WUFE;
	}

	/* ... */
}
```

### Sending the Patch

Following the [Patch Development Guidelines](https://pad.riseup.net/p/MAC0470-patch-development-guidelines-keep), we found no problems compiling, dealing with codestyle and writing the commit message, since we looked at some examples of other patches. The commit message was:
```
iio: accel: kxcjk-1013: Deduplicate setup interrupt functions

The contents of kxcjk1013_setup_any_motion_interrupt and
kxcj1013_setup_new_data_interrupt are very similar. An update was made
to merge them into one function with an additional flag indicating if
it's a new data interrupt.

...
```
The main problem was to send the contribution to the course email, because Francisco's email was not accepting his password, so I sent it. Our next step is look at the feedback and send a new version of the patch.