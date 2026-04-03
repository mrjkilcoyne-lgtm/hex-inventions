# PATENT APPLICATION (UK IPO FORMAT)

## 1. TITLE OF INVENTION

Multi-Modal Edge-Computing Food Freshness Inference System for Refrigeration Appliances

## 2. APPLICANT

Name: Matthew Kilcoyne
Address: [UK correspondence address]
Nationality: British

## 3. INVENTOR(S)

Name: Matthew Kilcoyne
Address: [Address]

## 4. DESCRIPTION

### 4.1 TECHNICAL FIELD

The invention relates to food storage monitoring systems, particularly to refrigeration appliances equipped with multi-modal sensor arrays and edge computing processors for on-device inference of food freshness states without cloud connectivity dependency.

### 4.2 BACKGROUND

Existing smart refrigerator systems (e.g., Samsung Family Hub, LG InstaView ThinQ) employ internal cameras that capture images of stored food items and transmit these images to remote cloud servers for processing and identification. Such systems suffer from several technical limitations:

First, cloud-dependent architectures introduce latency of 2-4 seconds per recognition event and fail completely when network connectivity is unavailable, rendering the smart features non-functional.

Second, current systems rely solely on visual identification of food items without assessing the actual biochemical state of the food. No commercially available smart refrigerator system measures or infers the freshness state of stored food items using objective sensor data.

Third, existing systems rely entirely on manufacturer-printed date labels (e.g., "best before", "use by") as freshness proxies. Research by the USDA and WRAP (UK) demonstrates that over 80% of consumers misinterpret these labels, leading to premature disposal of safe food and, conversely, consumption of spoiled food past its actual safe window.

Fourth, studies published by ReFED and USDA indicate that household food waste in the United States exceeds $99 billion annually, with approximately 40% of discarded refrigerated food still being within safe consumption parameters at the time of disposal.

There exists a need for a refrigeration monitoring system that objectively determines food freshness through direct measurement of food state indicators, operates entirely on-device without cloud dependency, and provides actionable guidance to reduce food waste.

### 4.3 SUMMARY OF THE INVENTION

The invention provides a food freshness inference system for a refrigeration appliance, the system comprising a multi-modal sensor array including visual, chemical, gravimetric, humidity, and temperature sensors, and an edge computing processor configured to fuse data from all sensor modalities to generate a freshness score for each stored food item, wherein all inference is performed on the device without requiring transmission of sensor data to a remote server.

### 4.4 DETAILED DESCRIPTION

#### Preferred Embodiment 1: Integrated Smart Refrigerator

The system comprises the following components installed within a refrigeration appliance:

**Sensor Array**:

(a) Visual sensors: At least one camera per refrigerator compartment, preferably a 5-megapixel wide-angle camera module, positioned to capture images of stored items on each shelf. The cameras operate in visible light during door-open events and under infrared illumination during periodic inventory scans with the door closed.

(b) Chemical gas sensors: An array of metal-oxide semiconductor (MOS) gas sensors positioned in the upper region of each compartment to detect volatile compounds associated with food spoilage. The array comprises at minimum: an ethylene sensor (sensitivity range 0.1-100 ppm) for detecting fruit and vegetable ripening; an ammonia sensor (sensitivity range 1-500 ppm) for detecting protein decomposition in meat, fish, and dairy; and a volatile organic compound (VOC) sensor for general spoilage detection.

(c) Gravimetric sensors: Per-shelf load cells integrated into or beneath each shelf surface, with resolution of at least 5 grams, configured to measure the weight of items on each shelf and detect weight changes over time indicative of dehydration (produce), gas loss (fermentation), or moisture absorption (bread, cereals).

(d) Environmental sensors: Per-zone humidity sensors (accuracy ±2% RH) and temperature sensors (accuracy ±0.5°C) providing ambient condition data for each compartment and zone.

**Edge Computing Module**:

A processing module comprising an ARM Cortex-A55 or equivalent processor, at least 2GB RAM, and at least 8GB non-volatile storage, running a local operating system and inference engine. The module executes:

(a) A food identification model: a convolutional neural network (preferably MobileNetV3-Small or equivalent, quantised to INT8, approximately 4MB) that processes camera images to identify food items and their locations within the appliance.

(b) A multi-modal freshness inference model: a transformer-based fusion model (approximately 2MB quantised) that receives as inputs: a visual feature embedding from the food identification model; normalised gas sensor readings; weight delta from entry weight; current humidity and temperature readings; and elapsed time since item entry. The model outputs a freshness score on a scale of 0 to 100 for each tracked item.

