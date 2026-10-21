# Design decisions

These decisions capture choices that shape the wider system or reusable platform. Controller-specific allocations and build details remain in their implementation repositories. Decision status records the choice, not independent verification.

# DGB-system decisions

## SYS-DD-001: Use Home Assistant as the central user-control platform

Status: accepted  
Applies to: DGB-systems  
Description: Use Home Assistant as the common interface and automation environment for household devices rather than other platforms like Homey and Domotics, or adding a separate gateway (and app) for each use case. The choice reflects its existing device coverage, extensibility and active ecosystem. It centralizes user interaction but also creates a dependency on Home Assistant for central control; local device behavior remains an implementation concern. These are personal, time-specific considerations and do not mean other platforms or choises are inferior. 

### History

- 2021: Recorded the platform choice and its central-service trade-off.

## SYS-DD-002: Prefer local operation for essential device control

Status: accepted  
Applies to: DGB-systems and connected devices  
Description: Prefer devices and integrations whose essential control and data handling can operate locally. Accept cloud or semi-local services where their additional value justifies the dependency and essential local operation remains available. This is a local-first preference, not a prohibition on cloud or remote services.

### History

- 2026-10-08: Captured as a draft.

## SYS-DD-003: Schedule periodic restarts of controller nodes

Status: draft  
Applies to: controller nodes  
Description: A monthly restart at night is proposed. A scheduled restart can interrupt device control but also refreshes the OS. No documented need or observed failure currently establishes that monthly rebooting is beneficial, so keep this as a draft until its purpose and acceptable interruption are clearer.

### History

- 2026-10-21: Captured as a draft.

# Software decisions

## SW-DD-001: Use Python for the HMD-DGB service

Status: implemented  
Applies to: HMD-DGB  
Description: Implement the reusable controller service in Python. The original reasons were that it is open, easy to test, and already familiar from work. The choice supports experimentation and a common implementation across controllers, while still requiring attention to runtime support and dependencies on the target node.

### History

- 2021: Python was selected as the service's programming language.

