import java.util.*;

class Person {
    String name;
    int age;
    String gender;

    public Person(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
}

class Patient extends Person {
    int patientId;
    static int idCounter = 1000;

    public Patient(String name, int age, String gender) {
        super(name, age, gender);
        this.patientId = idCounter++;
    }

    public String toString() {
        return "Patient ID: " + patientId + ", Name: " + name + ", Age: " + age + ", Gender: " + gender;
    }
}

class Doctor extends Person {
    int doctorId;
    String specialty;
    static int idCounter = 500;

    public Doctor(String name, int age, String gender, String specialty) {
        super(name, age, gender);
        this.doctorId = idCounter++;
        this.specialty = specialty;
    }

    public String toString() {
        return "Doctor ID: " + doctorId + ", Name: " + name + ", Specialty: " + specialty;
    }
}

class Appointment {
    static int appIdCounter = 1;
    int appointmentId;
    Patient patient;
    Doctor doctor;
    String date;

    public Appointment(Patient patient, Doctor doctor, String date) {
        this.appointmentId = appIdCounter++;
        this.patient = patient;
        this.doctor = doctor;
        this.date = date;
    }

    public String toString() {
        return "Appointment ID: " + appointmentId + ", " + patient.name + " with Dr. " + doctor.name + " on " + date;
    }
}

public class HospitalManagementSystem {
    static Scanner scanner = new Scanner(System.in);
    static List<Patient> patients = new ArrayList<>();
    static List<Doctor> doctors = new ArrayList<>();
    static List<Appointment> appointments = new ArrayList<>();
    static HashMap<String, String> users = new HashMap<>();
    static String currentUser = null;

    public static void main(String[] args) {
        welcomeScreen();
    }

    static void welcomeScreen() {
        while (true) {
            System.out.println("\n=== Welcome to Hospital Management System ===");
            System.out.println("1. Register");
            System.out.println("2. Login");
            System.out.println("3. Exit");
            System.out.print("Choose an option: ");
            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    registerUser();
                    break;
                case "2":
                    loginUser();
                    break;
                case "3":
                    System.out.println("Exiting system...");
                    return;
                default:
                    System.out.println("Invalid option!");
            }
        }
    }

    static void registerUser() {
        System.out.print("Enter new username: ");
        String username = scanner.nextLine();

        if (users.containsKey(username)) {
            System.out.println("Username already exists. Try a different one.");
            return;
        }

        System.out.print("Enter password: ");
        String password = scanner.nextLine();
        users.put(username, password);
        System.out.println("Registration successful. You can now log in.");
    }

    static void loginUser() {
        System.out.print("Enter username: ");
        String username = scanner.nextLine();

        System.out.print("Enter password: ");
        String password = scanner.nextLine();

        if (users.containsKey(username) && users.get(username).equals(password)) {
            currentUser = username;
            System.out.println("Login successful. Welcome, " + currentUser + "!");
            runHospitalSystem();
        } else {
            System.out.println("Invalid username or password.");
        }
    }

    static void runHospitalSystem() {
        boolean running = true;

        while (running) {
            System.out.println("\n--- Hospital Management Menu ---");
            System.out.println("1. Add Patient");
            System.out.println("2. Add Doctor");
            System.out.println("3. Schedule Appointment");
            System.out.println("4. View Appointments");
            System.out.println("5. Logout");
            System.out.print("Select an option: ");

            int option = scanner.nextInt();
            scanner.nextLine(); // consume newline

            switch (option) {
                case 1:
                    addPatient();
                    break;
                case 2:
                    addDoctor();
                    break;
                case 3:
                    scheduleAppointment();
                    break;
                case 4:
                    viewAppointments();
                    break;
                case 5:
                    running = false;
                    currentUser = null;
                    System.out.println("Logged out successfully.");
                    break;
                default:
                    System.out.println("Invalid option!");
            }
        }
    }

    static void addPatient() {
        System.out.print("Enter name: ");
        String name = scanner.nextLine();
        System.out.print("Enter age: ");
        int age = scanner.nextInt();
        scanner.nextLine(); // consume newline
        System.out.print("Enter gender: ");
        String gender = scanner.nextLine();

        Patient patient = new Patient(name, age, gender);
        patients.add(patient);
        System.out.println("Added: " + patient);
    }

    static void addDoctor() {
        System.out.print("Enter name: ");
        String name = scanner.nextLine();
        System.out.print("Enter age: ");
        int age = scanner.nextInt();
        scanner.nextLine(); // consume newline
        System.out.print("Enter gender: ");
        String gender = scanner.nextLine();
        System.out.print("Enter specialty: ");
        String specialty = scanner.nextLine();

        Doctor doctor = new Doctor(name, age, gender, specialty);
        doctors.add(doctor);
        System.out.println("Added: " + doctor);
    }

    static void scheduleAppointment() {
        if (patients.isEmpty() || doctors.isEmpty()) {
            System.out.println("Add at least one patient and one doctor first.");
            return;
        }

        System.out.println("\nSelect a patient:");
        for (int i = 0; i < patients.size(); i++) {
            System.out.println((i + 1) + ". " + patients.get(i));
        }
        int patientIndex = scanner.nextInt() - 1;

        System.out.println("Select a doctor:");
        for (int i = 0; i < doctors.size(); i++) {
            System.out.println((i + 1) + ". " + doctors.get(i));
        }
        int doctorIndex = scanner.nextInt() - 1;

        scanner.nextLine(); // consume newline
        System.out.print("Enter appointment date (YYYY-MM-DD): ");
        String date = scanner.nextLine();

        Appointment appointment = new Appointment(patients.get(patientIndex), doctors.get(doctorIndex), date);
        appointments.add(appointment);
        System.out.println("Scheduled: " + appointment);
    }

    static void viewAppointments() {
        if (appointments.isEmpty()) {
            System.out.println("No appointments scheduled.");
            return;
        }

        for (Appointment appt : appointments) {
            System.out.println(appt);
        }
    }
}
