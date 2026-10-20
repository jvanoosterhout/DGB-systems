# Requirements

These are selected requirements derived from the [system needs](system-context.md#needs-and-motivation). They describe desired behaviour, not a feature catalogue. Preferences such as affordability, low power use and approachable setup remain needs until a useful, agreed acceptance boundary exists. All requirements below are drafts; implementation or verification is not implied.

# DGB-system requirements

## SYS-REQ-001: Present HMD-DGB capabilities as meaningful devices in a central home automation platform

Status: draft  
Applies to: HMD-DGB and central home automation platform
Description: A user shall be able to configure, control and inspect HMD-DGB's meaningful device capabilities in a central home automation platform without needing to configure or operate the underlying GPIO pins individually in HMD-DGB. A device with several inputs, outputs and/or states shall be represented as one logical device.

### History

- 2026-10-08: Drafted from the needs for coherent user control and meaningful composite devices.


## SYS-REQ-002: Set service configurations from a central home automation platform 

Status: draft  
Applies to: Controllers
Description: A user shall be able to set and send configuration from a central platform a service. 

### History

- 2026-10-08: Drafted from the needs.

## SYS-REQ-003: Restore service configuration without loading it by hand

Status: draft  
Applies to: service and central home automation platform
Description: After replacement of a failed controller node or SD card, the intended service configuration shall be recoverable from a maintained central source without manually recreating or resending the service's device and pin configuration.

### History

- 2026-10-08: Drafted from the replacement-node goal; current setup still has manual installation and broker/Home Assistant configuration steps.

## SYS-REQ-004: Keep essential device operation local

Status: draft  
Applies to: DGB-systems and connected devices  
Description: Essential local device control shall remain available without a vendor-cloud service or internet connection. Optional remote or cloud features may be used when their added value justifies the dependency, provided the essential local operation is not lost.

### History

- 2026-10-08: Drafted from the user's local-first preference; no availability criterion or device-specific exception has been agreed.

## SYS-REQ-005: Defined behaviour for startup and failed configuration

Status: draft  
Applies to: HMD-DGB 
Description: Before bindings become active, startup behaviour for configured devices and outputs shall be defined. The user shall be able to chose from a list of startup behaviour. An incomplete or rejected configuration cycle shall not be treated as live and handle according to the startup behaviour. Each physical controller shall separately define the appropriate physical design for its own restart and failure conditions.

### History

- 2026-10-08: Drafted from the need for explicit startup and failure behaviour; intended physical states remain controller-specific.

# Hardware requirements

## HW-REQ-001: Custom controllers shall be build form a standardized components. 

Status: draft  
Applies to: Controllers
Description: Custom controllers shall be constructed with components from a selected list of parts for enclosure, connectors, power, data/network, so that it can be assembled and serviced consistently. 

### History

- 2026-10-08: Drafted from the needs.

## HW-REQ-002: Each node shall use as little as possible power

Status: draft  
Applies to: Node and HMD-DGB
Description: Each node shall be configured such that power consumption is minimized. 

### History

- 2026-10-08: Drafted from the needs. "as little as possible" has no pass criterion, though there is no alternative jet as power cannot be measured. The aim it to, one day, experiment with throttling the Pi zero devices while keeping performance.  

- 2023: power consumption on a pi 4 with Poe is an enormous 6w. Poe and main Pi boards where abandoned for that reason. Pi zero boards were considered instead.  

## HW-REQ-003: Controllers shall be or use trustworthy components. 

Custom-built controllers (and personally this also holds for commercial gateways) shall use components with documented specifications, an identifiable supplier or source and can be at least assumed to have a reasonable lifespan with support. Any component with uncertain support or lifespan shall be called out in the controller documentation.

### History

- 2026-10-08: Drafted from the needs.

# Software requirements

## SW-REQ-001: A user shall be able to define GPIO as inputs for a service on a controller.

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to configure GPIO as inputs or sensors given a predefined set of configuration capabilities. A minimum set consists of a binary sensor and a counter. Preferably these are extended by common GPIO sensors with a variety of protocols. The configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-002: A user shall be able to define GPIO as outputs for a service on a controller.

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to configure GPIO as outputs or actors given a predefined set of configuration capabilities. A minimum set consists of an on/off state. Preferably these are extended by serial, PWM and other control signals. The configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-003: A user shall be able to define entity metadata for a service on a controller.

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to configure entity metadata to allow the service to configure this entity in the central home automation platform. The configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-004: A user shall be able to associate related entities with one logical device by defining its metadata for a service on a controller.

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to associate related entities with one logical device via configurations to allow the service to configure this device and relations in the central home automation platform. The configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-005: A user shall be able to configure bindings supported GPIO, entities and themselves.

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to configure bindings between supported GPIO and entities and themselves. The bindings shall trigger on an event, that is an entity command inputs or GPIO sensor states. The binding configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-006: Execute actions only for matching configured events

Status: draft  
Applies to: HMD-DGB
Description: The binding shall execute a configured action only when the required events match their configured values and conditions in the intended sequence or time window. An incomplete or mismatched event combination shall not trigger that action. The binding configuration will be stored on, and send from, the central home automation platform, like in SYS-REQ-002. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-007: Third party packages shall be reasonably trustworthy, maintained and have a good documentation and/or active community 

Status: draft  
Applies to: Controllers
Description: Each HMD-DGB dependency shall have documentation, active maintenance and can reasonably be trusted by means of community feedback or support. Dependencies without active maintenance or adequate documentation require a documented reason for continued use. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-008: Restart service (cleanly) from a central home automation platform 

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to request a service restart on a node from a central platform. The node shall report or expose whether it returned to operation. There shall be an options to: 
- Restart
- Restart and clear all exposed devices on the home automation platform, 
- Restart and clear all configurations and all exposed content. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-009: Service's essential functions shall maintain available in case of a system crash due to user misconfiguration. 

Status: draft  
Applies to: Controllers
Description: A rejected or invalid configuration or a bug in catching these, shall not prevent the service's essential functions from operating. This means that the Node and Service device in the central home automation platform remain functional. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-010: The service switches to safe mode at connectivity failure 

Status: draft  
Applies to: Controllers
Description: When a node loses its connection to the central home automation platform, it shall apply a user specified fallback behaviour for each affected output. The user can configure this fallback behaviour from a list of configuration options. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-011: Install nodes with minimal effort 

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to install and provision a node by following one documented procedure that identifies required manual steps, prerequisites and required configuration. "Minimal" means maximally these main steps: install the controller OS and install/configure HMD-DGB with controller specific parameters.

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-012: Update services from a central home automation platform 

Status: draft  
Applies to: HMD-DGB
Description: A user shall be able to initiate a HMD-DGB software update from the central environment and determine whether the update completed or failed. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-013: The software shall be structured such that it supports human readability, human understandability and automated unit and integration testing. 

Status: draft  
Applies to: HMD-DGB
Description: The software shall be structured so that supported behaviour can be understood easily, and automated unit, smoke and integration tests can exercise its functioning easily. 
- Unit tests shall cover 100% of the code. 
- Smoke tests shall cover default HMD-DGB configuration with broker and a basic configuration like the[MQTT_duration_run](https://github.com/jvanoosterhout/HMD-DGB/blob/main/Examples/venv_MQTT_project/MQTT_duration_run.py) example. 
- Integration tests shall cover each module and modules with the core. 

### History

- 2026-10-08: Drafted from the needs.

## SW-REQ-014: The software shall be able to work for at least 31 days. 

Status: draft  
Applies to: HMD-DGB
Description: The software shall not crash or restart unintendedly during at least 31 days under normal operations with hourly interaction. 

### History

- 2026-10-19: Drafted from the needs.
