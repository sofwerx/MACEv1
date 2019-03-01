# Project MACE : Military Applications of Cyber Effects
- [Overview](#overview)
- [Assumptions](#assumptions)
- [Phases](#phases)
   - [x] [Phase 1: Discovery & Reconnaissance](#current-phase)
     - Visual Identification of Devices
     - Network Identification of Devices
     - RF Identification of Devices
     - Device Meta-Data
     - Data Warehousing
   - [ ] Phase 2: Data Ingestion & Capabilities
   - [ ] Phase 3: Employment of Capabilities


## Overview
This project is managed under the JIRA Epic Task [Mace-1](https://jira.supermicro0.opswerx.org/projects/MACE/issues/MACE-1). Project MACE is an ongoing effort to leverage existing technologies and the internet of space to the advantage of warfighters whose arena is moving rapidly towards a mixed reality of physical and cyber space. Given the assumptions below, modern warfighters have a need to:

## Assumptions
Project MACE is based on the following assumptions:
  1.  SOF forces of the future will operate in ultra-dense urban environments (i.e., Brazilian favelas, New Delhi, etc.)
  2.  Based on assumption 1, space-based assets will not be able to penetrate to the ground to provide the required intelligence.
  3.  Based on assumption 1, deployment of munitions from air assets will result in an unacceptable collateral damage estimate.
  4.  The second and third world will adapt Internet of Things (IoT) and ICS/SCADA faster than the U.S. due to an absence of legacy infrastructure.
  5.  Based on assumptions 1 & 4, the electromagnetic environment will present particular challenges to tradition RF capabilities such as terrestrial radio.
  6.  Based upon assumption 1, 4, 5, satellite communications will also be unsuitable due to a lack of ability to see the horizon without making our operators’ significant targets.

## Phases
The assumptions above require a re-examination of capabilities that are typically provided by support elements to improve efficiency and war fighter capabilities. Our warfighters need, with some degree of imediacy, to be able to:
  1. Conduct a rapid survey of both physical and electromagnetic space
  2.  Quickly assess, map, and integrate knowledge gained from step 1 in order to provide offensive, defensive and support capabilities based on the physical an EM environment. These capabilities include a necessity to visualize large amounts of information in coordination with geographic location in a GPS-degraded or denied environment.
  3.  Conduct operations in ultra-dense urban environments while taking advantage of embedded capabilities and opportunities already present in the environment.  These capabilities need to be employed with a minimum of operator interaction and must rely on new methods of information visualization and non-traditional human-computer interfaces

These objectives can be condensed into three phases:
- [x] [Phase 1: Discovery & Reconnaissance](#current-phase)
- [ ] Phase 2: Data Ingestion & Capabilities
- [ ] Phase 3: Employment of Capabilities

### Current Phase
As of 03.01.2019, Project MACE is in the midst of its Discovery and Reconnaissance phase, in which we intend to assemble tools capable of (1) obtaining multiple device signatures from and area and (2) compiling these signatures in a single database or warehouse capable of (3) querying device meta-data from one or more signatures.
