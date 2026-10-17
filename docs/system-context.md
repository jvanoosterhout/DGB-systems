# System context

## Needs and motivation

The original goals of HMD-DGB: home automation that is affordable, understandable, trustworthy and maintainable over years, with as few separate systems to integrate as practical. The goal changed and expaned over the years. This system context document descripes these goals as the user needs related Systems Enginering practice. 

### 1. User comfort and control

- **Approachable automation:** make setup and everyday use manageable without requiring skills the user does not (yet) have. 

    Homey's easy start, including a limited free account, was attractive in 2021; Home Assistant appeared to offer similar capabilities without cost. Raspberry Pi and Python were familiar, and a desktop environment with TeamViewer initially made development accessible before SSH skills were acquired. These are historical reasons behind the chosen approach, not a claim that the present system is effortless.

- **Coherent user control:** operate varied household equipment through one familiar environment, with related entities grouped meaningfully instead of scattered across interfaces. This includes being able to configure and extend devices from that single environment; it is about the user and user experience, not software reuse by itself.

    Home Assistant is the chosen environment. The configuration mechanisem was first a REST API that, with each call, would send the entire configuration for the targeted device. Currently the configuration mechanisem relays on retained MQTT messages. 

- **Composite devices as one logical unit:** operate a multi-part device through meaningful controls and status rather than exposing its elementary pins. For example, a cover may combine up/down/stop commands with position sensors. This concerns the user's interaction with a complex device.

### 2. Local operation and connectivity

- **Local-only over semi-local and cloud connectivity:** prefer device control and data handling to work locally on (W)LAN, without depending on a vendor cloud or internet connection. Semi-local or cloud-connected features may be acceptable when their added value justifies the dependency and essential local operation remains available. 

- **Limit wireless clutter:** avoid accumulating many separate wireless systems and gateways, and avoid unnecessary radio communication where a practical alternative exists. Prefere to group multiple functions in a single node and prefere to have a LAN interface. The original motivation was a concern that many wireless products would make the home feel like a "wireless mess"; this is a preference, not a measured claim about radio interference.

### 3. Continuity, trust and maintainability

- **Long-term support:** rely on hardware and software that are likely to remain supported, preferably with an active community. Prefer support over many years, potentially a decade.

- **Maintainability:** Have a clean and logical software argitecture with unit and intergartion tests. Avoid duplicated custom backends. 

- **Recoverable controller nodes:** replace a failed Pi or SD card and restore the node without manually reloading its controller configuration. It should be as simple as writing an SD card, power on, (configure its IP address,) install HMD-DGB, then manage configuration and updates from Home Assistant. 

- **Predictable behavior during interruption:** know what outputs and devices do during and after restart, configuration failure or communication loss, especially where physical equipment is affected. 

- **Trusted technology:** understand where data goes, whether a device can be used locally, and whether its software and support are likely to last. 

    A Wi-Fi infrared heating panel with LED was not bought, because of its country of origin, uncertainty about future updates and uncertainty about capabilities, leading to purchase of an 433MHz. Shelly's local-only mode is highly valued. These are personal, time-specific trust considerations, not a blanket rejection of wireless or proprietary technology.

### 4. Cost and resource use

- **Cost-effective automation:** get started without paying for a gateway whose cost is disproportionate to the functions it provides. 

    A gateway above EUR 200 is unlikely to be adopted. Though Somfy and Powerview gateways were considered in relation to how many devices they could serve. For a single device that already has a basic physical interface, a custom local controller may be more economical. Cost, coverage and available functionality all matter.

- **Minimize power consumption:** Use few as posible energy by gateways and controller nodes. 

    An earlier belief that wired Ethernet, especially PoE, would use less power because it needed fewer power adapters was later rejected. An earliere MV controller was build on a Raspberry Pi 4 with Poe and used 6 W, This motivated to switching to a Pi Zero 2 W using 1-2W. 

### 5. Physical integration and construction

- **Adapt existing equipment:** connect useful non-smart equipment and sensors rather than replacing them unnecessarily. This concerns preserving and interfacing with available equipment.

- **Practical controller construction:** make compact enclosures that are straightforward to manufacture and service, with detachable field wiring. A clear cover for visual inspection is a personal preference. 

### 6. Personal learning goals

These are personal development goals and needs, not recurring needs of the system or its users. They have nevertheless influenced design choices and experiments over time:

- 2021: learn to interface with devices, initially focusing on a REST API.
- 2023: learn how to unify connectivity and control across different use cases rather than maintain one custom backend per controller.
- 2025: learn more about Systems Engineering, documentation and extending compatibility.
- 2026: experiment with GitHub Copilot as a coding assistant.

## Controller-specific examples of needs

These examples explain why each implementation exists; they are not requirements for every controller:

- [MKcontroller](https://github.com/jvanoosterhout/MKcontroller/blob/main/README.md): notice doorbell presses when people may be outside, silence the bell without losing notification capability. Also monitor hard-water use to know when the water softener needs salt. 
- [Gcontroller](https://github.com/jvanoosterhout/Gcontroller/blob/main/README.md): avoid repeatedly replacing lost costly garage remotes, know when a garage door has remained open, prevent accidental operation. Also control irrigation.
- [MVcontroller](https://github.com/jvanoosterhout/MVcontroller/blob/main/README.md): control ventilation in response to household conditions (Relative Humidity and CO2) and control demand-valves for the living-room and kitchen VS bedrooms. 


## Participants and boundaries

Users interact with Home Assistant entities and automations. A user builds, configures and updates controllers. Home Assistant communicates with a broker through MQTT; [HMD-DGB](https://github.com/jvanoosterhout/HMD-DGB) runs on the Raspberry Pi controller nodes and presents devices to Home Assistant through MQTT discovery. Controller-specific GPIO allocation and electrical circuits connect the node to sensors and actuators. 

Home Assistant is responsible for higher-level user automations; controller software binds meaningful devices to pins. The exact split for a particular installation must be checked in its controller configuration and Home Assistant files, not inferred from this family-level model.

