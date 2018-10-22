---
layout: post
author: joshua
categories: [embedded_development]
tags: [ARM, interrupts]
---

The ARM microcontoller has found widespread adoption in the field of embedded development in recent years. One of the most common tasks for a developer to do is setup interrupts to handle peripheral interraction and other significant hardware events. While this is a straightforward task for developers familiar with the ARM architecture, it's not necessarily obvious to those new to ARM development.

The ARM architecture uses a three tiers of interrupt enabling to enable any interrupt in the system. TODO: <-- Reference ARM manual

1. Enable specific desired interrupt in subsystem
   - ex. SPI receive byte

1. Enable subsystem interrupts
   - ex. Enable SPI interrupts

1. Enable global interrupts
   - Enable all currently enabled interrupts to run

These three tiers being present may be confusing to those new to the ARM architecture, but they make for a great method for ensuring no interrupts are accidentally enabled during development too. Not to mention, the great granularity/resolution that is provided by being able to either disable a whole subsystems interrupts or just one of the interrupts within the subsystem.

Below is some sample code for enabling an interrupt on Ti's MSP432 ARM Cortex-M4F processor.

```C
int t;

void main(void) {
  // Enable specific interrupt

  // Enable subsystem interrupts

  // Enable global interrupts
  MAP_Interrupt_enableMaster();

  while(1) {
    if(t == 1) {
      // Reset local interrupt flag
      t = 0;

      // do something interesting
    }
  }
}

void interrupt_handler(void) {
  t = 1;
}
```