(c) A local inventory database: storing item identity, entry timestamp, location (shelf and zone), entry weight, current freshness score, and freshness trajectory. The database is encrypted with a key derived from a user-provided passphrase and is stored exclusively on the device.

**Freshness Score Calculation**:

The freshness inference model employs a cross-attention mechanism with at least 6 attention heads across 3 transformer layers to learn correlations between the sensor modalities. The model is trained on data comprising:

- Published food science decay curves from the USDA FoodData Central database and equivalent international databases, parameterised by food category, temperature, and humidity;
- Synthetic training data generated by sampling from these decay curves with added noise representative of real sensor variance;
- Optionally, real sensor readings collected from prototype deployments.

The freshness score is calibrated such that a score of 100 indicates peak freshness at time of entry, a score of 50 indicates the item is past peak quality but safe for consumption, a score of 20 indicates the item should be consumed immediately or discarded, and a score of 0 indicates the item is unsafe for consumption.

**User Interface**:

The freshness scores and inventory data are presented to the user via:

(a) A display mounted on the refrigerator door, showing a visual inventory with colour-coded freshness indicators (green/amber/red);

(b) A mobile application that communicates with the refrigerator via local network protocols (mDNS service discovery, REST API over HTTPS on the local network) without requiring any cloud relay;

(c) Optional voice interface using an on-device speech recognition model.

**Waste-Priority Meal Planning Module**:

An on-device meal planning engine that:

(a) Maintains a local recipe database;
(b) Prioritises recipe suggestions based on the freshness urgency of available ingredients;
(c) Generates notifications when items approach low freshness thresholds, including specific recipe suggestions that would utilise those items.

#### Preferred Embodiment 2: Retrofit Module

The sensor array and edge computing module are packaged as a self-contained retrofit unit mountable inside any existing refrigerator. The unit comprises:

- A compact sensor module per shelf (camera + gas sensor + load cell integrated into a shelf liner or clip-on unit)
- A central processing hub attached to the interior side wall of the refrigerator
- Power supplied via a cable routed through the door seal to an external power adapter
- Communication via Wi-Fi to the user's mobile device (local network only)

#### Preferred Embodiment 3: Commercial Refrigeration

The system is scaled for commercial refrigeration units in food service and retail environments, with:

- Higher-resolution cameras and additional gas sensors per unit
- Network of multiple units reporting to a local server (on-premises, not cloud)
- Integration with point-of-sale systems for dynamic pricing based on freshness scores
- Inventory management dashboard for food service operators

### 4.5 BRIEF DESCRIPTION OF DRAWINGS

Figure 1: System architecture block diagram showing sensor array, edge computing module, and user interfaces.

Figure 2: Multi-modal freshness inference model architecture showing data flow from sensor inputs through fusion network to freshness score output.

Figure 3: Data flow diagram comparing cloud-dependent prior art architecture with the edge-first architecture of the invention.

Figure 4: Exploded view of retrofit module showing per-shelf sensor units and central processing hub.

## 5. CLAIMS

**Claim 1** (Independent — Apparatus):
A food freshness monitoring system for a refrigeration appliance, the system comprising:
a multi-modal sensor array including at least a visual sensor, a chemical gas sensor array, and a gravimetric sensor;
an edge computing processor configured to receive data from the multi-modal sensor array;
a food identification module executing on the edge computing processor, configured to identify food items from visual sensor data;
a freshness inference module executing on the edge computing processor, configured to fuse data from at least three sensor modalities including visual data, chemical gas data, and gravimetric data to compute a freshness score for each identified food item;
and a local storage module configured to store food item identities and associated freshness scores;
wherein the freshness inference module operates entirely on the edge computing processor without requiring transmission of sensor data to a remote server.

**Claim 2**:
The system of claim 1, wherein the chemical gas sensor array comprises at least an ethylene sensor, an ammonia sensor, and a volatile organic compound sensor.

**Claim 3**:
The system of claim 1, wherein the freshness inference module comprises a transformer-based fusion network with cross-attention mechanism configured to learn correlations between the at least three sensor modalities.

**Claim 4**:
The system of claim 1, wherein the gravimetric sensor comprises per-shelf load cells configured to detect weight changes in stored items over time.

**Claim 5**:
The system of claim 1, further comprising environmental sensors measuring humidity and temperature per compartment zone, wherein the freshness inference module additionally receives humidity and temperature data as inputs.

