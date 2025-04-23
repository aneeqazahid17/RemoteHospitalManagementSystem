package rpms;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

abstract class User {
    private String id;
    private String name;

    public User(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() 
    { return id; }
    public String getName() 
    { return name; }

    public abstract void displayDetails();
}

class Patient extends User {
    private List<VitalSign> vitals = new ArrayList<>();
    private MedicalHistory medicalHistory = new MedicalHistory();

    public Patient(String id, String name) {
        super(id, name);
    }

    @Override
    public void displayDetails() {
        System.out.println("Patient ID: " + getId() + ", Name: " + getName());
    }

    public void uploadVitals(VitalSign vital) {
        vitals.add(vital);
        System.out.println("vitals uploaded for " + getName());
    }

    public void addMedicalHistory(Feedback feedback) {
        medicalHistory.addFeedback(feedback);
    }

    public void viewMedicalHistory() {
        medicalHistory.displayHistory();
    }

    public List<VitalSign> getVitals() {
        return vitals;
    }
}

class Doctor extends User {
    public Doctor(String id, String name) {
        super(id, name);
    }

    @Override
    public void displayDetails() {
        System.out.println("Doctor ID: " + getId() + ", Name: " + getName());
    }

    public void provideFeedback(Patient patient, String comments, Prescription prescription) {
        Feedback feedback = new Feedback(comments, prescription);
        patient.addMedicalHistory(feedback);
        System.out.println("feedback given to " + patient.getName());
    }
}

class Administrator extends User {
    private List<Patient> patients = new ArrayList<>();

    public Administrator(String id, String name) {
        super(id, name);
    }

    @Override
    public void displayDetails() {
        System.out.println("Administrator ID: " + getId() + ", Name: " + getName());
    }

    public void addPatient(Patient patient) {
        patients.add(patient);
        System.out.println("Patient " + patient.getName() + " is added successfully.");
    }

    public List<Patient> getPatients() {
        return patients;
    }

    public Patient getPatientById(String id) {
        for (Patient patient : patients) {
            if (patient.getId().equals(id)) {
                return patient;
            }
        }
        return null;
    }
}

// class VitalSign
class VitalSign {
    private int heartRate;
    private int oxygenLevel;
    private int systolicPressure;
    private int diastolicPressure;
    private double temperature;

    // constructor
    public VitalSign(int heartRate, int oxygenLevel, int systolicPressure, int diastolicPressure, double temperature) {
        this.heartRate = heartRate;
        this.oxygenLevel = oxygenLevel;
        this.systolicPressure = systolicPressure;
        this.diastolicPressure = diastolicPressure;
        this.temperature = temperature;
    }

    public void displayVitalSigns() {
        System.out.println("heart rate: " + heartRate + " bpm, Oxygen Level: " + oxygenLevel + "%" + 
                           ", blood pressure: " + systolicPressure + "/" + diastolicPressure + " mmHg, Temperature: " + temperature + "°C");
    }

    public int getHeartRate() {
        return heartRate;
    }

    public int getOxygenLevel() {
        return oxygenLevel;
    }

    public double getTemperature() {
        return temperature;
    }
    public int getSystolicPressure() {
        return systolicPressure;
    }

    public int getDiastolicPressure() {
        return diastolicPressure;
    }
}

class Feedback {
    private String comments;
    private Prescription prescription;

    public Feedback(String comments, Prescription prescription) {
        this.comments = comments;
        this.prescription = prescription;
    }

    public String getComments() {
        return comments;
    }

    public Prescription getPrescription() {
        return prescription;
    }

    public void displayFeedback() {
        System.out.println("comments: " + comments);
        if (prescription != null) {
            prescription.displayPrescription();
        } else {
            System.out.println("no prescription");
        }
    }
}

class Prescription {
    private String medication;
    private String dosage;

    public Prescription(String medication, String dosage) {
        this.medication = medication;
        this.dosage = dosage;
    }

    public void displayPrescription() {
        System.out.println("Medication: " + medication + ", Dosage: " + dosage);
    }
}

class Appointment {
    private String date;
    private String status;
    private Patient patient;
    private Doctor doctor;

    public Appointment(String date, Patient patient, Doctor doctor) {
        this.date = date;
        this.patient = patient;
        this.doctor = doctor;
        this.status = "pending";
    }
    public String getDate() {
        return date;
    }
    public Doctor getDoctor() {
        return doctor;
    }

    public void displayAppointment() {
        System.out.println("Date: " + date + ", Patient: " + patient.getName() + ", Doctor: " + doctor.getName() + ", status: " + status);
    }
}

class AppointmentManager {
    private List<Appointment> appointments = new ArrayList<>();

