# DGB-systems

DGB-systems connects the generic [HMD-DGB](https://github.com/jvanoosterhout/HMD-DGB) software platform with separately maintained physical nodes. It documents the family-wide context, architecture and design history; the software and hardware implementations remain in their own repositories.

This DGB-systems repository covers system-wide, software and controller concerns. Technical details stay with the implementation repositories; records link back to this repository instead of duplicating them. It aims to learn and practice Systems Enginering skills. 

# Definitions

These are frequently used definition throughout the DGB-systems. 

- DGB-system: Components involved in the DGB eco system. These consist of controllers equiped with SBC hardware nodes running the HMD-DGB service.
- HMD-DGB: Main software component within the DGB-system. When it runs it makes the service. Sometimes also called the service of just DGB. 
- Node: The Single Board Computer (SBC) that runs the HMD-DGB service within the DGB-system
- Service: The HMD-DGB software that runs on the node and handels al device and pin bindings through the HMD-DGB software.
- Controller: The combined piece of hardware being able to perform actual action and measurments at the intended location. The hardware can include SBC, relays, sensors, connectors and many more components. 
- Central home automation platform: a platform or service to connects all the users smart home systems in one place. This central system could be e.g. Homey or Domotics, but by Design Decision ### [NEEDS REF] Home Assistant was chosen. 

## Components

| Project | Role |
| --- | --- |
| [HMD-DGB](https://github.com/jvanoosterhout/HMD-DGB) | Generic software package that can bind Raspberry Pi GPIO and Home Assistant device. |
| [MKcontroller](https://github.com/jvanoosterhout/MKcontroller) | Utility-cupboard controller, including doorbell and water-meter use cases. |
| [Gcontroller](https://github.com/jvanoosterhout/Gcontroller) | Garage-door and garden-irrigation controller use cases. |
| [MVcontroller](https://github.com/jvanoosterhout/MVcontroller) | Mechanical-ventilation controller use cases; current hardware documentation is incomplete. |

## System context

```mermaid
flowchart TB
	User[User] --> HA[Home Assistant]
	HA <--> MQTT[MQTT broker]
	MQTT <--> MK[MKcontroller: HMD-DGB and hardware]
	MQTT <--> G[Gcontroller: HMD-DGB and hardware]
	MQTT <--> MV[MVcontroller: HMD-DGB and hardware]
	MK <--> MKField[Utility-cupboard sensors and actuators]
	G <--> GField[Garage and garden sensors and actuators]
	MV <--> MVField[Mechanical ventilation equipment]
```

Home Assistant provides the user-facing entities and automations. HMD-DGB runs on the controller nodes and connects MQTT devices to GPIO through device, pin and binding configuration. The controller repositories own the concrete wiring, configuration and installation details. See the [system context](docs/system-context.md) for a description of the user needs.

## Documentation

- [Documentation map](docs/README.md)
- [Needs and motivation](docs/system-context.md#needs-and-motivation)
- [Requirements](docs/requirements.md), [design decisions](docs/design-decisions.md) and [design attempts](docs/design-attempts.md)
- [Project history notes](braindump.md) and [documentation migration instructions](instructions.md)

## Status

This is a work in progress repository. 

## License & Disclaimer

This is a personal home automation project shared as-is for educational purposes. Use at your own risk. Electrical work involving relays and/or high voltage should be performed safely and verified before deployment.