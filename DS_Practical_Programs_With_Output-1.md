# Data Structure Practical Programs (Code + Output)

This file combines the programs from the uploaded Data Structure practical PDFs and Word document. Each program is provided in a separate code block so it can be copied independently. Sample outputs are included below each program.

> **Note:** The uploaded files use different practical-number labels in their filenames. To avoid assigning the wrong number, the sections are identified by their program topics. The telephone-book hashing program is taken from `DS PRACTICAL NO4.docx`. The earlier screenshot of Practical No. 3 showed only part of that program, so this file uses the complete version available in the Word document.

---
## 1. Linear Search and Binary Search

**Program statement:** Write a menu based program to search whether a particular student attended a training program or not. Use Linear Search for roll numbers stored in random order and Binary Search for roll numbers stored in sorted order.

**Code:**

```cpp
#include <iostream>
using namespace std;
bool linearSearch(int arr[], int n, int key) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == key)
            return true;
    }
    return false;
}

int main() {
    int n;
    cout << "Enter number of students who attended training: ";
    cin >> n;
    int rollNumbers[n];
    cout << "Enter roll numbers of students:\n";
    for (int i = 0; i < n; i++) {
        cin >> rollNumbers[i];
    }
    int key;
    cout << "Enter roll number to search: ";
    cin >> key;
    if (linearSearch(rollNumbers, n, key))
        cout << "Student with roll number " << key << " attended the training program.\n";
    else
        cout << "Student with roll number " << key << " did NOT attend the training program.\n";
    return 0;
}
```

**Sample output:**

```text
Enter number of students who attended training: 4
Enter roll numbers of students:
4 3 2 1
Enter roll number to search: 3
Student with roll number 3 attended the training program.
```


### 1(b). Binary Search

**Program statement:** Store roll numbers of students who attended training in sorted order and search whether a particular student attended the training program using Binary Search.

**Code:**

```cpp
#include <iostream>
using namespace std;
bool binarySearch(int arr[], int n, int key) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == key)
            return true;
        else if (arr[mid] < key)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return false;
}

int main() {
    int n;
    cout << "Enter number of students who attended training: ";
    cin >> n;
    int rollNumbers[n];
    cout << "Enter roll numbers in sorted order:\n";
    for (int i = 0; i < n; i++) {
        cin >> rollNumbers[i];
    }
    int key;
    cout << "Enter roll number to search: ";
    cin >> key;
    if (binarySearch(rollNumbers, n, key))
        cout << "Student with roll number " << key << " attended the training program.\n";
    else
        cout << "Student with roll number " << key << " did NOT attend the training program.\n";
    return 0;
}
```

**Sample output:**

```text
Enter number of students who attended training: 4
Enter roll numbers in sorted order:
1 2 3 4
Enter roll number to search: 3
Student with roll number 3 attended the training program.
```

---
## 2. Balanced Parentheses Using Stack

**Program statement:** Write a program using stack to check whether a given expression is well parenthesized or not. Check (), {} and [].

**Code:**

```c
#include <stdio.h>
#include <string.h>
#include <stdbool.h>

#define MAX 100

char stack[MAX];
int top = -1;

void push(char ch) {
    if (top < MAX - 1)
        stack[++top] = ch;
}

char pop() {
    if (top >= 0)
        return stack[top--];
    return '\0';
}

bool isMatching(char open, char close) {
    return (open == '(' && close == ')') ||
           (open == '{' && close == '}') ||
           (open == '[' && close == ']');
}

bool isBalanced(char exp[]) {
    for (int i = 0; exp[i] != '\0'; i++) {
        char ch = exp[i];
        if (ch == '(' || ch == '{' || ch == '[')
            push(ch);
        else if (ch == ')' || ch == '}' || ch == ']') {
            if (top == -1)
                return false;
            char topChar = pop();
            if (!isMatching(topChar, ch))
                return false;
        }
    }
    return top == -1;
}

int main() {
    char expression[MAX];
    printf("Enter an expression: ");
    fgets(expression, MAX, stdin);
    expression[strcspn(expression, "\n")] = '\0';
    if (isBalanced(expression))
        printf("Expression is well parenthesized.\n");
    else
        printf("Expression is NOT well parenthesized.\n");
    return 0;
}
```

**Sample output:**

```text
Enter an expression: ({a+b}*[c/d])
Expression is well parenthesized.

Enter an expression: ({a+b*[c/d])
Expression is NOT well parenthesized.
```

---
## 3. Pizza Parlor Order Queue

