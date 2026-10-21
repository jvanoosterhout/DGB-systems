# Design attempts

A record of explored approaches that shaped reusable architecture or system choices. These attempts are historical evidence, not a chain from a requirement to a decision.

# System design attempts


# Software design attempts

## SW-DA-001: Generik backend and REST-API configuration of GPIO pins instead of generik REST api with custom backend

Status: implemented  
Applies to: GPIOapi (HMD-DGB predecessor) 
Description: Work progressed from a REST `get_state`/`set_state` with custom GPIO backend to a shared configurable pin model and GPIOapi.

### Question explored

Could one generic REST interface and configurable GPIO model support multiple controller use cases without writing and maintaining a different GPIO backend for each controller? Which pin behavior needed to be represented as configuration, and which behavior belonged in the reusable service?

### Exploration

- **January 2023:** The startingpoint with MVcontroller that used a REST `get_state`/`set_state` interface with a custom GPIO backend. The backend became to difficult to understand and could not be maintained for long. One custom backend per use case would not scale well.
- **After that observation:** The design moved toward one configurable backend, with GPIO input/output behavior represented through pin configuration (later described as PinIn/PinOut and PinKeeper).
- **2024 to Q1 2025:** GPIO control was generalized behind the REST API, and GPIOapi was separated into a reusable package. Gcontroller and MVcontroller ran GPIOapi by Q1 2025. REST calls carried the full pin configuration to support recovery after restart or power loss; the notes state the intent. 

### Findings

