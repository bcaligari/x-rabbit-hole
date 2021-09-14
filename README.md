# rabbit-hole

Rabbit Hole is a multi-state capable test HA Resource Agent.

Based heavily on the Dummy OCF RA.

*** This is a works in progress.  It is not sanitised and not meant
for use in anything other than a most disposable of test virtual HA
clusters ***.



## Installation

* Assuming recent SLES
* Create directory `/usr/lib/ocf/resource.d/hole`
* Drop `Rabbit` inside the hole
* `chmod +x Rabbit`
* Go down the rabbit hole

## Runtime stuff

### Filesystem Artifacts

* State file - `/var/run/Rabbit-<ra>.state`
* Behaviour directory - `/var/run/Rabbit-<ra>.cfg` - not cleaned up automatically

### Tweaking Behaviour

* Fail start - `touch /var/run/Rabbit-<ra>.cfg/fail_start`
* Fail stop - `touch /var/run/Rabbit-<ra>.cfg/fail_stop`
* Ban promotion  - `touch /var/run/Rabbit-<ra>.cfg/unpromotable`
* Crash - `touch /var/run/Rabbit-<ra>.cfg/crash` - fails once and is removed
* Crash and Burn - `touch /var/run/Rabbit-<ra>.cfg/burn` - fails and sets fail start
* Fail - `touch /var/run/Rabbit-<ra>.cfg/fail` - fail and stay failed - not implemented

## Example cluster config

```{text}
primitive white ocf:hole:Rabbit \
	op start interval=0 \
	op stop interval=0 \
	op monitor role=Master interval=20 \
	op monitor role=Slave interval=10
ms ms-white white \
	meta target-role=Master clone-max=2 master-max=1 master-node-max=1
```