**Program statement:** A pizza parlor accepts a maximum of M orders. Orders are served on a first-come-first-served basis. Write a program to simulate the job queue with functions to add jobs and delete jobs from the queue.

**Code:**

```cpp
#include <iostream>
using namespace std;

#define MAX 5
class Queue {
    int front, rear;
    int orders[MAX];

public:
    Queue() {
        front = -1;
        rear = -1;
    }
    bool isFull() {
        return (rear == MAX - 1);
    }
    bool isEmpty() {
        return (front == -1 || front > rear);
    }
    void addOrder(int orderID) {
        if (isFull()) {
            cout << "Cannot accept more orders! Queue is full.\n";
            return;
        }
        if (front == -1)
            front = 0;
        rear++;
        orders[rear] = orderID;
        cout << "Order " << orderID << " added successfully!\n";
    }
    void serveOrder() {
        if (isEmpty()) {
            cout << "No orders to serve! Queue is empty.\n";
            return;
        }
        cout << "Order " << orders[front] << " served.\n";
        front++;
        if (front > rear) {
            front = rear = -1;
        }
    }
    void displayOrders() {
        if (isEmpty()) {
            cout << "No pending orders.\n";
            return;
        }
        cout << "Pending Orders: ";
        for (int i = front; i <= rear; i++)
            cout << orders[i] << " ";
        cout << endl;
    }
};

int main() {
    Queue pizzaQueue;
    int choice, orderID;
    cout << "==== PIZZA PARLOR ORDER SYSTEM ====\n";
    do {
        cout << "\n1. Add Order\n2. Serve Order\n3. Display Orders\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
        case 1:
            cout << "Enter Order ID: ";
            cin >> orderID;
            pizzaQueue.addOrder(orderID);
            break;
        case 2:
            pizzaQueue.serveOrder();
            break;
        case 3:
            pizzaQueue.displayOrders();
            break;
        case 4:
            cout << "Exiting... Thank you for using Pizza Parlor System!\n";
            break;
        default:
            cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 4);
    return 0;
}
```

**Sample output:**

```text
==== PIZZA PARLOR ORDER SYSTEM ====

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter your choice: 1
Enter Order ID: 23
Order 23 added successfully!

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter your choice: 3
Pending Orders: 23

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter your choice: 2
Order 23 served.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter your choice: 4
Exiting... Thank you for using Pizza Parlor System!
```

---
## 4. Binary Search Tree Operations

**Program statement:** Construct a Binary Search Tree by inserting given values. Insert a new node, find the number of nodes in the longest path from root, find the minimum data value, swap left and right pointers at every node, and search a value.

**Code:**

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* createNode(int value) {
    Node* newNode = new Node();
    newNode->data = value;
    newNode->left = newNode->right = nullptr;
    return newNode;
}

Node* insert(Node* root, int value) {
    if (root == nullptr)
        return createNode(value);
    if (value < root->data)
        root->left = insert(root->left, value);
    else if (value > root->data)
        root->right = insert(root->right, value);
    return root;
}

int findMin(Node* root) {
    if (root == nullptr) {
        cout << "Tree is empty!" << endl;
        return -1;
    }
    while (root->left != nullptr)
        root = root->left;
    return root->data;
}

int findHeight(Node* root) {
    if (root == nullptr)
        return 0;
    int leftHeight = findHeight(root->left);
    int rightHeight = findHeight(root->right);
    return max(leftHeight, rightHeight) + 1;
}

void mirror(Node* root) {
    if (root == nullptr)
        return;
    mirror(root->left);
    mirror(root->right);
    Node* temp = root->left;
    root->left = root->right;
    root->right = temp;
}

bool search(Node* root, int key) {
    if (root == nullptr)
        return false;
    if (root->data == key)
        return true;
    else if (key < root->data)
        return search(root->left, key);
    else
        return search(root->right, key);
}

