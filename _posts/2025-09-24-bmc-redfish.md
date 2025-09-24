---
title: "REDFISH"
date: 2025-09-24 00:00:00  +0500
categories: [Server]
tags: [BMC]
---

### **Redfish Interview Questions for a BMC Engineer**

#### **Category 1: Foundational Concepts & Architecture**

**Question 1: What is Redfish, and how does it improve upon previous management interfaces like IPMI?**

**What the interviewer is looking for:** Understanding of Redfish's value proposition and architectural advantages.

**Sample Answer:**
"Redfish is a modern, standards-based RESTful API designed for out-of-band systems management, spearheaded by DMTF. It's a significant improvement over IPMI:

*   **Protocol & Data Format:** Redfish uses HTTPS and JSON, which are web-standard, human-readable, and easy to integrate with modern automation tools (Ansible, Python scripts). IPMI uses a proprietary binary protocol over UDP, which is more complex to work with.
*   **Data Model:** Redfish has a rich, hierarchical, and discoverable data model. You can navigate from the root service (`/redfish/v1`) to specific resources like systems, managers, and chassis. IPMI has a flat command set.
*   **Scalability:** Redfish is designed for large-scale data centers and composable infrastructure. Its resource-oriented design scales better than IPMI's single-server focus.
*   **Security:** Redfish mandates HTTPS with strong authentication, while IPMI often had weak security defaults.
*   **Standardization:** Redfish provides a standardized schema for hardware, making multi-vendor management much more consistent than with the vendor-specific extensions common in IPMI."

---

**Question 2: Describe the core Redfish data model. What are the key top-level resources starting from the Redfish Service Root?**

**What the interviewer is looking for:** Knowledge of the fundamental structure used to model a system.

**Sample Answer:**
"The model is a logical representation of the hardware. Starting from the Service Root (`/redfish/v1`), the key resources are:

1.  **`Systems` (`/redfish/v1/Systems`):** This collection represents the logical compute nodes. Each `System` resource contains properties like power state, boot settings, processor/memory information, and host OS-related controls.
2.  **`Managers` (`/redfish/v1/Managers`):** This collection represents the management controllers—typically the BMC itself. It's the entry point for BMC-specific functions like network settings, log services, virtual media, and the BMC's own firmware inventory.
3.  **`Chassis` (`/redfish/v1/Chassis`):** This collection represents the physical enclosure(s). It contains properties for physical assets (SKU, serial number), thermal sensors (temperatures, fans), power supplies, and indicators (LEDs). A single chassis can contain multiple `Systems` (e.g., a blade enclosure).

The elegance is in the separation of concerns: `System` for logical compute, `Manager` for control, and `Chassis` for physical attributes. These resources are linked together using the `@odata.id` property."

---

#### **Category 2: API Interaction & Operations**

**Question 3: Walk me through the exact HTTP calls you would make to gracefully shut down a server and then power it back on using Redfish.**

**What the interviewer is looking for:** Practical knowledge of using the API for a common task.

**Sample Answer:**
"Sure. First, I need to find the specific system. I'd start with discovery and then use the `Actions` mechanism.

1.  **Discover the System Resource:**
    ```
    GET https://<BMC_IP>/redfish/v1/Systems
    Authorization: Basic <credentials>
    ```
    The response will contain a `Members` array. I'd select the first member (e.g., `Members[0]@odata.id` = `/redfish/v1/Systems/1`).

2.  **Graceful Shutdown:** This is done by sending a `POST` to the `Actions` endpoint within the System resource. The action is `ComputerSystem.Reset`.
    ```
    POST https://<BMC_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset
    Authorization: Basic <credentials>
    Content-Type: application/json

    {
        "ResetType": "GracefulShutdown"
    }
    ```
    This commands the BMC to request a clean shutdown from the host OS.

3.  **Power On:** Once the system is off, I'd power it back on using the same action but with a different `ResetType`.
    ```
    POST https://<BMC_IP>/redfish/v1/Systems/1/Actions/ComputerSystem.Reset
    Authorization: Basic <credentials>
    Content-Type: application/json

    {
        "ResetType": "On"
    }
    ```
    Other common `ResetType` values are `ForceOff`, `ForceRestart`, `PushPowerButton`, and `Nmi`."

---

**Question 4: How would you change the one-time boot source to PXE using Redfish?**

**What the interviewer is looking for:** Understanding of boot configuration, a critical lifecycle management task.

**Sample Answer:**
"This involves modifying the `Boot` object within the `System` resource. The goal is to set a one-time override.

1.  **Retrieve the Current Boot Configuration:**
    ```
    GET https://<BMC_IP>/redfish/v1/Systems/1
    ```
    Look for the `Boot` property in the response.

2.  **Patch the Boot Object:** I would use the `PATCH` method to update the `BootSourceOverrideTarget` for the next boot only.
    ```
    PATCH https://<BMC_IP>/redfish/v1/Systems/1
    Authorization: Basic <credentials>
    Content-Type: application/json

    {
        "Boot": {
            "BootSourceOverrideTarget": "Pxe",
            "BootSourceOverrideEnabled": "Once"
        }
    }
    ```
    *   `BootSourceOverrideTarget`: Set to `Pxe` to specify the boot device.
    *   `BootSourceOverrideEnabled`: Set to `"Once"` to make this a one-time change. Other values are `"Continuous"` (persistent) or `"Disabled"`.

