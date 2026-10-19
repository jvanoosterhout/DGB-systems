# Design attempts

A record of explored approaches that shaped reusable architecture or system choices. These attempts are historical evidence, not a chain from a requirement to a decision.

## SW-DA-00: Generik backend and REST-API configuration of GPIO pins instead of generik REST api with custom backend

Status: implemented  
Applies to: GPIOapi (HMD-DGB predecessor) 
Description: Service work progressed from a REST `get_state`/`set_state` with custom GPIO backend to a shared configurable pin model and GPIOapi.

### History

- 2026-10-08: Consolidated the REST-to-MQTT evolution and its configuration-management lesson; earlier attempt IDs remain in the archive.

## HW-DA-00: Try Pi Zero 2 W and SSH instead of Pi 4 desktop operation with teamviewer

Status: implemented  
Applies to: controllers; initially explored for a space-constrained controller  
Description: Tried a Pi Zero 2 W with configuration over SSH. The after the Pi 4/PoE setup was considered too large. Aditional motivation came from the power-hungry MV controller. SSH was adopted for instalation and remote maintenance after initial programing on a Pi 4 with desktop/TeamViewer use. Later VS Code via SSH was adopted for Pi 4. VS code via SSH on a Pi Zero itself was unreliable because of its resource limits, so development continued on a Pi 4 through SSH. Outcome: smaller Pi nodes and SSH-based administration were used where they fit, while development and network/power choices remained context-dependent.

Related decision: [SW-DD-000x](...).

### History

- 2026-10-08: Re-recorded the platform and administration experiment from the project history; earlier ID remains in the archive.
- 2024: tested and adopted VS code over SSH on Pi4
- 2023: Configured the code on a Pi 4 and installed it via SSH on the Pi Zero. 
- 2023: Need for a small controller to control the bathroom lights and heating system. PI Zero seemd fit fit the job. 

## SW-DA-00: Explore configurable rule-engine approaches

Status: implemented  
Applies to: HMD-DGB  
Description: Explored ways to configure event bindings, including dynamically creating Python rules, a custom JSON representation and Durable Rules' JSON format. The custom JSON approach was too limited; dynamic action dispatch and timers were difficult in both approaches. Durable Rules was selected for HMD-DGB, with integration and upstream-maintenance risks remaining. 

Related decision: [SW-DD-0003](design-decisions.md#sw-dd-0003-use-durable-rules-for-configurable-event-bindings).

### History

- 2026-10-08: Re-recorded the rule-engine exploration in the needs-derived record set; earlier ID remains in the archive.
- 2026-Q2: first working binding principle in HMD-DGB ready, Design Attempt closed as implemented. 
- 2026-Q1: Atempted to work with Durable rules. First approach was custom configs with dynamic python rule configuration. This gave limited posibilities with complicated code. JSON based configurations were discoverd before but were not expected to fit, though in the end JSON methods seemd the best fit with only timers (and run action) being none working. 
- 2025-Q4: explore rule engines, Durable rules seemd to have the best fit with the project scope

## SW-DA-00: Explore MQTT device configuration (to replace REST api)

Status: implemented  
Applies to: GPIOapi migration to HMD-DGB 
Description: Service work progressed from the genrik REST-API in GPIOapi towards MQTT-configured devices. Major accelerators in this transitions were work experience with MQTT and an ebusd MQTT. Both showed the value of MQTT and ebusd additionally showed the power of MQTT discovery and grouping entities as devices. Configuration complexity with GPIOapi in Home Assistant motivated moving toward MQTT-configured devices. Outcome: HMD-DGB implements this direction.

Related decision: [SW-DD-0002](design-decisions.md#sw-dd-0002-use-mqtt-discovered-device-configuration-between-home-assistant-and-nodes).

### History

- 2026-10-08: Consolidated the REST-to-MQTT evolution and its configuration-management lesson; earlier attempt IDs remain in the archive.
- 2026-Q2: first working MQTT principle in HMD-DGB ready, Design Attempt closed as implemented. 
- 2025-Q4: Explore the HMD package and made a first succefull implementation.  
- 2025-Q4: Explore MQTT discoveralble packages. HMD seemd to have the best fit with the project scope. 

## SW-DA-00: Test pre-release HMD-DGB test node

Status: implemented  
Applies to: MKcontroller and HMD-DGB  
Description: The new MKcontroller was build and used to trial pre-alpha and alpha HMD-DGB releases so experimental software would not directly run on controllers supporting more essential household functions. A first stage was a non MK-controller function called the `MQTT_duration_run` example. It ran for more than ten consecutive days in Q1 2026 (It predated the 31 day requirement [SW-REQ-014](DGB-systems/docs/requirements.md)). Further tests where conducted with MK functions, which helped improving HMD-DGB user experience, functions and finding & fixing bugs.

### History

- 2026-10-08: Re-recorded the MK test-platform lesson in the needs-derived set; no independent run log was present in the inspected sources.