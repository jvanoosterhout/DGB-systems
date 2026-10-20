# Design decisions

These decisions capture choices that shape the wider system or reusable platform. Controller-specific allocations and build details remain in their implementation repositories. Decision status records the choice, not independent verification.

# DGB-system decisions

## SYS-DD-001: Use Home Assistant as the central user-control platform

Status: accepted  
Applies to: DGB-systems  
Description: Use Home Assistant as the common interface and automation environment for household devices rather than other platforms like Homey and Domotics, or adding a separate gateway (and app) for each use case. The choice reflects its existing device coverage, extensibility and active ecosystem. It centralizes user interaction but also creates a dependency on Home Assistant for central control; local device behavior remains an implementation concern. These are personal, time-specific considerations and do not mean other platforms or choises are inferior. 

### History

- 2026-10-08: Recorded the established platform choice and its central-service trade-off.

## SYS-DD-00: Prefer local operation for essential device control

Status: accepted  
Applies to: DGB-systems and connected devices  
Description: Prefer devices and integrations whose essential control and data handling can operate locally. Accept cloud or semi-local services where their additional value justifies the dependency and essential local operation remains available. This is a local-first preference, not a prohibition on cloud or remote services.

Related requirement: [SYS-REQ-0003](requirements.md#sys-req-0003-keep-essential-device-operation-local).

### History

- 2026-10-08: Recorded the maintainer's stated local-first choice without assuming that wired networking guarantees local operation.

## SYS-DD-00: Implement a monthly reboot of the controller's node. 

# Software decisions

## SW-DD-00: Use Python as programming laguage

## SW-DD-00: Use REST API as main interface

## SW-DD-00: Make software generic applicable 

## SW-DD-00: Use MQTT-discovered device configuration between Home Assistant and nodes

Status: accepted  
Applies to: HMD-DGB-based controllers  
Description: Use MQTT discovery and device-level configuration to present meaningful devices to Home Assistant, while the controller software owns bindings between those devices and physical pins. This replaced the earlier REST-oriented direction and reduces scattered pin-level configuration in Home Assistant. It adds an MQTT broker and configuration lifecycle as dependencies; adoption and recovery behavior must be checked per controller.

Source: [Coherent-control need](system-context.md#1-user-comfort-and-control); [project history](../braindump.md) (MQTT discovery and device grouping); [HMD-DGB configuration reference](https://github.com/jvanoosterhout/HMD-DGB/blob/main/README.md).

### History

- 2026-10-08: Recorded the selected device-level MQTT model; the earlier ID remains in the archive.

## SW-DD-00: Use Durable Rules for configurable event bindings

Status: accepted  
Applies to: HMD-DGB  
Description: Use Durable Rules to express conditions, state changes, timers and actions between configured devices and GPIO. This supports reusable combinations of events without a custom backend for every controller. The integration has required custom work for action dispatch and timers, and the upstream dependency's maintenance is a risk; adoption does not imply every rule pattern or safety behavior is proven.

Source: [Event-combination requirement](requirements.md#sw-req-0003-execute-actions-only-for-matching-configured-events); [HMD-DGB dependency](https://github.com/jvanoosterhout/HMD-DGB/blob/main/pyproject.toml) and [binding documentation](https://github.com/jvanoosterhout/HMD-DGB/blob/main/README.md); [project history](../braindump.md) describes the rule-format exploration.

### History

- 2026-10-08: Re-established the adopted engine choice and its maintenance/integration trade-offs; earlier ID remains in the archive.

## SW-DD-00: Use a hexagonal/port-adapter software argitecture

## SW-DD-00: 

# Hardware decisions

## HW-DD-00: Use Raspberry Pi nodes for local controller software

Status: accepted  
Applies to: controller implementations  
Description: Use Raspberry Pi nodes as a reusable, software-configurable interface to household equipment instead of requiring a separate commercial gateway or bespoke controller for each use case. Pi model, power source and network connection remain installation-specific. Trade-offs include operating-system maintenance, boot/recovery behavior and energy use.

Source: [Project history](../braindump.md) records the Pi 4 and Pi Zero 2 W choices and the reasons for trying each; controller implementations document their concrete models.

### History

- 2026-10-08: Consolidated the recurring node-platform choice while leaving model and provisioning details to each controller.

## HW-DD-00: Use detachable field connectors selected for each installation
Status: accepted  
Applies to: controller hardware  
Description: Use detachable connectors between controller enclosures and field wiring to support assembly and service, selecting connector type and pinout for the installation. MKcontroller reports that tulip/RCA, power and serial-style alternatives were unsuitable for its needs because they were too limited, fragile or large; it also keeps the water sensor's JST-SM connector and adapts it to GX12 rather than cutting it off. Gcontroller uses GX12 for the garage and RCA for irrigation. These examples support choosing connectors per interface; no one type, pinout or rating is mandated across the family.

Source: [Practical-construction need](system-context.md#5-physical-integration-and-construction); [MKcontroller connectivity rationale](https://github.com/jvanoosterhout/MKcontroller/blob/main/README.md) and [water sensor wiring](https://github.com/jvanoosterhout/MKcontroller/blob/main/hardware/wiring/water-sensor-wireviz.yaml), [Gcontroller wiring](https://github.com/jvanoosterhout/Gcontroller/blob/main/hardware/wiring/door-wireviz.yaml), and [MVcontroller hardware notes](https://github.com/jvanoosterhout/MVcontroller/blob/main/hardware/README.md). Alternatives noted for MK were considered/tried locally; they do not establish a family-wide connector requirement.

### History

- 2026-10-08: Added MK's connector trade-offs and retained JST-SM adaptation as an installation example; connector choice and ratings remain local.
- 2026-10-08: Recorded detachable field wiring as the shared intent while leaving connector selection installation-specific.

## HW-DD-00: Use components listed here as much as posible.


