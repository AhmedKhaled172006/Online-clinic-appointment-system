# Online-clinic-appointment-system
#include <iostream>
#include <string>
using namespace std;

#define MAX_PATIENTS 100
#define MAX_DOCTORS 50
#define MAX_APPOINTMENTS 100

// ---------------- STRUCTS ----------------

struct Phone {
    string number;
};

struct Email {
    string email;
};

struct Address {
    string address;
};

struct Patient {
    int id;
    string name;
    string password;
    string disease;

    Phone phones[3];
    int phoneCount = 0;

    Email emails[3];
    int emailCount = 0;

    Address addresses[3];
    int addressCount = 0;
};

struct Doctor {
    int id;
    string name;
    string specialization;
    int slots;
};

struct Appointment {
    int patientID;
    int doctorID;
    string date;
};

// ---------------- ARRAYS ----------------

Patient patients[MAX_PATIENTS];
Doctor doctors[MAX_DOCTORS];
Appointment appointments[MAX_APPOINTMENTS];

int patientCount = 0;
int doctorCount = 0;
int appointmentCount = 0;

// ---------------- FUNCTIONS ----------------

// إنشاء حساب
void signUp() {
    Patient p;
    p.id = patientCount + 1;

    cout << "Name: ";
    cin.ignore();
    getline(cin, p.name);

    cout << "Password: ";
    getline(cin, p.password);

    cout << "Disease: ";
    getline(cin, p.disease);

    // phone
    cout << "Number of phones (max 3): ";
    cin >> p.phoneCount;
    if (p.phoneCount > 3) p.phoneCount = 3;
    cin.ignore();

    for (int i = 0; i < p.phoneCount; i++) {
        cout << "Phone number: ";
        getline(cin, p.phones[i].number);
    }

    patients[patientCount++] = p;

    cout << "Account created! Your ID: " << p.id << endl;
}

// تسجيل دخول
int login() {
    int id;
    string pass;

    cout << "ID: ";
    cin >> id;
    cin.ignore();

    cout << "Password: ";
    getline(cin, pass);

    for (int i = 0; i < patientCount; i++) {
        if (patients[i].id == id && patients[i].password == pass)
            return id;
    }

    return -1;
}

// إضافة دكتور
void addDoctor() {
    Doctor d;
    d.id = doctorCount + 1;

    cout << "Doctor name: ";
    cin.ignore();
    getline(cin, d.name);

    cout << "Specialization: ";
    getline(cin, d.specialization);

    cout << "Available slots: ";
    cin >> d.slots;

    doctors[doctorCount++] = d;

    cout << "Doctor added!\n";
}

// عرض الدكاترة
void viewDoctors() {
    for (int i = 0; i < doctorCount; i++) {
        cout << doctors[i].id << " - "
             << doctors[i].name << " - "
             << doctors[i].specialization
             << " (Slots: " << doctors[i].slots << ")\n";
    }
}

// حجز
void bookDoctor(int patientID) {
    int docID;

    viewDoctors();
    cout << "Enter Doctor ID: ";
    cin >> docID;

    for (int i = 0; i < doctorCount; i++) {
        if (doctors[i].id == docID && doctors[i].slots > 0) {

            Appointment a;
            a.patientID = patientID;
            a.doctorID = docID;

            cout << "Enter date: ";
            cin >> a.date;

            appointments[appointmentCount++] = a;
            doctors[i].slots--;

            cout << "Booked successfully!\n";
            return;
        }
    }

    cout << "Doctor not available!\n";
}

// ---------------- MENUS ----------------

void patientMenu(int id) {
    int choice;

    do {
        cout << "\n1.View Doctors\n2.Book Doctor\n3.Logout\n";
        cin >> choice;

        if (choice == 1) viewDoctors();
        else if (choice == 2) bookDoctor(id);

    } while (choice != 3);
}

void mainMenu() {
    int choice;

    do {
        cout << "\n1.Sign Up\n2.Login\n3.Add Doctor\n4.Exit\n";
        cin >> choice;

        if (choice == 1) signUp();

        else if (choice == 2) {
            int id = login();
            if (id != -1) {
                cout << "Login success\n";
                patientMenu(id);
            } else {
                cout << "Wrong data\n";
            }
        }

        else if (choice == 3) addDoctor();

    } while (choice != 4);
}

// ---------------- MAIN ----------------

int main() {
    mainMenu();
    return 0;
}
