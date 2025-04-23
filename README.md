# RemoteHospitalManagementSystem
A Java based Remote Patient Monitoring System (RPMS) project developed using **NetBeans IDE**. The system simulates an online hospital portal for managing patients, doctors, appointments, vitals, emergency alerts, and live communication (chat/video).

---

## 🚀 Features

### 🧑‍⚕️ User Management
- `User` (Base class)
- `Patient`: Handles personal data, vitals, feedback, and appointments.
- `Doctor`: Views patients, gives feedback and prescriptions.
- `Administrator`: Manages users and system logs.

### 💉 Health Monitoring
- `VitalSign`: Stores vitals like heart rate, blood pressure, SpO2, temperature, etc.
- `VitalsDatabase`: Stores and retrieves vitals for patients.

### 📅 Appointment System
- `Appointment`: Contains details of patient-doctor appointments.
- `AppointmentManager`: Schedules and views appointments.

### 💬 Doctor-Patient Interaction
- `Feedback`: Stores doctor feedback.
- `Prescription`: Contains medication details.
- `MedicalHistory`: Stores history of consultations and prescriptions.

### 🚨 Emergency Alert System
- `EmergencyAlert`: Raises an alert if vitals are outside safe thresholds.
- `NotificationService`: Sends alerts to doctors/admins.
- `PanicButton`: Manually triggers emergencies from the patient side.

### 📞 Chat & Video Consultation
- `ChatServer` and `ChatClient`: Enables text communication between doctor and patient.
- `VideoCall`: Simulates a basic video consultation interface (console-based placeholder or optional GUI mockup).

---
**Open in NetBeans**
- Launch NetBeans
- Click `File > Open Project`
- Select the cloned `RPMS` folder

**Run the Project**
- Right-click the project → `Run`
- `Main.java` will launch the console-based menu
- Navigate as Patient, Doctor, or Admin using console input

**Test Emergency Alerts**
- Enter abnormal vitals (e.g., heart rate 200 bpm)
- System will trigger an emergency alert via `EmergencyAlert` class

**Use Chat & Video**
- Run `ChatServer` in one terminal
- Run `ChatClient` in another to simulate chat
- `VideoCall` simulates a basic call logic via console

---
## 🛠️ Technologies Used

- Java (OOP principles)
- NetBeans IDE
- File I/O for data persistence
- Collections (ArrayList, HashMap)
- Console-based interface

---

## 📁 Project Structure

```plaintext
RPMS/
│
├── src/
│   ├── users/                  # User, Doctor, Patient, Administrator classes
│   ├── vitals/                 # VitalSign, VitalsDatabase
│   ├── appointments/           # Appointment, AppointmentManager
│   ├── interaction/            # Feedback, Prescription, MedicalHistory
│   ├── emergency/              # EmergencyAlert, PanicButton, NotificationService
│   ├── communication/          # ChatServer, ChatClient, VideoCall
│   └── Main.java               # Entry point

## 🚀 How to Run

1. Ensure you have **Java JDK 8+** installed.
2. Clone the repository:
   ```bash
   git clone https://github.com/aneeqazahid17/RemoteHospitalManagementSystem.git