## SW-DD-002: (deprecated) REST API to contol the service on the node
Status: deprecated  
Applies to: HMD-DGB predecessor and its Home Assistant interface  
Description: The initial direction used a generic REST API with `get_state` and `set_state` endpoints, later extended to configure reusable GPIO behavior. It was replaced by MQTT-configured, discoverable devices: the REST approach was understandable and useful for learning, but configuration and entities became difficult to maintain and organize in Home Assistant. The current direction is recorded in [MQTT-discovered device configuration](#sw-dd-004-use-mqtt-discovered-device-configuration-between-home-assistant-and-services).

### History

- Q1–Q2 2026: Replaced the REST-oriented interface with MQTT device configuration and discovery.
- 2021–2025: REST was explored and used for state access and generic GPIO configuration.

## SW-DD-003: Build one reusable backend for different controller use cases

Status: implemented  
Applies to: HMD-DGB and controller implementations  
Description: Reuse one generic service across controller use cases instead of copying software or maintaining a custom backend for each controller. Express hardware-specific differences through configuration and bindings, while keeping controller-specific wiring and installation details in their own repositories. A common backend reduces duplicated code, but it does not imply that every controller configuration or behavior is interchangeable.

### History

- Q1 2026: Started major redesign to bind GPIO in the service to Home Assistant entities and devices. 
- Q1 2025: Recorded reuse across use cases as the direction for the GPIO service.
- 2023: A second custom backend exposed the maintainability limits of one-off implementations.

## SW-DD-004: Use MQTT-discovered device configuration between Home Assistant and services

Status: accepted  
Applies to: HMD-DGB-based controllers  
Description: Use MQTT discovery and device-level configuration to present meaningful devices to Home Assistant, while the controller software owns bindings between those devices and physical pins. This replaced the earlier REST-oriented direction and reduces scattered pin-level configuration in Home Assistant. It adds an MQTT broker and configuration lifecycle as dependencies; adoption and recovery behavior must be checked per controller.

### History

- 2026-10-08: Captured as a draft.

## SW-DD-005: Use Durable Rules for configurable event bindings

Status: accepted  
Applies to: HMD-DGB  
Description: Use Durable Rules to express conditions, state changes, timers and actions between configured devices and GPIO. This supports reusable combinations of events without a custom backend for every controller. Compared with dynamic Python rules and a custom JSON format, it offered a more expressive configuration model, at the cost of custom action/timer integration and an upstream-maintenance risk.


### History

- 2026-10-08: Captured as a draft.

## SW-DD-006: Separate software concerns with ports and adapters

Status: proposed
Applies to: HMD-DGB 
Description: Use a ports-and-adapters (hexagonal) architecture to separate DGB application logic from external technologies and libraries. Organize HMD-DGB as a modular monolith, keeping configuration, object registry, binding semantics, and runtime behavior in the application core while accessing external capabilities through explicit ports. Implement technology-specific behavior, including HMD/Home Assistant/MQTT, GPIO hardware, and the rules engine, as adapters behind these ports. Ports should express concepts required by DGB rather than mirror the APIs of the underlying libraries. Introduce ports only where a meaningful boundary with external infrastructure or replaceable technology exists, avoiding abstractions that merely duplicate existing interfaces. Keep the architecture process-independent so that components can be isolated later if a concrete reliability or lifecycle requirement justifies it.

### History
- 2026-10-21: Captured as a draft.

## SW-DD-007: Prefer established packages when they fit

Status: accepted  
Applies to: HMD-DGB  
Description: Prefer suitable, established packages over implementations to limit project-specific code and its maintenance burden. Evaluate fit, integration effort, support and dependency risk before adopting a package; using a package does not eliminate the need to test and maintain the integration.

### History

- Q4 2025–Q1 2026: Recorded the preference for relying on existing packages as the software was redesigned.

# Hardware decisions

## HW-DD-001: Use Raspberry Pi nodes for local controller software

Status: accepted  
Applies to: controller implementations  
Description: Use Raspberry Pi nodes as a reusable, software-configurable interface to household equipment instead of requiring a separate commercial gateway for each use case. Pi model, power source and network connection remain installation-specific. Trade-offs include operating-system maintenance, boot/recovery behavior and energy use.

### History

- 2026-10-08: Captured as a draft.

## HW-DD-002: Use detachable field connectors selected for each installation
Status: accepted  
Applies to: controller hardware  
Description: Use detachable connectors between controller enclosures and field wiring to support assembly and service. The selection of suitable connectors has some history. The first idea was to use cheap [DC male jacks](https://www.tinytronics.nl/nl/kabels-en-connectoren/connectoren/dc-jack/dc-jack-plastic-5.5mm-male) [DC female jacks](https://www.tinytronics.nl/nl/kabels-en-connectoren/connectoren/dc-jack/dc-jack-plastic-5.5mm-female-mount-dc-025), though especially the male jacks were to fragile. The second attempt used 
[RCA connector](https://www.reichelt.com/nl/nl/shop/product/tulp_chassisdeel_inbouw_geisoleerd_verguld_met_gekl_-143), though they had a too loss fit in my opinion. The current, and most favorit, is the [GX12 connector](https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-4-connector-set). They exists in 2-7 pin connector and they are only slightly more expensiven than the RCA, while beeing very robust. 

### History

- 2026-10-08: Captured as a draft.

## SW-DD-003: Prefer standardized, documented components where practical

Status: draft  
Applies to: controller hardware  
Description: Prefer components from a shared, documented selection where they suit the controller, to make construction and servicing more consistent. The consolidated BOM is an inventory candidate with suggested suppliers, not an approved whitelist: confirm compatibility, availability, support and cost before standardizing a part, and allow documented controller-specific exceptions.

The following inventory consolidates unique components from the [MKcontroller BOM](https://github.com/jvanoosterhout/MKcontroller/blob/main/hardware/bom.csv), [MVcontroller BOM](https://github.com/jvanoosterhout/MVcontroller/blob/main/hardware/bom.csv), and [Gcontroller BOM](https://github.com/jvanoosterhout/Gcontroller/blob/main/hardware/bom.csv). Repeated components are listed once. Where BOM entries differ, the table identifies the controller; blank cells indicate that the BOMs do not provide a value. Product identifiers found on linked product pages are labelled by type in `part_number`; cells remain blank where no identifier could be confirmed.

| category | component | manufacturer | part_number | details | status | notes | url |
|---|---|---|---|---|---|---|---|
| controller | Raspberry Pi Zero 2 W | Raspberry Pi |  | Main controller (MK, G); controller platform (MV) | required | Single-board computer |  |
| thermal | Waveshare aluminium heatsink | Waveshare |  | Thermal management | required | For Raspberry Pi Zero 2 W | https://elektronicavoorjou.nl/product/heatsink-raspberry-pi-zero/ |
| network | Waveshare USB Ethernet HUB HAT for Raspberry Pi Zero | Waveshare | `MPN: WS-19635` | Gigabit Ethernet and USB connectivity |  | Required for wired network connection | https://www.kiwi-electronics.com/nl/poe-ethernet-usb-hub-hat-voor-rpi-zero-10711 |
| network | PoE Ethernet / USB HUB HAT for Raspberry Pi Zero | Waveshare | `MPN: WS-19635` |  |  |  | https://www.berrybase.de/en/poe-ethernet-usb-hub-hat-for-raspberry-pi-zero-1x-rj45-3x-usb |
| network | Panel-mount Ethernet extension cable |  | `Kiwi product ID: 2542` | Ethernet connection | |  | https://www.kiwi-electronics.com/nl/panel-mount-ethernet-verlengkabel-2542 |
| power | Delock Micro USB connector | Delock | `EAN: 4043619852451` | Power input | |  | https://www.reichelt.com/nl/nl/shop/product/usb_2_0-kabel_micro_b-bus_naar_micro_b-stekker_0_25_m-212024
| power | DC-DC buck converter |  | `TinyTronics SKU: 002166` | 24 V to 5 V for Raspberry Pi |  | | https://www.tinytronics.nl/nl/power/spanningsconverters/buck-(step-down)-converters/dc-dc-step-down-buck-converter-2a-5v-met-usb |
| power | Panel-Mount Fuse Holders with screw cap |  |  |  |  |  | https://elektronica-online.nl/product/zekeringhouder-4590 |
| power | Glass fuse 5x20mm 1A fast |  |  |  |  |  | https://elektronica-online.nl/product/10-zekeringen-1a-snel-5x20--4411 |
| control | 5V 2-channel relay module |  | `TinyTronics SKU: 5VREL2HL` |  | | High-active or low-active variant | https://www.tinytronics.nl/en/switches/relays/relay-modules/5v-relay-2-channel-high-active-or-low-active |
| control | 5V 4-channel relay module |  | `BerryBase SKU/MPN: HLRELM-4` |  | |  | https://www.berrybase.de/en/5v-4-channel-relay-module-with-definable-switching-signal-high-low |
| control | 5V 8-channel relay module |  | `BerryBase SKU/MPN: HLRELM-8` |  | |  | https://www.berrybase.de/en/5v-8-channel-relay-module-with-definable-switching-signal-high-low |
| sensor | reed relais |  | `Model: MC-38` |  |  |  | https://www.tinytronics.nl/nl/schakelaars/magneetschakelaars/deur-schakelaar-reed-relais-met-magneet |
| sensor | 2-channel optocoupler module |  | `TinyTronics SKU: ISOBOARD2CH` | Signal isolation |  |  | https://www.tinytronics.nl/en/switches/optocouplers/optocoupler-isolation-module-2-channels |
| sensor | YF-B6 water flow sensor |  | `Model: YF-B6` | Water measurement | | G3/4 inch | https://www.tinytronics.nl/en/sensors/liquid/yf-b6-water-flow-sensor-brass-g3-4 |
| sensor | Shelly BLU H&T |  | `SKU: FG3800235266816`; `EAN: 3800235266816` |  | |  | https://www.shelly.com/products/shelly-blu-h-t-ivory |
| connector | GX12-2 connector |  | `Model: GX12-2`; `TinyTronics product ID: 1960` | | | | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-2-connector-set |
| connector | GX12-3 connector |  | `Model: GX12-3`; `TinyTronics product ID: 1961` | | | | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-3-connector-set |
| connector | GX12-4 connector |  | `Model: GX12-4`; `TinyTronics product ID: 1962` | | |  | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-4-connector-set |
| connector | GX12-5 connector |  | `Model: GX12-5`; `TinyTronics product ID: 1963` | | | | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-5-connector-set |
| connector | GX12-6 connector |  | `Model: GX12-6`; `TinyTronics product ID: 1964` | | | | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-6-connector-set |
| connector | GX12-7 connector |  | `Model: GX12-7`; `TinyTronics product ID: 1965` | | | | https://www.tinytronics.nl/en/cables-and-connectors/connectors/aviation-style/gx12/gx12-7-connector-set |
| connector | Removable screw terminal 2-pin 3.81mm |  | `TinyTronics SKU: REMSCREWT-3.81MM-2P` | Terminal connection | |  | https://www.tinytronics.nl/en/cables-and-connectors/connectors/screw-terminals/removable-screw-terminal-2p-3.81mm |
| assembly | BOX4U industrial enclosure |  | `Reichelt item no.: 324314` | Small enclosure for <=4 relay count | | 125 x 115 x 58 mm IP65 | https://www.reichelt.com/nl/nl/shop/product/industriele_behuizing_125_x_115_x_58_mm_ip65_lichtgrijs-324314 |
| assembly | Kradex enclosure | Kradex |  | Larger enclosure for >4 relay count | | 176 x 126 x 57 mm IP65 |  |
| assembly | Wago clamps |  |  | Terminal connections | required |  |  |
| assembly | Jumper cables heat shrink and masking tape |  |  | Wiring and assembly | required |  |  |



### History

- 2026-10-21: Added a consolidated inventory of unique components from the MKcontroller, MVcontroller and Gcontroller BOMs.
- 2026-10-21: Added product identifiers visible on linked supplier pages, labelling each identifier by type.