- A generic `get_state`/`set_state` interface alone did not prevent controller-specific backend duplication.
- Separating pin variation into configuration enabled a more reusable GPIO backend and reduced the need to copy controller-specific code.
- Packaging GPIOapi separately supported reuse across projects, but the experience also exposed configuration and maintainability limits that later motivated a different system/software direction.
- This attempt concerns the REST/GPIOapi path. The later move from REST to MQTT-discovered devices is recorded separately in [SW-DA-003](#sw-da-003-explore-mqtt-device-configuration-to-replace-rest-api).

### Relationship to needs and requirements

- It responds mainly to [long-term support and maintainability](system-context.md#3-continuity-trust-and-maintainability), and secondarily to [approachable automation](system-context.md#1-user-comfort-and-control) through a more understandable reusable backend.
- It relates to [SW-REQ-001](requirements.md) and [SW-REQ-002](requirements.md) for configurable GPIO inputs and outputs, and [SW-REQ-005](requirements.md) for bindings. Those requirements describe desired capabilities; this attempt records the historical route taken to explore them.

### History

- 2026-10-08: Consolidated the REST-to-MQTT evolution and its configuration-management lesson; earlier attempt IDs remain in the archive.

## SW-DA-002: Explore configurable rule-engine approaches

Status: implemented  
Applies to: HMD-DGB  
Description: Explored ways to configure event bindings. Durable Rules was selected for HMD-DGB, with integration and upstream-maintenance risks remaining. 

### Question explored

How can configured device and GPIO events be matched and routed to reusable actions, without writing a separate rule implementation for each controller?

### Exploration

- **Q4 2025:** Exploration of rule engines and found Durable Rules promising for the project scope.
- **Q1 2026:** The first approach generated Python rule definitions dynamically from configuration. This offered limited possibilities and became complicated to maintain. Even worse, it required a custom JSON rule representation 

- **Q1 2026:** The build in JSON loader was tested succesfully. Matching could be expressed declaratively, but action dispatch and timers required additional work.
- **Q2 2026:** A first working binding approach was ready in HMD-DGB; Durable Rules was adopted for configurable bindings.

### Findings

- A declarative rule format was a better fit for configuration than dynamically generating Python code, but it did not remove all integration complexity.
- The custom JSON approach was too limited for the required rule behavior.
- Durable Rules enabled the chosen binding model, while action dispatch, timers and dependency maintenance remained trade-offs. Adoption is not proof that every event sequence is correct or safe.

### Relationship to needs and requirements

- This attempt supports the need for maintainable, reusable software and the need to represent composite-device behavior.
- It relates to [SW-REQ-006](requirements.md#sw-req-006-execute-actions-only-for-matching-configured-events). The requirement states desired behavior; this DA records exploration of a mechanism for expressing it.
- The decision to use Durable Rules is recorded in [SW-DD-005](design-decisions.md#sw-dd-005-use-durable-rules-for-configurable-event-bindings), including its dependency trade-off.

### History

- 2026-10-08: Re-recorded the rule-engine exploration in the needs-derived record set; earlier ID remains in the archive.
- 2026-Q2: first working binding principle in HMD-DGB ready, Design Attempt closed as implemented. 
- 2026-Q1: Atempted to work with Durable rules. First approach was custom configs with dynamic python rule configuration. This gave limited posibilities with complicated code. JSON based configurations were discoverd before but were not expected to fit, though in the end JSON methods seemd the best fit with only timers (and run action) being none working. 
- 2025-Q4: explore rule engines, Durable rules seemd to have the best fit with the project scope

## SW-DA-003: Explore MQTT device configuration (to replace REST api)

Status: implemented  
Applies to: GPIOapi migration to HMD-DGB 
Description: Service work progressed from the genrik REST-API in GPIOapi towards MQTT-configured devices. 

### Question explored

Could MQTT discovery and device-level configuration present coherent Home Assistant devices while moving pin bindings and controller-specific configuration out of scattered Home Assistant automations, entities and helpers?

### Exploration

- **Q4 2025:** MQTT was tried with ebusd. Device discovery was found to be very intuitive as related entities were grouped under one Home Assistant device, including a software-update indicator.
- At the same time, GPIOapi configuration and Home Assistant entities/helpers had become difficult to manage across controller use cases; some configuration also remained in Home Assistant's UI.
- **Q1-Q2 2026:** Work began replacing the REST-oriented interface with HMD-DGB's MQTT device model. HMD-DGB now implements this direction; migration and configuration state still vary by controller.

### Findings

- MQTT discovery provided a useful grouping model for related entities and allowed a shift toward meaningful devices.
- Moving configuration toward controller services addressed scattered pin-level setup, but introduces a broker and retained-configuration lifecycle that also needs maintenance. Though at least this one can be managed centrally.

### Relationship to needs and requirements

- This attempt supports coherent user control and maintainability by reducing scattered entities and repeated controller configuration.
- It relates to [SYS-REQ-001](requirements.md#sys-req-001-present-hmd-dgb-capabilities-as-meaningful-devices-in-a-central-home-automation-platform) and [SYS-REQ-002](requirements.md#sys-req-002-set-service-configurations-from-a-central-home-automation-platform). These requirements describe desired capabilities, not proof that migration is complete.
- The selected MQTT device-configuration direction is recorded in [SW-DD-004](design-decisions.md#sw-dd-004-use-mqtt-discovered-device-configuration-between-home-assistant-and-services).

### History

- 2026-10-08: Consolidated the REST-to-MQTT evolution and its configuration-management lesson; earlier attempt IDs remain in the archive.
- 2026-Q2: first working MQTT principle in HMD-DGB ready, Design Attempt closed as implemented. 
- 2025-Q4: Explore the HMD package and made a first succefull implementation.  
- 2025-Q4: Explore MQTT discoveralble packages. HMD seemd to have the best fit with the project scope. 

## SW-DA-004: Test pre-release HMD-DGB test node

Status: implemented  
Applies to: MKcontroller and HMD-DGB  
Description: The new MKcontroller was build and used to trial pre-alpha and alpha HMD-DGB releases so experimental software would not directly run on controllers supporting more essential household functions. 

### Question explored

 Could a simple duration run expose basic stability problems over multiple days?Could HMD-DGB be exercised on a separate, less critical controller before release, so experimental software would not first be deployed on controllers supporting more essential household functions?

### Exploration

- **Q1 2026:** MKcontroller was constructed and used as a standalone pre-alpha test platform. The `MQTT_duration_run` example ran for more than ten consecutive days.
- The MKcontroller was later used to test alpha HMD-DGB releases and controller functions, with reported improvements to HMD-DGB behavior and fixes from issues found during testing.

### Findings

- Separating release trials from controllers serving more essential functions reduced the risk of exposing those installations to experimental software.
- The reported duration run is limited evidence for that example and setup; it does not establish release stability or validate all controller functions.
- The run exceeded ten days but did not meet the later 31-day target in [SW-REQ-014](requirements.md#sw-req-014-the-software-shall-be-able-to-work-for-at-least-31-days). 


### Relationship to needs and requirements

- This attempt supports the needs for dependable, maintainable software and for avoiding disruption to essential household functions.
- It provides historical context for [SW-REQ-014](requirements.md#sw-req-014-the-software-shall-be-able-to-work-for-at-least-31-days), but the reported run is not evidence of satisfying that requirement.

### History

- 2026-10-08: Re-recorded the MK test-platform lesson in the needs-derived set; no independent run log was present in the inspected sources.

# Hardware design attempts

## HW-DA-002: Try Pi Zero 2 W and SSH instead of Pi 4 desktop operation with teamviewer

Status: implemented  
Applies to: controllers; initially explored for a space-constrained controller  
Description: Tried a Pi Zero 2 W with configuration over SSH. The after the Pi 4/PoE setup was considered too large. Aditional motivation came from the power-hungry MV controller. SSH was adopted for instalation and remote maintenance after initial programing on a Pi 4 with desktop/TeamViewer use. Later VS Code via SSH was adopted for Pi 4. VS code via SSH on a Pi Zero itself was unreliable because of its resource limits, so development continued on a Pi 4 through SSH. Outcome: smaller Pi nodes and SSH-based administration were used where they fit, while development and network/power choices remained context-dependent.

### Question explored

Could a smaller, lower-power node fit a space-constrained installation while remaining practical to set up and maintain? Could SSH replace desktop/TeamViewer access for routine node administration?

### Exploration

- **2023:** A Pi Zero 2 W and SSH were tried for a compact controller after the Pi 4/PoE arrangement was considered too large and power-hungry. SSH access worked well enough to encourage wider use.
- **2024:** Development moved from a Pi 4 desktop/TeamViewer workflow to a laptop using VS Code over SSH to the Pi 4. VS Code over SSH directly on the Pi Zero was also tried, but was unreliable under its resource limits.

### Findings

- The Pi Zero 2 W suited smaller controller installations, while the Pi 4 remained useful as a development and testing host.
- SSH became practical for installation and remote maintenance as the maintainer gained experience; this does not mean SSH or headless setup was initially a better approach.
- The reported approximately 6 W Pi 4/PoE consumption is a local observation that motivated experimentation, not a general power comparison between Pi, PoE, Ethernet and Wi-Fi.

### Relationship to needs and requirements

- This attempt supports approachable setup, reduced power use and practical physical integration, but shows trade-offs between node size, energy use and development tooling.
- It informs [HW-REQ-002](requirements.md#hw-req-002-each-node-shall-use-as-little-as-possible-power) and [SW-REQ-011](requirements.md#sw-req-011-install-nodes-with-minimal-effort). 
- The family-level Raspberry Pi choice is captured in [HW-DD-001](design-decisions.md#hw-dd-001-use-raspberry-pi-nodes-for-local-controller-software); individual model and network choices remain installation-specific.

### History

- 2026-10-08: Re-recorded the platform and administration experiment from the project history; earlier ID remains in the archive.
- 2024: tested and adopted VS code over SSH on Pi4
- 2023: Configured the code on a Pi 4 and installed it via SSH on the Pi Zero. 
- 2023: Need for a small controller to control the bathroom lights and heating system. PI Zero seemd fit fit the job. 

