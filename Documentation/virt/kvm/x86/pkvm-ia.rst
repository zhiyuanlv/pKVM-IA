.. SPDX-License-Identifier: GPL-2.0

==============================
protected kvm for IA (pKVM-IA)
==============================

Protected KVM for IA (pKVM-IA) is an extension to pKVM project from ARM to Intel
x86 for similar use cases with similar software architecture. This document
describes the high level design of pKVM-IA: the problem statement, design
choices and some architecture details. The project status and related work are
also summarized. This document tries to be helpful for those who want to get
pKVM-IA big picture before looking into code details. Since the project is still
under developing and community review, contents here may change frequently based
on community inputs and design changes.

Project Background
==================

1. protected KVM for ARM
------------------------

Protected KVM (pKVM) is from Google based on KVM ARM for Android system to
support Trusted Execution Environment (TEE) with virtualization technologies.
Different from ARM hardware solutions with Trust Zone, pKVM runs TEE inside a VM
(protected VM) in ARM EL1 privilege level as a peer of Android host OS. pKVM
enhanced KVM ARM in EL2 to isolate cpu states, memory and IO of protected VM
from host OS access.

[More Details to be Added]

2. Use cases and requirements
-----------------------------

Like ARM, the TEE usages on X86 clients are also very common. Consider a Linux
based laptop to support face authentication feature which needs to save and
process users' biology information. Such information is highly sensitive and
should be isolated from host Linux OS kernel access, because Linux kernel is too
big and can be vulnerable. If not concerning costs, there are always hardware
solutions with extra components, but they are not in the scope of today's
discussion which explores software solutions only.

A good client TEE solution should address the requirements from both security
and feature capabilities, making good balance between the enforced security and
the cost/impact to user experiences. Below will talk about them respectively.

Since TEE in clients is mainly used to protect personal information of the
device owners, some software components, like hypervisor, can be considered
trusted in Trusted Computing Base (TCB). Meanwhile the TCB should be minimized
for security, the smaller the better. The TEE should have at least cpu state and
memory protection for confidential computing. If devices can be supported in TEE
environment that would be very convenient.

The user experience is related to below requirements in terms of both
functionalities and performance:

* I/O support in host OS: It should be almost the same as bare metal,
  including both PCIE devices and other platform specific devices. Host OS is
  still the main interface to end users and should give people bare metal
  devices experiences.

* Normal VMs in host OS: If the bare metal devices support normal virtual
  machine usages, they should continue to work while TEE is enabled.

* compatibility in host OS: Introducing TEE support should have minimal impact
  to existing software stack compatibility.

* Power management in host OS: Power consumption is very important for clients.
  A TEE solution should not harm the system power management features comparing
  with bare metals.

* Good performance indicators: Good benchmark scores are always important, but
  the client platform performance is more than that. The system boot time and
  system response latencies both impact user experience heavily. Also consider
  that client platforms have smaller resources than servers, they are more
  sensitive to resource overhead like memory.

3. related work summary
-----------------------

In today's X86 platforms there are various approaches to support TEE in clients,
but none of them can well addressing all above requirements.

Type-2 hypervisors like KVM run host OS in highest privilege level which is not
possible to be isolated from TEE. The only way with KVM is to move host OS into
a VM on top of KVM, basically with nested virtualization, which will impact user
experiences pretty significantly;

type-1 hypervisors like Xen support Dom0 or primary VM to run in a lower
privileged level, making them easier to support TEE isolation. However Dom0
still has gaps for clients in terms of power management and latency etc., and
Xen hypervisor itself is not small as TCB.

Below sections will talk how pKVM-IA can meet those requirements with TEE
support while keeping host OS almost the same as bare metal.

Architecture Introduction
=========================

0. architecture overview (?)
----------------------------

1. host deprivilege
-------------------

2. page-table management
------------------------

3. VMX instruction emulation
----------------------------

4. page ownership and protection
--------------------------------

5. protected VM support
-----------------------

6. Misc
-------

Current Status of pKVM-IA
=========================

1. Basic Functionalities
------------------------

2. Security
-----------

3. Performance
--------------

Remaining Opens and TODOs
=========================


Related Work
============

1. type-1 hypervisors
---------------------

2. ikgt
-------

3. VBH
------

4. Jailhouse
------------


References
==========

[1] Google Android Virtualization Framework (AVF) https://source.android.com/docs/core/virtualization?hl=en
[2] Why AVF https://source.android.com/docs/core/virtualization/whyavf
[3] AVF architecture https://source.android.com/docs/core/virtualization/architecture?hl=en
[4] LWN topic about pKVM https://lwn.net/Articles/836693/
[5] pKVM slides in LPC https://lpc.events/event/7/contributions/780/attachments/514/925/LPC2020_-_Protected_KVM_.pdf
[6] pKVM on KVM forum 2020 https://www.youtube.com/watch?v=wY-u6n75iXc
[7] pKVM deep dive on KVM forum 2022 https://www.youtube.com/watch?v=9npebeVFbFw
[8] Linux ARM Hypervisor in USENIX ATC 2017 https://www.usenix.org/system/files/conference/atc17/atc17-dall.pdf
[9] Related slides of <8> above: http://events17.linuxfoundation.org/sites/events/files/slides/To%20EL2%20and%20Beyond_0.pdf
[10] pKVM-IA on KVM forum 2022 https://www.youtube.com/watch?v=EP9ps_h-WeI
[11] AArch64 virtualization https://developer.arm.com/documentation/102142/0100/?lang=en
[12] KVM-based Type1 (or 1.5) hypervisor from KVM forum 2020 https://www.youtube.com/watch?v=6Gjex5jNkxs
[13] Virtualization Based Hardening from KVM forum 2019 https://static.sched.com/hosted_files/kvmforum2019/d9/VBH_final.pdf
