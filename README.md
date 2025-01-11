# Student-Management-System
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Student {
    string name;
    int rollNumber;
    float marks;
};

const int MAX_STUDENTS = 100;
Student students[MAX_STUDENTS];
int studentCount = 0;

void addStudent() {
    if (studentCount >= MAX_STUDENTS) {
        cout << "Cannot add more students. Storage is full." << endl;
        return;
    }
    cout << "Enter student name: ";
    cin.ignore(); // Clear the input buffer
    getline(cin, students[studentCount].name);
    cout << "Enter roll number: ";
    cin >> students[studentCount].rollNumber;
    cout << "Enter marks: ";
    cin >> students[studentCount].marks;
    studentCount++;
    cout << "Student added successfully!" << endl;
}

void updateStudent() {
    int rollNumber;
    cout << "Enter roll number of the student to update: ";
    cin >> rollNumber;
    bool found = false;
    for (int i = 0; i < studentCount; i++) {
        if (students[i].rollNumber == rollNumber) {
            found = true;
            cout << "Enter new name: ";
            cin.ignore();
            getline(cin, students[i].name);
            cout << "Enter new marks: ";
            cin >> students[i].marks;
            cout << "Student information updated successfully!" << endl;
            break;
        }
    }
    if (!found) {
        cout << "Student with roll number " << rollNumber << " not found." << endl;
    }
}

void deleteStudent() {
    int rollNumber;
    cout << "Enter roll number of the student to delete: ";
    cin >> rollNumber;
    bool found = false;
    for (int i = 0; i < studentCount; i++) {
        if (students[i].rollNumber == rollNumber) {
            found = true;
            for (int j = i; j < studentCount - 1; j++) {
                students[j] = students[j + 1];
            }
            studentCount--;
            cout << "Student deleted successfully!" << endl;
            break;
        }
    }
    if (!found) {
        cout << "Student with roll number " << rollNumber << " not found." << endl;
    }
}

void displayStudents() {
    if (studentCount == 0) {
        cout << "No students available." << endl;
        return;
    }
    cout << "Student List:" << endl;
    for (int i = 0; i < studentCount; i++) {
        cout << "Name: " << students[i].name
             << ", Roll Number: " << students[i].rollNumber
             << ", Marks: " << students[i].marks << endl;
    }
}

void saveToFile() {
    ofstream file("students.txt");
    if (!file) {
        cout << "Error opening file for writing!" << endl;
        return;
    }
    for (int i = 0; i < studentCount; i++) {
        file << students[i].name << "," << students[i].rollNumber << "," << students[i].marks << endl;
    }
    file.close();
    cout << "Student data saved to file successfully!" << endl;
}

void loadFromFile() {
    ifstream file("students.txt");
    if (!file) {
        cout << "Error opening file for reading!" << endl;
        return;
    }
    studentCount = 0;
    while (file && studentCount < MAX_STUDENTS) {
        getline(file, students[studentCount].name, ',');
        file >> students[studentCount].rollNumber;
        file.ignore(); // Ignore the comma
        file >> students[studentCount].marks;
        file.ignore(); // Ignore the newline
        if (!file.fail()) {
            studentCount++;
        }
    }
    file.close();
    cout << "Student data loaded from file successfully!" << endl;
}

int main() {
    loadFromFile();
    int choice;
    do {
        cout << "\n*** Student Management System ***" << endl;
        cout << "1. Add Student" << endl;
        cout << "2. Update Student" << endl;
        cout << "3. Delete Student" << endl;
        cout << "4. Display Students" << endl;
        cout << "5. Save to File" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                updateStudent();
                break;
            case 3:
                deleteStudent();
                break;
            case 4:
                displayStudents();
                break;
            case 5:
                saveToFile();
                break;
            case 6:
                cout << "Exiting the system. Goodbye!" << endl;
                break;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 6);

    return 0;
}
