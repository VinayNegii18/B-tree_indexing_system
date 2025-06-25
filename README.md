Q-1)
// Class to hold the total sum safely
class Total {
    private int totalSum = 0;

    // synchronized method to safely update total
    public synchronized void updateTotal(int value) {
        totalSum += value;
    }

    public int getTotal() {
        return totalSum;
    }
}

// Thread to calculate sum of first 100 even numbers
class EvenSumThread extends Thread {
    Total total;

    EvenSumThread(Total total) {
        this.total = total;
    }

    public void run() {
        int sumEven = 0;
        for (int i = 2, count = 0; count < 100; i += 2, count++) {
            sumEven += i;
        }
        total.updateTotal(sumEven);
        System.out.println("Even sum: " + sumEven);
    }
}

// Thread to calculate sum of first 100 odd numbers
class OddSumThread extends Thread {
    Total total;

    OddSumThread(Total total) {
        this.total = total;
    }

    public void run() {
        int sumOdd = 0;
        for (int i = 1, count = 0; count < 100; i += 2, count++) {
            sumOdd += i;
        }
        total.updateTotal(sumOdd);
        System.out.println("Odd sum: " + sumOdd);
    }
}

// Main class to run both threads
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Total total = new Total();

        EvenSumThread t1 = new EvenSumThread(total);
        OddSumThread t2 = new OddSumThread(total);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Total Sum = " + total.getTotal());
    }
}



.................................................................................................................

import java.sql.*;
import java.util.*;

class Employee {
    int empId;
    double salary;
    String department;

    Employee(int empId, double salary, String department) {
        this.empId = empId;
        this.salary = salary;
        this.department = department;
    }
}

public class EmployeeDB {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Employee[] employees = new Employee[3];

        // Taking input from user
        for (int i = 0; i < 3; i++) {
            System.out.println("Enter details for employee " + (i + 1));
            System.out.print("Emp ID: ");
            int id = sc.nextInt();
            System.out.print("Salary: ");
            double salary = sc.nextDouble();
            System.out.print("Department: ");
            sc.nextLine();  // consume newline
            String dept = sc.nextLine();

            if (salary < 10000) {
                salary += salary * 0.10; // increase by 10%
            }

            employees[i] = new Employee(id, salary, dept);
        }

        // Database update logic
        try {
            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "root", "root");
            String query = "UPDATE employee SET salary=?, department=? WHERE empId=?";
            PreparedStatement ps = con.prepareStatement(query);

            for (Employee emp : employees) {
                ps.setDouble(1, emp.salary);
                ps.setString(2, emp.department);
                ps.setInt(3, emp.empId);
                ps.executeUpdate();
            }

            System.out.println("Employees updated successfully.");

        } catch (SQLException e) {
            System.out.println("SQL Error: " + e.getMessage());
        }
    }
}

