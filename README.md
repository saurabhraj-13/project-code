# project-code
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Scanner;

// Basic Models for Patient and Appointment
class Patient {
    private int id;
    private String name;
    private int age;
    private String contact;

    public Patient(int id, String name, int age, String contact) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.contact = contact;
    }

    public int getId() { return id; }
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getContact() { return contact; }

    @Override
    public String toString() {
        return "Patient ID: " + id + ", Name: " + name + ", Age: " + age + ", Contact: " + contact;
    }
}

class Appointment {
    private int id;
    private int patientId;
    private Date date;
    private String doctorName;

    public Appointment(int id, int patientId, Date date, String doctorName) {
        this.id = id;
        this.patientId = patientId;
        this.date = date;
        this.doctorName = doctorName;
    }

    public int getId() { return id; }
    public int getPatientId() { return patientId; }
    public Date getDate() { return date; }
    public String getDoctorName() { return doctorName; }

    @Override
    public String toString() {
        return "Appointment ID: " + id + ", Patient ID: " + patientId + ", Date: " + date + ", Doctor: " + doctorName;
    }
}

// Service Layer for managing patients and appointments
class HealthcareService {
    private List<Patient> patients = new ArrayList<>();
    private List<Appointment> appointments = new ArrayList<>();
    private int patientCounter = 1;
    private int appointmentCounter = 1;

    // Add new patient
    public Patient addPatient(String name, int age, String contact) {
        Patient patient = new Patient(patientCounter++, name, age, contact);
        patients.add(patient);
        return patient;
    }

    // Schedule new appointment
    public Appointment scheduleAppointment(int patientId, Date date, String doctorName) {
        Appointment appointment = new Appointment(appointmentCounter++, patientId, date, doctorName);
        appointments.add(appointment);
        return appointment;
    }

    // Retrieve all patients
    public List<Patient> getAllPatients() {
        return patients;
    }

    // Retrieve all appointments
    public List<Appointment> getAllAppointments() {
        return appointments;
    }
}

// Main Application
public class HealthcareManagementSystem {
    public static void main(String[] args) {
        HealthcareService service = new HealthcareService();
        Scanner scanner = new Scanner(System.in);

        System.out.println("Healthcare Management System");
        
        while (true) {
            System.out.println("\n1. Add Patient");
            System.out.println("2. Schedule Appointment");
            System.out.println("3. View All Patients");
            System.out.println("4. View All Appointments");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter patient name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter patient age: ");
                    int age = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter patient contact: ");
                    String contact = scanner.nextLine();
                    Patient patient = service.addPatient(name, age, contact);
                    System.out.println("Added: " + patient);
                    break;
                
                case 2:
                    System.out.print("Enter patient ID for appointment: ");
                    int patientId = scanner.nextInt();
                    scanner.nextLine(); // Consume newline
                    System.out.print("Enter doctor name: ");
                    String doctorName = scanner.nextLine();
                    System.out.print("Enter appointment date (in milliseconds): ");
                    long dateMillis = scanner.nextLong();
                    Date date = new Date(dateMillis);
                    Appointment appointment = service.scheduleAppointment(patientId, date, doctorName);
                    System.out.println("Scheduled: " + appointment);
                    break;
                
                case 3:
                    System.out.println("Patients:");
                    for (Patient p : service.getAllPatients()) {
                        System.out.println(p);
                    }
                    break;
                
                case 4:
                    System.out.println("Appointments:");
                    for (Appointment a : service.getAllAppointments()) {
                        System.out.println(a);
                    }
                    break;
                
                case 5:
                    System.out.println("Exiting...");
                    scanner.close();
                    System.exit(0);
                
                default:
                    System.out.println("Invalid option. Try again.");
            }
        }
    }
}
