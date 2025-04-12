# manage-employee-data.c
//---------------------------------------------------------------------------------------------------
//Employee Payroll Management System (Using Linked List)
//You are developing an Employee Payroll Management System using a singly linked list to store
//and manage employee salary details. The system should allow the following operations:
//1. Add a new employee's salary details (insert at the end).
//2. Remove an employee by their Employee ID (deletion).
//3. Search for an employee by their Employee ID (traversing and comparing).
//4. Display all employees sorted by Employee ID.
//Each employee should have the following attributes:
//● Employee ID (integer)
//● Name (string)
//● Salary (integer)
//------------------------------------------------------------------------------------------------------------
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Employee structure for linked list
struct Employee {
    int employeeID;
    char name[100];
    float salary;
    struct Employee* next;  // Pointer to the next employee
};
void linklistTraversal(struct Employee* ptr) {
    while (ptr != NULL) {
        printf("ID: %d, Name: %s, Salary: %.2f\n", ptr->employeeID, ptr->name, ptr->salary);
        ptr = ptr->next;
    }
}


void at_end(struct Employee** head, int id, const char* name, float salary) {
    // Allocate memory for the new employee node
    struct Employee* newEmployee = (struct Employee*)malloc(sizeof(struct Employee));
    if (newEmployee == NULL) {
        printf("Memory allocation failed!\n");
        return;
    }

    // Set data for the new node
    newEmployee->employeeID = id;
    strcpy(newEmployee->name, name);
    newEmployee->salary = salary;
    newEmployee->next = NULL;

    // If the list is empty, the new employee becomes the head
    if (*head == NULL) {
        *head = newEmployee;
    } else {
        // Traverse to the last employee
        struct Employee* temp = *head;
        while (temp->next != NULL) {
            temp = temp->next;
        }

        // Link the new employee at the end of the list
        temp->next = newEmployee;
    }
}

void removeEmployee(struct Employee** head, int id) {
    struct Employee* temp = *head;
    struct Employee* prev = NULL;

    // If the list is empty
    if (temp == NULL) {
        printf("The list is empty.\n");
        return;
    }

    // Search for the employee with the given ID
    while (temp != NULL && temp->employeeID != id) {
        prev = temp;
        temp = temp->next;
    }

    // If employee not found
    if (temp == NULL) {
        printf("Employee with ID %d not found.\n", id);
        return;
    }

    // If the employee is the first in the list
    if (prev == NULL) {
        *head = temp->next;
    } else {
        prev->next = temp->next;
    }

    // Free memory for the removed employee
    free(temp);
    printf("Employee with ID %d removed.\n", id);
}

// Function to search for an employee by their Employee ID
void searchEmployee(struct Employee* head, int id) {
    struct Employee* current = head;
    while (current != NULL) {
        if (current->employeeID == id) {
            printf("Employee found: ID: %d, Name: %s, Salary: %.2f\n", current->employeeID, current->name, current->salary);
            return;
        }
        current = current->next;
    }
    printf("Employee with ID %d not found.\n", id);
}

// Function to sort employees by Employee ID using Bubble Sort
void sortEmployees(struct Employee* head) {
    struct Employee* current;
    struct Employee* nextNode;
    int tempID;
    char tempName[100];
    float tempSalary;

    for (current = head; current != NULL; current = current->next) {
        for (nextNode = current->next; nextNode != NULL; nextNode = nextNode->next) {
            if (current->employeeID > nextNode->employeeID) {
                // Swap data
                tempID = current->employeeID;
                current->employeeID = nextNode->employeeID;
                nextNode->employeeID = tempID;

                strcpy(tempName, current->name);
                strcpy(current->name, nextNode->name);
                strcpy(nextNode->name, tempName);

                tempSalary = current->salary;
                current->salary = nextNode->salary;
                nextNode->salary = tempSalary;
            }
        }
    }
}

int main() {
    struct Employee* head = NULL;
    int choice, id;
    char name[100];
    float salary;

    while (1) {
        printf("\nEmployee Payroll Management System\n");
        printf("1. Add Employee\n");
        printf("2. Remove Employee by ID\n");
        printf("3. Search Employee by ID\n");
        printf("4. Display Payroll\n");
        printf("5. Sort Employees by ID\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter Employee ID: ");
                scanf("%d", &id);
                printf("Enter Employee Name: ");
                scanf(" %[^\n]%*c", name);  // Read full name with spaces
                printf("Enter Employee Salary: ");
                scanf("%f", &salary);
                at_end(&head, id, name, salary);
                break;

            case 2:
                printf("Enter Employee ID to remove: ");
                scanf("%d", &id);
                removeEmployee(&head, id);
                break;

            case 3:
                printf("Enter Employee ID to search: ");
                scanf("%d", &id);
                searchEmployee(head, id);
                break;

            case 4:
                printf("Employee Payroll:\n");
                linklistTraversal(head);
                break;

            case 5:
                sortEmployees(head);
                printf("Employees sorted by ID.\n");
                break;

            case 6:
                printf("Exiting program.\n");
                exit(0);

            default:
                printf("Invalid choice. Try again.\n");
        }
    }

    return 0;
}

