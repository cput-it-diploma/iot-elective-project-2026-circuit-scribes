[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/AnR2QgvN)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22952764&assignment_repo_type=AssignmentRepo)
# 🌐 IoT Elective Project 2026
### Cape Peninsula University of Technology — IT Diploma
**Module:** Internet of Things (IoT) Elective | **Year:** 2026

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Group Members](#group-members)
3. [Project Idea & Problem Statement](#project-idea--problem-statement)
4. [System Architecture & Design](#system-architecture--design)
5. [Hardware Components](#hardware-components)
6. [Software & Technologies](#software--technologies)
7. [Circuit Diagram / Wiring](#circuit-diagram--wiring)
8. [Build Process (with photos)](#build-process-with-photos)
9. [Code Documentation](#code-documentation)
10. [Testing & Results](#testing--results)
11. [Challenges & Solutions](#challenges--solutions)
12. [Project Demonstration](#project-demonstration)
13. [References](#references)
14. [Assessment Rubric](#assessment-rubric)

---

## 📌 Project Overview

**Project Title:** `Smart Car System`  
**Group Name / Number:** `circuit-scribes`  
**Presentation Date:** 20 May 2026

---

## 👥 Group Members

| Student Name | Student Number | Role / Responsibility |
|---|---|---|
| Mvuyisi Mchithwa  | 220452709 | Hardware Lead |
| Jayden Avontuur | 222032278 | Hardware Lead |
| Stephanie Tola Oluwafemi Lewu | 230211216 | Documentation Lead |
| Mpumelelo Sithole  | 230526934 | Testing Lead |
| Owen Jnr Makene | 223219665 | Coding Lead |

---

## 💡 Project Idea & Problem Statement

### Problem Statement
Car theft remains a major issue, especially when traditional security methods such as keys, remotes, or PIN-based systems can be stolen, duplicated, or bypassed. There is a need for a more secure and intelligent system that ensures only authorized individuals can start a vehicle.

### Proposed Solution
The project proposes an AI-powered facial recognition system integrated into a mini smart car. A camera mounted on the dashboard captures the driver's face and verifies identity using facial recognition.

If the face is recognized → the system allows the engine (DC motor) to start
If not recognized → the engine remains locked

The system uses infrared (IR) technology to function in low-light or night conditions.

### Objectives
- Develop a facial recognition system using AI
- Enable secure engine access control
- Implement night detection using infrared lighting
- Integrate hardware and software into a working IoT prototype

---

## 🏗️ System Architecture & Design

![System Architecture Diagram](images/architecture_diagram.png)

### Design Decisions
> _Explain the key design decisions your group made._

---

## 🔧 Hardware Components

| Component | Description | Quantity | Purpose |
| :--- | :--- | :--- | :--- |
| Raspberry Pi 4 | Single-board computer | 1 | Main processor and controller |
| MicroSD Card 32GB | Flash storage media | 1 | Operating system and local data storage |
| NoIR Camera | Infrared-sensitive camera module | 1 | Capturing images/video, particularly in low light |
| Motor DC | Direct current motor | 1 | Providing physical movement/actuation |
| Motor Battery | Battery power source | 1 | Dedicated power supply for the motor |
| 2 Channel Relay Board | Electromechanical switch board | 1 | Controlling high-power/voltage devices safely |
| Power Supply | AC/DC power adapter | 1 | Powering the Raspberry Pi |
| Micro HDMI to HDMI cable | Video display cable | 1 | Connecting the Raspberry Pi to a monitor |
---

## 💻 Software & Technologies

| Tool / Platform | Purpose |
|---|---|
| [e.g. Arduino IDE] | [Firmware development] |
| [e.g. MQTT / Node-RED] | [Data communication / dashboard] |
| [e.g. GitHub] | [Version control & documentation] |
| [e.g. Fritzing] | [Circuit design] |

---

## 🔌 Circuit Diagram / Wiring

![Circuit Diagram](images/circuit_diagram.png)

| Component Pin | Microcontroller Pin | Notes |
|---|---|---|
| Relay IN | GPIO 17 (Pin 11) | Control signal from Pi |
| Relay VCC | 5V (Pin 2) | Power for relay module |
| Relay GND | GND (Pin 6) | Common ground |
| Motor + | Relay COM | Switched by relay |
| Motor - | Battery GND | Common ground |
| Battery + | Relay NO | Normally Open — motor off by default |
---

## 🏭 Build Process (with photos)

## Steps to Install Raspberry Pi OS

1. **Download Raspberry Pi Imager**
   * Go to the official [Raspberry Pi Imager page](https://www.raspberrypi.com/software/).
   * Download and install it on your laptop or PC.

2. **Insert Your microSD Card**
   * Put the microSD card into your computer using:
     * A built-in SD slot, or
     * A USB card reader.
   * *Note: A 16GB or larger card is recommended.*

3. **Open Raspberry Pi Imager**
   * Launch the application. You will see 3 main options:
     * Choose Device
     * Choose OS
     * Choose Storage

4. **Choose Device**
   * Select your Raspberry Pi model (for example, **Raspberry Pi 4**).

5. **Choose Operating System**
   * Select **Raspberry Pi OS (64-bit)**. This is the normal recommended operating system.

6. **Choose Storage**
   * Select your microSD card.
   > **Warning:** Be careful to choose the correct drive because it will be completely erased.

7. **Write the OS**
   * Click **Next** or **Write**.
   * The software will:
     * Download Raspberry Pi OS
     * Install it onto the SD card
     * Verify the installation
   * *This may take a few minutes.*

8. **Insert the SD Card into the Raspberry Pi**
   * After the installation finishes:
     * Remove the SD card safely from your computer.
     * Insert it into the Raspberry Pi.

9. **Connect Everything**
   * Connect the following to your Pi:
     * HDMI cable to a monitor/TV
     * Keyboard
     * Mouse
     * Power supply

10. **Power On the Raspberry Pi**
    * Plug in the power supply. The Raspberry Pi will boot into Raspberry Pi OS automatically.
    * You’ll then complete the first-time setup for:
      * Language
      * Wi-Fi
      * Password
      * Updates

***

**Success!** After that, Raspberry Pi OS is installed and ready to use.
---

## 🖥️ Code Documentation

### Main Firmware (e.g., `main.ino`)

```cpp
import cv2
import numpy as np
import os

PHOTOS_FOLDER = "AuthorizedFaces"

finder = cv2.CascadeClassifier(
    "/usr/share/opencv4/haarcascades/haarcascade_frontalface_default.xml"
)

trainer = cv2.face.LBPHFaceRecognizer_create()

faces = []
labels = []
label_map = {}
current_id = 0

# ---------------- LOOP PEOPLE ----------------
for person_name in os.listdir(PHOTOS_FOLDER):
    person_path = os.path.join(PHOTOS_FOLDER, person_name)

    if not os.path.isdir(person_path):
        continue

    label_map[current_id] = person_name

    # ---------------- LOOP IMAGES ----------------
    for img_name in os.listdir(person_path):
        img_path = os.path.join(person_path, img_name)

        img = cv2.imread(img_path)
        if img is None:
            continue

        gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        gray = cv2.resize(gray, (200, 200))

        detected = finder.detectMultiScale(gray, 1.3, 5)

        for (x, y, w, h) in detected:
            face = gray[y:y+h, x:x+w]
            face = cv2.resize(face, (200, 200))

            if face.shape[0] < 50 or face.shape[1] < 50:
                continue

            faces.append(face)
            labels.append(current_id)

    current_id += 1

# ---------------- SAFETY CHECK ----------------
if len(faces) == 0:
    print("ERROR: No faces found in training data")
    exit()

# ---------------- TRAIN ----------------
trainer.train(faces, np.array(labels))
trainer.write("trainer.yml")

print("Training Done!")
print("People trained:", label_map)
print("Total faces:", len(faces))
```

### Key Functions

| Function Name | Description |
|---|---|
| `setup()` | Initializes hardware peripherals and serial communication |
| `loop()` | Main execution loop |
| `[yourFunction()]` | [Describe it] |

---

## 🧪 Testing & Results

| Test # | Description | Expected Result | Actual Result | Pass/Fail |
|---|---|---|---|---|
| 1 | [e.g. Sensor reads temperature] | [e.g. ±2°C accuracy] | [e.g. ±1.5°C] | ✅ Pass |
| 2 | [e.g. Wi-Fi transmission] | [e.g. Every 10s] | | |

---

## ⚠️ Challenges & Solutions

| Challenge Encountered | Solution Applied |
|---|---|
| [Configuring the Rasberry Pi via a laptop] | [We used a kewyboard and a mouse.] |
| [Using a USB Port from a PC as the primary power source] | [Using a powerbank as our primary electric source] |

---

## 🎥 Project Demonstration

- 📹 **Demo Video:** [Insert link here]
- 📊 **Presentation Slides:** [Insert link here]


---

## 📚 References

1. [How to Setup the Raspberry Pi NoIR Camera
V2 8MP](https://youtu.be/bpzGN35oaJ4?si=rX-MXX4XT5EObkdQ) — _This video served as our primary visual guide, providing the step-by-step instructions we used to manually assemble, configure, and figure out the hardware prototype._


---

## 📊 Assessment Rubric

> ⚠️ **Students: Do NOT modify this section.**

### 📝 T1 — 50 Marks

| Criteria | Excellent (5) | Good (4) | Satisfactory (3) | Needs Improvement (2) | Incomplete (0-1) | Marks |
|---|---|---|---|---|---|---|
| Project Proposal & Problem Statement | Clear, detailed, well-researched | Clear with minor gaps | Stated but lacks depth | Vague | Not submitted | /5 |
| System Design & Architecture | Detailed diagram + design decisions | Good diagram with some docs | Basic diagram | Incomplete | Not submitted | /5 |
| Hardware Component Selection | All justified with images | Most documented | Listed not justified | Incomplete | Not attempted | /5 |
| Circuit Diagram / Wiring | Complete + pin mapping | Mostly complete | Partial | Incomplete | Not submitted | /5 |
| GitHub Repository Setup | Well-structured, clear commits | Good with minor issues | Basic structure | Minimal | Repo not set up | /5 |
| Markdown Documentation Quality | Excellent: headings, tables, images, code | Good with minor issues | Basic Markdown | Minimal | None | /5 |
| GitHub Commit History (T1) | Regular commits, all members | Regular, most members | Some commits | Few | None | /5 |
| Initial Code / Prototype | Working + well-commented | Working + some comments | Partial prototype | Started, not working | None | /5 |
| Group Collaboration Evidence | Issues, PRs, commits from all | Good evidence | Some evidence | Minimal | None | /5 |
| Build Progress Photos | Step-by-step + descriptions | Good photos | Photos, few descriptions | Few photos | None | /5 |
| | | | | | **T1 Total** | **/50** |

---

### 📝 T2 — 50 Marks *(Final Presentation: End of April 2026)*

| Criteria | Excellent (5) | Good (4) | Satisfactory (3) | Needs Improvement (2) | Incomplete (0-1) | Marks |
|---|---|---|---|---|---|---|
| Final Working Project | Fully functional | Mostly functional | Partially functional | Limited functionality | Not functional | /5 |
| Live Demonstration | Confident, all features | Good, minor issues | Core features shown | Partial/unclear | No demonstration | /5 |
| Testing & Results Documentation | All tests + analysis | Most documented | Some documented | Minimal | None | /5 |
| Code Quality & Comments | Clean, structured, fully commented | Good, most commented | Works, lacks comments | Messy/partial | None | /5 |
| Markdown Documentation Quality (T2) | Complete professional README | Good with minor gaps | Most sections filled | Incomplete | Minimal/none | /5 |
| GitHub Commit History (T2) | Consistent, all members | Good, most members | Some commits | Few | None | /5 |
| Challenges & Solutions | All documented with solutions | Most documented | Some documented | Vague | Not documented | /5 |
| System Architecture (Final) | Updated, matches build | Mostly matches | Partially updated | Outdated | Not present | /5 |
| Presentation Quality | Professional, all members | Good, all contribute | Acceptable | Weak/incomplete | None | /5 |
| References & Attribution | All properly listed | Most listed | Some listed | Minimal | None | /5 |
| | | | | | **T2 Total** | **/50** |

---

### 🏆 Final Mark Summary

| Term | Marks Available | Marks Achieved |
|---|---|---|
| T1 | 50 | /50 |
| T2 | 50 | /50 |
| **Total** | **100** | **/100** |

---

> 📌 **Assessed by:** `[Lecturer Name]`  
> 📅 **Final Submission Deadline:** End of April 2026  
> 🏫 **Institution:** Cape Peninsula University of Technology (CPUT)

---

*Documented using Markdown on GitHub — CPUT IT Diploma IoT Elective 2026* 🚀