    public void scheduleAppointment(Appointment appointment) {
        appointments.add(appointment);
        System.out.println("appointment is scheduled.");
    }

    public void viewAppointments() {
        for (Appointment app : appointments) {
            app.displayAppointment();
        }
    }
    // method which gets appointment by its date
    public Appointment getAppointmentByDate(String date) {
        for (Appointment app : appointments) {
            if (app.getDate().equals(date)) {
                return app;
            }
        }
        // return null if no appointment match
        return null;
    }
}

//class for emergency alert system
class EmergencyAlert {
    public static void checkVitals(Patient patient, VitalSign vital) {
        boolean alert = false;

        if (vital.getHeartRate() < 50 || vital.getHeartRate() > 120) alert = true;
        if (vital.getOxygenLevel() < 90) alert = true;
        if (vital.getTemperature() < 35 || vital.getTemperature() > 39) alert = true;

        if (alert) {
            NotificationService.sendAlert(patient);
        }
    }

    public static void triggerManualAlert(Patient patient) {
        System.out.println("manual alert triggered by " + patient.getName());
        NotificationService.sendAlert(patient);
    }
}

class NotificationService {
    public static void sendAlert(Patient patient) {
        System.out.println("emergency alert!!! sending SMS/Email for patient: " + patient.getName());
        // doctor gets email or SMS notification
        System.out.println("Notified assigned doctor: Dr. Nazia");
    }
}

class PanicButton {
    public static void press(Patient patient) {
        EmergencyAlert.triggerManualAlert(patient);
    }
}

//class for chat and video consultation
class ChatServer {
    private static List<String> chatLog = new ArrayList<>();

    public static void receiveMessage(String sender, String message) {
        String msg = sender + ": " + message;
        chatLog.add(msg);
    }

    public static void displayChat() {
        System.out.println(" chat conversation:");
        for (String msg : chatLog) {
            System.out.println(msg);
        }
    }
}

class ChatClient {
    public static void sendMessage(String sender, String message) {
        ChatServer.receiveMessage(sender, message);
    }
}

class VideoCall {
    public static void startCall(Patient patient, Doctor doctor) {
        System.out.println(" video call will start between " + doctor.getName() + " and " + patient.getName());
        System.out.println("join via link: https://meet.example.com/" + patient.getId() + doctor.getId());
    }
}

interface Notifiable {
    void sendNotification(String message);
}
class EmailNotification implements Notifiable {
    @Override
    public void sendNotification(String message) {
        System.out.println("email has been sent: " + message);
    }
}

class SMSNotification implements Notifiable {
    @Override
    public void sendNotification(String message) {
        System.out.println("SMS has been sent: " + message);
    }
}
class ReminderService {
    private Notifiable notificationMethod;

    public ReminderService(Notifiable notificationMethod) {
        this.notificationMethod = notificationMethod;
    }

    public void sendAppointmentReminder(Patient patient, Appointment appointment) {
        String message = "Reminder: you have appointment scheduled with Dr. " + appointment.getDoctor().getName() + " on " + appointment.getDate() + ".";
        notificationMethod.sendNotification(message);
    }

    public void sendMedicationReminder(Patient patient, String medication, String dosage) {
        String message = "Reminder: take your medication (" + medication + ") with dosage: " + dosage + ".";
        notificationMethod.sendNotification(message);
    }
}

public class RPMS {
    private static Scanner scanner = new Scanner(System.in);
    private static Administrator admin = new Administrator("A1", "Admin");
    private static Doctor doctor = new Doctor("01", "Dr. Nazia");
    private static AppointmentManager appointmentManager = new AppointmentManager();
     private static ReminderService emailReminderService = new ReminderService(new EmailNotification());
    private static ReminderService smsReminderService = new ReminderService(new SMSNotification());

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n***** Remote Hospital Management System Menu *****");
            System.out.println("1. Add Patient");
            System.out.println("2. Upload Vitals");
            System.out.println("3. View Patient Vitals");
            System.out.println("4. Schedule Appointment");
            System.out.println("5. View Appointments");
            System.out.println("6. Provide Feedback");
            System.out.println("7. Trigger Panic Button");
            System.out.println("8. Chat with Doctor");
            System.out.println("9. Start Video Consultation");
            System.out.println("10. Send Appointment Reminder via Email");
            System.out.println("11. Send Appointment Reminder via SMS");
            System.out.println("12. Exit");
            System.out.print("Select an option: ");
            int option = scanner.nextInt();
            scanner.nextLine();