**Claim 6**:
The system of claim 1, further comprising a meal planning module configured to suggest recipes prioritised by freshness urgency of available ingredients, wherein the meal planning module executes on the edge computing processor using the freshness scores from the freshness inference module.

**Claim 7**:
The system of claim 1, wherein the local storage module stores food item data in an encrypted database, the encryption key being derived from a user-provided passphrase, such that stored data is inaccessible to the appliance manufacturer.

**Claim 8**:
The system of claim 1, further comprising a local network communication module configured to transmit inventory and freshness data to a user mobile device via a local area network without routing through any remote server.

**Claim 9**:
The system of claim 1, wherein the food identification module comprises a quantised convolutional neural network occupying less than 10 megabytes of storage, and the freshness inference module comprises a quantised multi-modal fusion model occupying less than 5 megabytes of storage.

**Claim 10** (Independent — Method):
A method for determining food freshness in a refrigeration appliance, comprising:
capturing visual data of food items stored in the appliance using at least one camera;
measuring chemical gas concentrations in the appliance atmosphere using a gas sensor array;
measuring weight of food items using gravimetric sensors;
identifying food items from the visual data using a food identification model executing on an edge computing processor within or attached to the appliance;
fusing the visual data, chemical gas data, and gravimetric data using a multi-modal freshness inference model executing on the edge computing processor to compute a freshness score for each identified food item;
storing the freshness scores in a local database on the edge computing processor;
wherein the method is performed entirely on the edge computing processor without transmitting sensor data to a remote server.

**Claim 11**:
The method of claim 10, further comprising measuring humidity and temperature within the appliance and providing said measurements as additional inputs to the multi-modal freshness inference model.

**Claim 12**:
The method of claim 10, further comprising computing a freshness trajectory for each food item based on sequential freshness scores over time, and generating an alert when the freshness trajectory predicts the item will reach a predetermined minimum freshness threshold within a configurable time window.

**Claim 13**:
The method of claim 10, further comprising selecting and presenting recipe suggestions that prioritise ingredients with lower freshness scores.

**Claim 14**:
The method of claim 10, wherein the freshness score computation has a latency of less than 200 milliseconds per food item.

**Claim 15** (Independent — System):
A food waste reduction system comprising:
a food freshness monitoring system according to claim 1;
a user application configured to receive inventory and freshness data from the monitoring system via a local network connection;
and a waste-priority meal planning engine configured to generate recipe suggestions that prioritise ingredients approaching freshness thresholds;
wherein the system operates without requiring cloud connectivity for any monitoring, inference, or recommendation function.

**Claim 16**:
The system of claim 15, wherein the food freshness monitoring system is packaged as a retrofit module configured for installation within an existing refrigeration appliance without modification to the appliance.

**Claim 17**:
The system of claim 15, further comprising an open application programming interface specification for the local network communication, enabling interoperability with third-party applications and appliances.

## 6. ABSTRACT

A food freshness monitoring system for refrigeration appliances employing a multi-modal sensor array comprising visual, chemical gas, gravimetric, humidity, and temperature sensors. An edge computing processor fuses data from all sensor modalities using a transformer-based multi-modal inference model to compute a freshness score for each stored food item. All processing occurs on-device without cloud dependency. The system tracks food inventory, predicts freshness trajectories, and provides waste-priority meal planning suggestions. Sensor array BOM cost is approximately $14, enabling widespread adoption as both integrated appliance feature and retrofit module. (Figure 1)

## 7. DRAWINGS

See `diagrams/` directory for Mermaid source files.

---

## NOTES ON FILING STRATEGY

1. **File UK IPO first** (£30 online filing + £150 search fee) — establishes priority date
2. **Within 12 months**: File PCT international application for global coverage
3. **Key jurisdictions**: UK, EU (EPO/Unitary Patent), US, Japan, South Korea, China (major appliance manufacturing markets)
4. **Freedom to operate**: Samsung's existing patents cover cloud-based food recognition and touchscreen refrigerator interfaces. The edge-first multi-modal freshness inference approach does not infringe these — it is architecturally distinct (on-device processing, no cloud requirement, multi-modal sensor fusion for freshness rather than identification).
5. **Strongest claims**: Claims 1, 10, and 15 are independent claims covering apparatus, method, and system respectively. The multi-modal fusion of visual + chemical + gravimetric data for on-device freshness inference is the novel core. No prior art combines all three modalities for edge-computed freshness scoring.
