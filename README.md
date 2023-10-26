#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <ctime>
#include <conio.h> // Note: conio.h is not standard C++ and may not be available on all systems.
using namespace std;

// Global variables and constants
const int size = 10;
string Month[12] = {"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"};
string Sports[7] = {"Futsal", "Badminton", "Netball", "Bowling", "Swimming", "Ping Pong", "Tennis"};

// EquipmentList class
class EquipmentList {
    int front, back, count;
    string items[size];

public:
    EquipmentList() {
        front = 0;
        back = size - 1;
        count = 0;
    }

    bool isEmpty() {
        return (count == 0);
    }

    void insertEq(string newItem) {
        if (newItem == "")
            return;
        back = (back + 1) % size;
        items[back] = newItem;
        ++count;
    }

    string getFront() {
        return items[front];
    }

    void removeEq() {
        front = (front + 1) % size;
        --count;
    }
};

EquipmentList Ball;
EquipmentList rentedBall;

// Node class
class Node {
public:
    string name, sportType, month, ic, borrowedItem;
    int age, date, chin, chout, mon;
    Node* link; // pointer to next node
};

// List class
class List {
private:
    Node* head;
    Node* borrowHead;

public:
    List(void) {
        head = NULL;
        borrowHead = NULL;
    }
    ~List(void) {
        head = NULL;
        borrowHead = NULL;
    }
    Node* InsertNode(string, string, string, int, string, int, int, int);
    Node* InsertNode(string, string, string, int, string, int, int, int, int);
    Node* InsertBorrowNode(string, string, int, int, string);
    void deleteNode(int);
    void Find();
    void FindName(string);
    void FindIC(string);
    void FindMonth(string);
    void Sort();
    void SortName(int);
    void SortIC(int);
    void SortAge(int);
    void Delete();
    void Insert();
    void Rent();
    void menu();
    int displayBorrower();
    int DisplayList();
};

Node* List::InsertNode(string n, string m, string s, int a, string i, int d, int ci, int co) {
    int currIndex = 0;
    Node* currNode = head;
    Node* prevNode = NULL;
    while (currNode && n > currNode->name && s > currNode->sportType && m > currNode->month && a > currNode->age &&
           i > currNode->ic && d > currNode->date && ci > currNode->chin && co > currNode->chout) {
        prevNode = currNode;
        currNode = currNode->link;
        currIndex++;
    }

    Node* newNode = new Node;
    newNode->name = n;
    newNode->sportType = s;
    newNode->month = m;
    newNode->age = a;
    newNode->ic = i;
    newNode->date = d;
    newNode->chin = ci;
    newNode->chout = co;

    if (currIndex == 0) {
        newNode->link = head;
        head = newNode;
    } else {
        newNode->link = prevNode->link;
        prevNode->link = newNode;
    }

    return newNode;
}

// Continue adding the corrected code for other functions
void List::menu() {
    int choice;
    do {
        cout << "\nStudent Sports Equipment Borrowing System\n";
        cout << "---------------------------------------\n";
        cout << "1. Insert student info\n";
        cout << "2. Delete student info\n";
        cout << "3. Find student info\n";
        cout << "4. Sort student info\n";
        cout << "5. Display student info\n";
        cout << "6. Rent sports equipment\n";
        cout << "7. Display renters\n";
        cout << "8. Exit\n";
        cout << "Select your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                Insert();
                break;
            case 2:
                Delete();
                break;
            case 3:
                Find();
                break;
            case 4:
                Sort();
                break;
            case 5:
                DisplayList();
                break;
            case 6:
                Rent();
                break;
            case 7:
                displayBorrower();
                break;
            case 8:
                cout << "Program terminated.\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 8);
}

void List::Insert() {
    string name, rollNo;
    cout << "Enter student name: ";
    cin >> name;
    cout << "Enter student roll number: ";
    cin >> rollNo;

    // Create a new student node
    Node* newNode = new Node(name, rollNo);

    if (head == NULL) {
        head = newNode;
    } else {
        // Find the appropriate position to insert in alphabetical order
        Node* current = head;
        Node* previous = NULL;
        while (current && name > current->name) {
            previous = current;
            current = current->next;
        }
        if (previous == NULL) {
            newNode->next = head;
            head = newNode;
        } else {
            newNode->next = current;
            previous->next = newNode;
        }
    }

    cout << "Student information inserted successfully." << endl;

}

void List::Rent() {
    string item;
    cout << "Enter student name: ";
    string n;
    cin >> n;
    if (borrowHead == NULL) {
        cout << "Error! No Borrowers Available." << endl;
        return;
    }
    Node* currNode = borrowHead;
    Node* prevNode = NULL;
    while (currNode && n > currNode->name) {
        prevNode = currNode;
        currNode = currNode->link;
    }

    if (currNode && n == currNode->name) {
        cout << "Enter equipment: ";
        cin >> item;
        // Check if the equipment is available in the list of equipment.
        if (Ball.isEmpty()) {
            cout << "No equipment available for rent." << endl;
            return;
        }

        bool equipmentFound = false;

        // Search for the equipment in the list of equipment.
        // If found, remove it from the list and update the student's borrowed item.
        // Otherwise, inform the user that the equipment is not available.
        for (int i = 0; i < size; i++) {
            if (Ball.getFront() == item) {
                equipmentFound = true;
                currNode->borrowedItem = item;
                rentedBall.insertEq(item);
                Ball.removeEq();
                cout << item << " has been rented by " << n << "." << endl;
                break;
            }
            Ball.insertEq(Ball.getFront( ));
            Ball.removeEq();
        }

        if (!equipmentFound) {
            cout << "Equipment not available." << endl;
        }
    } else {
        cout << "Student not found." << endl;
    }
}

int List::displayBorrower() {
 if (borrowHead == NULL) {
        cout << "No Borrowers Available." << endl;
    } else {
        cout << "List of Borrowers:\n";
        Node* current = borrowHead;
        while (current) {
            cout << "Name: " << current->name << ", Borrowed Item: " << current->borrowedItem << endl;
            current = current->link;
        }
    }
}

int List::DisplayList() {
      if (head == NULL) {
        cout << "Student list is empty." << endl;
    } else {
        cout << "List of Students:\n";
        Node* current = head;
        while (current) {
            cout << "Name: " << current->name << ", Roll Number: " << current->rollNo;
            if (!current->borrowedItem.empty()) {
                cout << ", Borrowed Item: " << current->borrowedItem;
            }
            cout << endl;
            current = current->next;
        }
    }

}
