---
title: "PLDM SPEC QUESTION"
date: 2025-09-25 00:00:00  +0500
categories: [Server]
tags: [BMC]
---

Based on the PLDM Base Specification, here are the **most important technical questions** for a BMC Engineer position, focusing on practical implementation and debugging:

## **1. PLDM Message Structure & Interpretation**
**Question:** "Walk me through the exact byte-by-byte structure of a PLDM request message. How would you decode a PLDM packet captured from the wire to determine what command is being executed?"

**What they're testing:** Understanding of the fundamental PLDM protocol structure.

**Key elements to cover:**
- **Byte 0:** Rq/D bits, Instance ID (5 bits), Header Version (2 bits)
- **Byte 1:** PLDM Type (6 bits) - identifies the protocol type (0x02=Monitoring, 0x0F=Firmware Update, etc.)
- **Byte 2:** PLDM Command Code - specific command within the type
- **Byte 3+:** Payload specific to the command
- **Importance of Instance ID** for matching requests with responses

## **2. PLDM Discovery Sequence**
**Question:** "As a BMC engineer, what's the exact sequence of PLDM commands you would use to discover what capabilities a newly connected PCIe device supports?"

**What they're testing:** Knowledge of the PLDM initialization workflow.

**Expected answer sequence:**
1. **GetTID** (0x02) - Get Terminus ID
2. **GetPLDMTypes** (0x04) - Discover supported PLDM types
3. **GetPLDMVersion** (0x03) - For each supported type, get version info
4. **GetPLDMCommands** (0x05) - Discover supported commands for each type
5. **SelectPLDMVersion** (0x06) - Optionally select specific version
6. **Type-specific discovery** (e.g., get sensors, firmware parameters)

## **3. Multipart Transfers for Firmware Updates**
**Question:** "Describe how PLDM multipart transfers work for a large firmware image update. What are the critical states and error handling mechanisms?"

**What they're testing:** Understanding of large data transfer mechanisms.

**Key points:**
- **NegotiateTransferParameters** first to agree on part size
- **MultipartSend** state machine: START → MIDDLE → END or START_AND_END
- **Error recovery:** XFER_CURRENT_PART (retry), XFER_FIRST_PART (restart section)
- **CRC-32 checksum** validation for each part
- **DataTransferHandle** management for tracking transfer state

## **4. PLDM Completion Codes & Error Handling**
**Question:** "You receive a PLDM response with completion code 0x84. What does this mean and how would you troubleshoot it?"

**What they're testing:** Practical debugging skills.

**Key completion codes to know:**
- **0x00:** SUCCESS
- **0x01:** ERROR (generic)
- **0x03:** ERROR_INVALID_LENGTH
- **0x05:** ERROR_UNSUPPORTED_PLDM_CMD
- **0x20:** ERROR_INVALID_PLDM_TYPE
- **0x84:** ERROR_INVALID_PLDM_VERSION_IN_REQUEST_DATA

## **5. Real-world Implementation Challenge**
**Question:** "You're implementing sensor monitoring using PLDM Type 0x02 (Platform Monitoring and Control). How do you handle the case where a sensor reading request times out? What's your retry strategy?"

**What they're testing:** Understanding of PLDM timing and reliability.

**Key timing parameters:**
- **PT1:** Request-to-response time (max 100ms)
- **PT2:** Timeout waiting for response
- **PT3:** Instance ID expiration (5-6 seconds)
- **Retry strategy:** Original try + 2 retries within PT3 window

## **6. PLDM over MCTP Integration**
**Question:** "How does PLDM integrate with MCTP in the BMC software stack? What happens when a PLDM message arrives over PCIe VDM?"

**What they're testing:** Understanding of the full stack integration.

**Stack flow:**
1. **Hardware:** PCIe VDM packet received
2. **MCTP Layer:** Extract MCTP packet, route by EID
3. **PLDM Layer:** Parse PLDM header, route by PLDM Type
4. **Application:** PLDM daemon processes the specific command
5. **Response:** Reverse the flow back to requester

## **7. Critical PLDM Data Types**
**Question:** "What's the difference between ver32 encoding and a regular uint32? How would you encode version '2.1.5b' in ver32 format?"

**What they're testing:** Attention to specification details.

**ver32 encoding specifics:**
- **Bytes 0-3:** Major, Minor, Update (BCD-encoded), Alpha (character)
- **Version 2.1.5b** → 0xF2F1F562
- **Special values:** 0xFF in update field = field not present
- **0xFFFFFFFF** = no specific version

These questions test the practical knowledge needed to implement, debug, and maintain PLDM functionality in a BMC - which is exactly what a BMC Engineer would do daily. The multipart transfer and discovery sequence questions are particularly valuable as they cover complex, real-world scenarios.
