q-1)

class Total {
    private int totalSum = 0;

    public synchronized void updateTotal(int value) {
        totalSum += value;
    }

    public int getTotal() {
        return totalSum;
    }
}

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


q-3)

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

        for (int i = 0; i < 3; i++) {
            System.out.println("Enter details for employee " + (i + 1));
            System.out.print("Emp ID: ");
            int id = sc.nextInt();
            System.out.print("Salary: ");
            double salary = sc.nextDouble();
            System.out.print("Department: ");
            sc.nextLine();
            String dept = sc.nextLine();

            if (salary < 10000) {
                salary += salary * 0.10;
            }

            employees[i] = new Employee(id, salary, dept);
        }

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



Q1) Explain 3 ways to achieve thread synchronization.
Synchronized Method – Entire method is locked for a thread.

Synchronized Block – Only a specific section of code is locked.

ReentrantLock (from java.util.concurrent.locks) – A more flexible and powerful way to control locking than synchronized keyword.

Q2) Explain why should we create a thread and not a process? (Mention 3 points)
Threads are lightweight and consume less memory than processes.

Threads within a process share the same memory, so data sharing is easier.

Creating threads is faster and has less overhead than creating processes.

Q3) Explain 3 differences between throw and throws.
throw is used to explicitly throw an exception, while throws is used to declare exceptions.

throw is followed by an instance; throws is followed by exception class names.

throw appears inside the method; throws is in the method signature.

Q4) Write 3 methods common among List and Set, and explain with help of code.
add() – Adds an element.

remove() – Removes an element.

contains() – Checks if element exists.

List<String> list = new ArrayList<>();
Set<String> set = new HashSet<>();
list.add("Java"); set.add("Java");
list.contains("Java"); set.contains("Java");

Q5) Explain what is the use of a Driver in JDBC.
A JDBC Driver acts as a bridge between Java application and the database.
It helps in establishing a connection, sending SQL queries, and retrieving results.
Without a driver, Java cannot communicate with the database.