void inorder(Node* root) {
    if (root == nullptr)
        return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

int main() {
    Node* root = nullptr;
    int n, value;
    cout << "Enter number of nodes to insert: ";
    cin >> n;
    cout << "Enter values: ";
    for (int i = 0; i < n; i++) {
        cin >> value;
        root = insert(root, value);
    }
    cout << "\nInorder traversal of BST: ";
    inorder(root);
    cout << endl;
    cout << "\nEnter value to insert: ";
    cin >> value;
    root = insert(root, value);
    cout << "BST after insertion: ";
    inorder(root);
    cout << endl;
    cout << "\nNumber of nodes in longest path (height): " << findHeight(root) << endl;
    cout << "Minimum value in the tree: " << findMin(root) << endl;
    mirror(root);
    cout << "\nInorder traversal after mirroring: ";
    inorder(root);
    cout << endl;
    cout << "\nEnter value to search: ";
    cin >> value;
    if (search(root, value))
        cout << value << " is found in the tree.\n";
    else
        cout << value << " is NOT found in the tree.\n";
    return 0;
}
```

**Sample output:**

```text
Enter number of nodes to insert: 5
Enter values: 11 22 33 44 55

Inorder traversal of BST: 11 22 33 44 55

Enter value to insert: 66
BST after insertion: 11 22 33 44 55 66

Number of nodes in longest path (height): 6
Minimum value in the tree: 11

Inorder traversal after mirroring: 66 55 44 33 22 11

Enter value to search: 77
77 is NOT found in the tree.
```

---
## 5. Selection Sort and Bubble Sort

**Program statement:** Store first-year percentages of students in an array. Write functions to sort floating-point numbers in ascending order using Selection Sort and Bubble Sort, and display the top five scores.

**Code:**

```cpp
#include <iostream>
using namespace std;
void selectionSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex])
                minIndex = j;
        }
        float temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}
void bubbleSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                float temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
void displayTopFive(float arr[], int n) {
    cout << "\nTop Five Scores are:\n";
    int count = (n < 5) ? n : 5;
    for (int i = n - 1; i >= n - count; i--) {
        cout << arr[i] << "%\n";
    }
}
int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;
    float percentage[n];
    cout << "Enter first-year percentages of students:\n";
    for (int i = 0; i < n; i++) {
        cin >> percentage[i];
    }
    float selectionArr[n];
    for (int i = 0; i < n; i++)
        selectionArr[i] = percentage[i];
    selectionSort(selectionArr, n);
    cout << "\nPercentages sorted using Selection Sort:\n";
    for (int i = 0; i < n; i++)
        cout << selectionArr[i] << " ";
    displayTopFive(selectionArr, n);
    float bubbleArr[n];
    for (int i = 0; i < n; i++)
        bubbleArr[i] = percentage[i];
    bubbleSort(bubbleArr, n);
    cout << "\n\nPercentages sorted using Bubble Sort:\n";
    for (int i = 0; i < n; i++)
        cout << bubbleArr[i] << " ";
    displayTopFive(bubbleArr, n);
    return 0;
}
```

**Sample output:**

```text
Enter number of students: 6
Enter first-year percentages of students:
83.0 55.0 67.89 89.21 90.40 45.0

Percentages sorted using Selection Sort:
45 55 67.89 83 89.21 90.4
Top Five Scores are:
90.4%
89.21%
83%
67.89%
55%

