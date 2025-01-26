# Management-Systems
College Management System
#include <iostream>
#include <string>
#include <vector>
#include <fstream>

using namespace std;

// Basic class for a student
class Student {
private:
    int studentID;
    string name;
    string course;
    int attendance;
    float grade;

public:
    // Constructor
    Student(int id, string n, string c, int att = 0, float g = 0.0) :
        studentID(id), name(n), course(c), attendance(att), grade(g) {}

    // Function to display student information
    void displayStudent() const {
        cout << "ID: " << studentID << ", Name: " << name << ", Course: " << course
             << ", Attendance: " << attendance << ", Grade: " << grade << endl;
    }

    // Function to update student attendance
    void updateAttendance(int att) {
        attendance = att;
    }

    // Function to update student grade
    void updateGrade(float g) {
        grade = g;
    }

    int getID() const {
        return studentID;
    }

    string getName() const {
        return name;
    }

    float getGrade() const {
        return grade;
    }

    // File save function
    void saveToFile(ofstream &file) const {
        file << studentID << " " << name << " " << course << " " << attendance << " " << grade << endl;
    }

    // File read function
    static Student loadFromFile(ifstream &file) {
        int id, att;
        float grade;
        string name, course;
        file >> id >> name >> course >> att >> grade;
        return Student(id, name, course, att, grade);
    }
};

// Vector to hold all students
vector<Student> students;

// Function to add student
void addStudent() {
    int id, att;
    float grade;
    string name, course;
    cout << "Enter Student ID: ";
    cin >> id;
    cin.ignore();  // Ignore newline characters from previous input

    cout << "Enter Student Name: ";
    getline(cin, name);  // Use getline to handle spaces in name

    cout << "Enter Course Name: ";
    getline(cin, course);

    cout << "Enter Attendance: ";
    cin >> att;

    cout << "Enter Grade: ";
    cin >> grade;

    // Add the student to the vector
    students.push_back(Student(id, name, course, att, grade));
    cout << "Student added successfully!" << endl;
}

// Function to display all students
void displayAllStudents() {
    if (students.empty()) {
        cout << "No students available to display." << endl;
        return;
    }

    for (const auto &student : students) {
        student.displayStudent();
    }
}

// Function to save data to file
void saveData() {
    ofstream file("students.txt");
    for (const auto &student : students) {
        student.saveToFile(file);
    }
    file.close();
    cout << "Data saved successfully!" << endl;
}

// Function to load data from file
void loadData() {
    ifstream file("students.txt");
    if (!file) {
        cout << "No data found. Starting with an empty list." << endl;
        return;
    }

    while (file) {
        if (file.peek() == EOF) break;
        students.push_back(Student::loadFromFile(file));
    }
    file.close();
    cout << "Data loaded successfully!" << endl;
}

// Main function
int main() {
    int choice;

    loadData();  // Load any existing data

    while (true) {
        cout << "\n1. Add Student\n2. Display All Students\n3. Save Data\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                displayAllStudents();
                break;
            case 3:
                saveData();
                break;
            case 4:
                cout << "Exiting program." << endl;
                return 0;
            default:
                cout << "Invalid choice, please try again." << endl;
        }
    }

    return 0;
}
