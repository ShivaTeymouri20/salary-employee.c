#include <stdio.h>
#include <stdlib.h>

typedef struct {
    char name[50];
    int id;
    float basic_salary;
    float insurance_deduction;
    float net_salary;
} Employee;

void calculateSalary(Employee *e) {
    e->insurance_deduction = e->basic_salary * 0.05;  
    e->net_salary = e->basic_salary - e->insurance_deduction;
}

void generateSalarySlip(Employee *e) {
    printf("\n--- Salary Slip for Employee ID: %d ---\n", e->id);
    printf("Name: %s\n", e->name);
    printf("Basic Salary: %.2f\n", e->basic_salary);
    printf("Insurance Deduction (5%%): %.2f\n", e->insurance_deduction);
    printf("Net Salary: %.2f\n", e->net_salary);
    printf("-----------------------------------\n");
}

int main() {
    Employee employees[EMP_COUNT];
    FILE *file;
    file = fopen("salaries.txt", "w"); 
    if (file == NULL) {
        printf("Error opening file!\n");
        return 1;
    }

    printf("Enter details for %d employees:\n", EMP_COUNT);
    for (int i = 0; i < EMP_COUNT; i++) {
        printf("\nEmployee %d\n", i + 1);
        printf("Enter ID: ");
        scanf("%d", &employees[i].id);
        printf("Enter Name: ");
        scanf("%s", employees[i].name);
        printf("Enter Basic Salary: ");
        scanf("%f", &employees[i].basic_salary);

        calculateSalary(&employees[i]);

      
        fprintf(file, "ID: %d, Name: %s, Basic Salary: %.2f, Insurance Deduction: %.2f, Net Salary: %.2f\n",
                employees[i].id, employees[i].name, employees[i].basic_salary,
                employees[i].insurance_deduction, employees[i].net_salary);
    }

    fclose(file);
    printf("\nSalaries saved to 'salaries.txt'.\n");


    for (int i = 0; i < EMP_COUNT; i++) {
        generateSalarySlip(&employees[i]);
    }

    return 0;
}