Percentages sorted using Bubble Sort:
45 55 67.89 83 89.21 90.4
Top Five Scores are:
90.4%
89.21%
83%
67.89%
55%
```

---
## 6. Linked List Set Operations

**Program statement:** Store two sets of students using a linked list. Display students who like both Vanilla and Butterscotch, students who like either but not both, and the number of students who like neither.

**Code:**

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

struct Node {
    int data;
    struct Node* next;
};

void insert(struct Node** head, int value) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = value;
    newNode->next = NULL;
    if (*head == NULL)
        *head = newNode;
    else {
        struct Node* temp = *head;
        while (temp->next != NULL)
            temp = temp->next;
        temp->next = newNode;
    }
}

bool search(struct Node* head, int key) {
    struct Node* temp = head;
    while (temp != NULL) {
        if (temp->data == key)
            return true;
        temp = temp->next;
    }
    return false;
}

void display(struct Node* head) {
    struct Node* temp = head;
    if (temp == NULL) {
        printf("None\n");
        return;
    }
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

struct Node* intersection(struct Node* A, struct Node* B) {
    struct Node* result = NULL;
    struct Node* tempA = A;
    while (tempA != NULL) {
        if (search(B, tempA->data))
            insert(&result, tempA->data);
        tempA = tempA->next;
    }
    return result;
}

struct Node* symmetricDifference(struct Node* A, struct Node* B) {
    struct Node* result = NULL;
    struct Node* temp = A;
    while (temp != NULL) {
        if (!search(B, temp->data))
            insert(&result, temp->data);
        temp = temp->next;
    }
    temp = B;
    while (temp != NULL) {
        if (!search(A, temp->data))
            insert(&result, temp->data);
        temp = temp->next;
    }
    return result;
}

int countNeither(int totalStudents, struct Node* A, struct Node* B) {
    struct Node* temp = A;
    int countLiked = 0;
    struct Node* unionList = NULL;
    while (temp != NULL) {
        if (!search(unionList, temp->data))
            insert(&unionList, temp->data);
        temp = temp->next;
    }
    temp = B;
    while (temp != NULL) {
        if (!search(unionList, temp->data))
            insert(&unionList, temp->data);
        temp = temp->next;
    }
    temp = unionList;
    while (temp != NULL) {
        countLiked++;
        temp = temp->next;
    }
    return totalStudents - countLiked;
}

int main() {
    struct Node *vanilla = NULL, *butterscotch = NULL;
    int n1, n2, total, x;
    printf("Enter total number of students: ");
    scanf("%d", &total);
    printf("Enter number of students who like Vanilla: ");
    scanf("%d", &n1);
    printf("Enter roll numbers of students who like Vanilla:\n");
    for (int i = 0; i < n1; i++) {
        scanf("%d", &x);
        insert(&vanilla, x);
    }
    printf("Enter number of students who like Butterscotch: ");
    scanf("%d", &n2);
    printf("Enter roll numbers of students who like Butterscotch:\n");
    for (int i = 0; i < n2; i++) {
        scanf("%d", &x);
        insert(&butterscotch, x);
    }
    printf("\nStudents who like both Vanilla and Butterscotch:\n");
    struct Node* both = intersection(vanilla, butterscotch);
    display(both);
    printf("\nStudents who like either Vanilla or Butterscotch but not both:\n");
    struct Node* either = symmetricDifference(vanilla, butterscotch);
    display(either);
    printf("\nNumber of students who like neither Vanilla nor Butterscotch: %d\n",
           countNeither(total, vanilla, butterscotch));
    return 0;
}
```

**Sample output:**

```text
Enter total number of students: 4
Enter number of students who like Vanilla: 2
Enter roll numbers of students who like Vanilla:
1 2
Enter number of students who like Butterscotch: 1
Enter roll numbers of students who like Butterscotch:
3

Students who like both Vanilla and Butterscotch:
None

Students who like either Vanilla or Butterscotch but not both:
1 2 3

Number of students who like neither Vanilla nor Butterscotch: 1
```

---
## 7. Cinemax Ticket Booking Using Doubly Linked List

**Program statement:** Implement a ticket booking system for a Cinemax theater with 10 rows and 7 seats in each row. Maintain a doubly linked list for free seats in rows, display available seats, book seats, and cancel bookings.

**Code:**

```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

struct Node {
    int seat_no;
    bool booked;
    Node* next;
    Node* prev;
};

Node* createRow() {
    Node* head = nullptr;
    Node* temp = nullptr;
    for (int i = 1; i <= 7; i++) {
        Node* newNode = new Node;
        newNode->seat_no = i;
        newNode->booked = false;
        newNode->next = nullptr;
        newNode->prev = temp;
        if (head == nullptr)
            head = newNode;
        else
            temp->next = newNode;
        temp = newNode;
    }
    return head;
}

void displayAvailable(Node* rows[]) {
    cout << "\n===== AVAILABLE SEATS =====\n";
    for (int i = 0; i < 10; i++) {
        cout << "Row " << i + 1 << ": ";
        Node* temp = rows[i];
        bool anyFree = false;
        while (temp != nullptr) {
            if (!temp->booked) {
                cout << temp->seat_no << " ";
                anyFree = true;
            }
            temp = temp->next;
        }
        if (!anyFree)
            cout << "No seats available";
        cout << endl;
    }
}

void bookSeat(Node* rows[], int row, int seat) {
    if (row < 1 || row > 10 || seat < 1 || seat > 7) {
        cout << "Invalid seat or row number!\n";
        return;
    }
    Node* temp = rows[row - 1];
    while (temp != nullptr) {
        if (temp->seat_no == seat) {
            if (temp->booked) {
                cout << "Seat already booked!\n";
            } else {
                temp->booked = true;
                cout << "Seat " << seat << " in Row " << row << " booked successfully!\n";
            }
            return;
        }
        temp = temp->next;
    }
}

void cancelSeat(Node* rows[], int row, int seat) {
    if (row < 1 || row > 10 || seat < 1 || seat > 7) {
        cout << "Invalid seat or row number!\n";
        return;
    }
    Node* temp = rows[row - 1];
    while (temp != nullptr) {
        if (temp->seat_no == seat) {
            if (!temp->booked) {
                cout << "Seat was not booked!\n";
            } else {
                temp->booked = false;
                cout << "Booking for Seat " << seat << " in Row " << row << " cancelled successfully!\n";
            }
            return;
        }
        temp = temp->next;
    }
}

void randomBookings(Node* rows[]) {
    for (int i = 0; i < 10; i++) {
        int randomSeats = rand() % 4;
        for (int j = 0; j < randomSeats; j++) {
            int seatNum = (rand() % 7) + 1;
            Node* temp = rows[i];
            while (temp != nullptr) {
                if (temp->seat_no == seatNum)
                    temp->booked = true;
                temp = temp->next;
            }
        }
    }
}

int main() {
    Node* rows[10];
    for (int i = 0; i < 10; i++)
        rows[i] = createRow();
    randomBookings(rows);
    int choice, row, seat;
    do {
        cout << "\n===== CINEMAX TICKET BOOKING SYSTEM =====\n";
        cout << "1. Display available seats\n";
        cout << "2. Book a seat\n";
        cout << "3. Cancel a booking\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
        case 1:
            displayAvailable(rows);
            break;
        case 2:
            cout << "Enter row number (1-10): ";
            cin >> row;
            cout << "Enter seat number (1-7): ";
            cin >> seat;
            bookSeat(rows, row, seat);
            break;
        case 3:
            cout << "Enter row number (1-10): ";
            cin >> row;
            cout << "Enter seat number (1-7): ";
            cin >> seat;
            cancelSeat(rows, row, seat);
            break;
        case 4:
            cout << "Exiting... Thank you for visiting Cinemax!\n";
            break;
        default:
            cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 4);
    return 0;
}
```