            switch (option) {
                case 1 -> addPatient();
                case 2 -> uploadVitals();
                case 3 -> viewPatientVitals();
                case 4 -> scheduleAppointment();
                case 5 -> appointmentManager.viewAppointments();
                case 6 -> provideFeedback();
                case 7 -> triggerPanicButton();
                case 8 -> chatWithDoctor();
                case 9 -> startVideoCall();
                case 10 -> sendAppointmentReminderEmail();
                case 11 -> sendAppointmentReminderSMS();
                case 12 -> System.exit(0);
                default -> System.out.println("Invalid option");
            }
        }
    }

    private static void addPatient() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        System.out.print("Enter patient name: ");
        String name = scanner.nextLine();
        Patient newPatient = new Patient(id, name);
        admin.addPatient(newPatient);
    }

    private static void uploadVitals() {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter heart rate: ");
            int heartRate = scanner.nextInt();
            System.out.print("Enter oxygen level: ");
            int oxygenLevel = scanner.nextInt();
            System.out.print("Enter systolic pressure: ");
            int systolic = scanner.nextInt();
            System.out.print("Enter diastolic pressure: ");
            int diastolic = scanner.nextInt();
            System.out.print("Enter temperature: ");
            // handle temperature input exception
        double temperature = 0.0;
        while (true) {
            System.out.print("Enter temperature: ");
            String tempInput = scanner.nextLine();
            try {
                temperature = Double.parseDouble(tempInput); // attempt to parse as double
                break; // valid input
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid temperature (e.g., 36.5)");
            }
        }

        scanner.nextLine(); // newline after reading double
            VitalSign vital = new VitalSign(heartRate, oxygenLevel, systolic, diastolic, temperature);
            patient.uploadVitals(vital);
            EmergencyAlert.checkVitals(patient, vital);  // Check vitals
        } else {
            System.out.println("Patient not found");
        }
    }

    private static void viewPatientVitals() {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            List<VitalSign> vitals = patient.getVitals();
            if (vitals.isEmpty()) {
                System.out.println("no vitals found.");
            } else {
                for (VitalSign vital : vitals) {
                    vital.displayVitalSigns();
                }
            }
        } else {
            System.out.println("patient not found");
        }
    }

    private static void scheduleAppointment() {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter appointment date: ");
            String date = scanner.nextLine();
            appointmentManager.scheduleAppointment(new Appointment(date, patient, doctor));
        } else {
            System.out.println("patient not found");
        }
    }

    private static void provideFeedback() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter feedback: ");
            String feedback = scanner.nextLine();
            System.out.print("Enter medication: ");
            String medication = scanner.nextLine();
            System.out.print("Enter dosage: ");
            String dosage = scanner.nextLine();
            Prescription prescription = new Prescription(medication, dosage);
            doctor.provideFeedback(patient, feedback, prescription);
        } else {
            System.out.println("patient not found");
        }
    }

    private static void triggerPanicButton() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            PanicButton.press(patient);
        } else {
            System.out.println("patient not found");
        }
    }

    private static void chatWithDoctor() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter message to doctor: ");
            String msg = scanner.nextLine();
            ChatClient.sendMessage(patient.getName(), msg);
            ChatClient.sendMessage(doctor.getName(), "Received your message, will respond shortly.");
            ChatServer.displayChat();
        } else {
            System.out.println("patient not found");
        }
    }

    private static void startVideoCall() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            VideoCall.startCall(patient, doctor);
        } else {
            System.out.println("Patient not found");
        }
    }
    private static void sendAppointmentReminderEmail() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter appointment date: ");
            String date = scanner.nextLine();
            Appointment appointment = appointmentManager.getAppointmentByDate(date);
            if (appointment != null) {
                emailReminderService.sendAppointmentReminder(patient, appointment);
            } else {
                System.out.println("appointment not found.");
            }
        } else {
            System.out.println("patient not found.");
        }
    }

    private static void sendAppointmentReminderSMS() {
        System.out.print("Enter patient ID: ");
        String id = scanner.nextLine();
        Patient patient = admin.getPatientById(id);
        if (patient != null) {
            System.out.print("Enter appointment date: ");
            String date = scanner.nextLine();
            Appointment appointment = appointmentManager.getAppointmentByDate(date);
            if (appointment != null) {
                smsReminderService.sendAppointmentReminder(patient, appointment);
            } else {
                System.out.println("appointment not found.");
            }
        } else {
            System.out.println("patient not found.");
        }
    }
}