After this PATCH, the next server reset will cause it to attempt a PXE boot."

---

#### **Category 3: Eventing & Advanced Features**

**Question 5: Explain how Redfish Eventing works. How does a subscriber get notified about events like a temperature threshold being exceeded?**

**What the interviewer is looking for:** Knowledge of the push-based notification model.

**Sample Answer:**
"Redfish uses a publish-subscribe model for eventing.

1.  **Subscription:** An external client (the subscriber) registers with the BMC's Event Service by `POST`ing to the `Subscriptions` collection.
    ```
    POST /redfish/v1/EventService/Subscriptions
    {
        "Destination": "https://my-listener.com/events",
        "EventTypes": ["Alert"], // Or "StatusChange", "ResourceUpdated"
        "Context": "MyAppSubscription"
    }
    ```
    This tells the BMC: 'When an event occurs, send an HTTP POST to my webhook URL (`Destination`).'

2.  **Event Trigger:** When an event occurs on the BMC (e.g., a CPU temperature sensor exceeds its critical threshold), the BMC generates an event message.

3.  **Notification:** The BMC serializes the event into a JSON payload and `POST`s it to all subscribed `Destination` URLs. The payload includes details like the event type, message ID, severity, and a link to the originator resource (e.g., the thermal sensor that triggered the alert).

This is far superior to the IPMI model of constantly polling SEL (System Event Log), as it provides real-time, targeted notifications."

---

**Question 6: Describe the process of performing a BMC firmware update via Redfish.**

**What the interviewer is looking for:** Understanding of the update workflow, which often uses the Task Service.

**Sample Answer:**
"The update is an action targeted at the `Manager` resource (the BMC itself). It typically involves the Task Service for long-running operations.

1.  **Locate Update Service:** Find the `UpdateService` resource from the Service Root.
2.  **HTTP PUT/POST the Firmware Image:** The common method is to `POST` a multipart/form-data request to the `UpdateService` `SimpleUpdate` action or, more commonly, to `PUT` the image directly to a software inventory resource.
    ```
    POST /redfish/v1/UpdateService/Actions/UpdateService.SimpleUpdate
    {
        "ImageURI": "http://my-server/bmc-firmware.img"
    }
    ```
3.  **Task Monitored Update:** The BMC often creates a `Task` resource to represent the update job. The response will include a link to this task (`Task@odata.id`).
4.  **Poll the Task:** I would periodically `GET` the Task resource to check its state (`Running`, `Completed`, `Exception`) and progress (percentage complete).
5.  **Reset the BMC:** Upon successful update, the BMC will usually need to be reset to activate the new firmware, which can also be triggered via a `Reset` action on the `Manager` resource."

---

#### **Category 4: Implementation & Debugging**

**Question 7: A user reports that a PATCH request to modify a property is returning a '405 Method Not Allowed' error. What are the potential causes?**

**What the interviewer is looking for:** Debugging skills for API issues.

**Sample Answer:**
"A `405` error indicates the server understands the request but won't fulfill it. The causes are typically:

1.  **Read-Only Property:** The most common cause. The property I'm trying to PATCH is defined as read-only in the Redfish schema. I need to check the `PATCH` body to ensure I'm only attempting to write writable properties.
2.  **Incorrect Resource URI:** I might be trying to PATCH a collection (e.g., `/redfish/v1/Systems`) instead of a specific member resource (e.g., `/redfish/v1/Systems/1`). PATCH is only allowed on individual resources.
3.  **Concurrency Control (ETag):** The resource might be protected by the `If-Match` header using ETags. If another client has modified the resource since I last read it, my PATCH will be rejected unless I provide the correct `ETag` in the `If-Match` header to prevent stale updates.
4.  **Privilege Level:** My authentication token might not have the necessary privileges (e.g., 'ConfigureManager' role) to perform the write operation.

My first step would be to double-check the Redfish schema for that resource to verify the property is writable."

---

**Question 8: How does the BMC translate an internal action, like a PLDM command to read a sensor, into the corresponding Redfish resource state?**

**What the interviewer is looking for:** Insight into the BMC's internal software architecture—the bridge between southbound and northbound interfaces.

**Sample Answer:**
"This is the core responsibility of the BMC's management software stack, often called the 'Redfish Provider' or 'Middleware.'

1.  **Data Collection:** The BMC has background daemons that constantly poll or receive events from the hardware. For example, a `hwmon` daemon might read temperature sensors via I2C, or a PLDM daemon might receive sensor updates from other components.
2.  **Internal Data Model:** These daemons update an internal database (e.g., D-Bus objects in OpenBMC) that represents the current state of the entire platform.
3.  **Redfish Interface Layer:** The Redfish REST server (e.g., bmcweb in OpenBMC) is separate. When an HTTP `GET` request comes in for, say, `/redfish/v1/Chassis/chassis1/Thermal`, the Redfish server:
    *   Parses the request.
    *   Queries the internal data model (via D-Bus) to get the current temperature values.
    *   Formats this data into a JSON structure that conforms to the official Redfish `Thermal` schema.
    *   Sends the JSON response back to the client.

The translation is not direct; it's a two-step process: hardware protocol -> internal model -> Redfish JSON. This abstraction allows the Redfish interface to remain consistent even if the underlying hardware protocol changes from IPMI to PLDM."