**Sample output:**

```text
===== CINEMAX TICKET BOOKING SYSTEM =====
1. Display available seats
2. Book a seat
3. Cancel a booking
4. Exit
Enter your choice: 2
Enter row number (1-10): 2
Enter seat number (1-7): 5
Seat 5 in Row 2 booked successfully!

Enter your choice: 3
Enter row number (1-10): 2
Enter seat number (1-7): 5
Booking for Seat 5 in Row 2 cancelled successfully!

Enter your choice: 4
Exiting... Thank you for visiting Cinemax!
```

---
## 8. Telephone Book Using Hash Table

**Program statement:** Consider the telephone book database of N clients. Use a hash table to quickly look up a client’s telephone number. Use linear probing, double hashing and quadratic probing for collision handling.

**Code:**

```cpp
#include <iostream>
#include <string>
using namespace std;

const int SIZE = 10;

class HashTable {
    string name[SIZE];
    string phone[SIZE];

public:
    int hashFunction(string key) {
        int sum = 0;
        for (int i = 0; i < key.length(); i++)
            sum += key[i];
        return sum % SIZE;
    }

    void linearProbing(string n, string p) {
        int index = hashFunction(n);
        for (int i = 0; i < SIZE; i++) {
            int pos = (index + i) % SIZE;
            if (name[pos] == "") {
                name[pos] = n;
                phone[pos] = p;
                cout << "Contact added successfully.\n";
                return;
            }
        }
        cout << "Hash table is full!\n";
    }

    void quadraticProbing(string n, string p) {
        int index = hashFunction(n);
        for (int i = 0; i < SIZE; i++) {
            int pos = (index + i * i) % SIZE;
            if (name[pos] == "") {
                name[pos] = n;
                phone[pos] = p;
                cout << "Contact added successfully.\n";
                return;
            }
        }
        cout << "No free location found!\n";
    }

    void doubleHashing(string n, string p) {
        int index = hashFunction(n);
        int step = 7 - (index % 7);
        for (int i = 0; i < SIZE; i++) {
            int pos = (index + i * step) % SIZE;
            if (name[pos] == "") {
                name[pos] = n;
                phone[pos] = p;
                cout << "Contact added successfully.\n";
                return;
            }
        }
        cout << "No free location found!\n";
    }

    void search(string n) {
        for (int i = 0; i < SIZE; i++) {
            if (name[i] == n) {
                cout << "Name: " << name[i] << endl;
                cout << "Telephone Number: " << phone[i] << endl;
                return;
            }
        }
        cout << "Contact not found!\n";
    }

    void display() {
        cout << "\nTelephone Book\n";
        cout << "-------------------------\n";
        for (int i = 0; i < SIZE; i++) {
            if (name[i] != "")
                cout << i << " : " << name[i] << " - " << phone[i] << endl;
        }
    }
};

int main() {
    HashTable h;
    int choice;
    string n, p;
    do {
        cout << "\n--- Telephone Book ---\n";
        cout << "1. Insert using Linear Probing\n";
        cout << "2. Insert using Quadratic Probing\n";
        cout << "3. Insert using Double Hashing\n";
        cout << "4. Search Contact\n";
        cout << "5. Display Contacts\n";
        cout << "6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
        case 1:
            cout << "Enter client name: ";
            cin >> n;
            cout << "Enter telephone number: ";
            cin >> p;
            h.linearProbing(n, p);
            break;
        case 2:
            cout << "Enter client name: ";
            cin >> n;
            cout << "Enter telephone number: ";
            cin >> p;
            h.quadraticProbing(n, p);
            break;
        case 3:
            cout << "Enter client name: ";
            cin >> n;
            cout << "Enter telephone number: ";
            cin >> p;
            h.doubleHashing(n, p);
            break;
        case 4:
            cout << "Enter client name to search: ";
            cin >> n;
            h.search(n);
            break;
        case 5:
            h.display();
            break;
        case 6:
            cout << "Program terminated.\n";
            break;
        default:
            cout << "Invalid choice!\n";
        }
    } while (choice != 6);
    return 0;
}
```

**Sample output:**

```text
--- Telephone Book ---
1. Insert using Linear Probing
2. Insert using Quadratic Probing
3. Insert using Double Hashing
4. Search Contact
5. Display Contacts
6. Exit
Enter your choice: 2
Enter client name: Aryan
Enter telephone number: 3475120945
Contact added successfully.

Enter your choice: 3
Enter client name: Aditya
Enter telephone number: 7643598012
Contact added successfully.

Enter your choice: 4
Enter client name to search: Aditya
Name: Aditya
Telephone Number: 7643598012

Enter your choice: 5
Telephone Book
-------------------------
1 : Viram - 1342760927
4 : Aditya - 7643598012
7 : Aryan - 3475120945

Enter your choice: 6
Program terminated.
```

---
## 9. Flight Routes Graph and Connectivity

**Program statement:** Represent flight paths between cities as a graph. An edge represents a flight and its cost can be time or fuel used. Use an adjacency-list representation and check whether the graph is connected.

**Code:**

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <string>
using namespace std;

void DFS(string city, unordered_map<string, vector<pair<string, int>>> &adjList,
         unordered_map<string, bool> &visited) {
    visited[city] = true;
    for (auto neighbor : adjList[city]) {
        if (!visited[neighbor.first]) {
            DFS(neighbor.first, adjList, visited);
        }
    }
}

bool isConnected(unordered_map<string, vector<pair<string, int>>> &adjList) {
    if (adjList.empty()) return true;
    unordered_map<string, bool> visited;
    for (auto it : adjList) visited[it.first] = false;
    DFS(adjList.begin()->first, adjList, visited);
    for (auto it : visited) {
        if (!it.second) return false;
    }
    return true;
}

int main() {
    int n;
    cout << "Enter number of flights: ";
    cin >> n;
    unordered_map<string, vector<pair<string, int>>> adjList;
    cout << "Enter flights in format: CityA CityB Cost\n";
    for (int i = 0; i < n; i++) {
        string cityA, cityB;
        int cost;
        cin >> cityA >> cityB >> cost;
        adjList[cityA].push_back({cityB, cost});
        adjList[cityB].push_back({cityA, cost});
    }
    cout << "\nAdjacency List:\n";
    for (auto it : adjList) {
        cout << it.first << " -> ";
        for (auto neighbor : it.second)
            cout << "(" << neighbor.first << ", " << neighbor.second << ") ";
        cout << endl;
    }
    if (isConnected(adjList))
        cout << "\nThe graph is connected.\n";
    else
        cout << "\nThe graph is NOT connected.\n";
    return 0;
}
```

**Sample output:**

```text
Enter number of flights: 4
Enter flights in format: CityA CityB Cost
Pune Mumbai 120
Mumbai Delhi 200
Delhi Chennai 300
Pune Chennai 250

Adjacency List:
Delhi -> (Mumbai, 200) (Chennai, 300)
Chennai -> (Delhi, 300) (Pune, 250)
Mumbai -> (Pune, 120) (Delhi, 200)
Pune -> (Mumbai, 120) (Chennai, 250)

The graph is connected.
```

---
