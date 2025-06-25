import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class CalculatorSwing extends JFrame implements ActionListener {
    JTextField t1, t2, result;
    JButton add, sub, mul, div;

    CalculatorSwing() {
        // Create labels
        JLabel l1 = new JLabel("First Number:");S
        JLabel l2 = new JLabel("Second Number:");
        JLabel l3 = new JLabel("Result:");

        // Create text fields
        t1 = new JTextField();
        t2 = new JTextField();
        result = new JTextField();
        result.setEditable(false);

        // Create buttons
        add = new JButton("+");
        sub = new JButton("-");
        mul = new JButton("*");
        div = new JButton("/");

        // Set layout to null
        setLayout(null);

        // Set bounds
        l1.setBounds(30, 50, 100, 20);
        t1.setBounds(140, 50, 100, 20);

        l2.setBounds(30, 90, 100, 20);
        t2.setBounds(140, 90, 100, 20);

        l3.setBounds(30, 130, 100, 20);
        result.setBounds(140, 130, 100, 20);

        add.setBounds(30, 170, 50, 30);
        sub.setBounds(90, 170, 50, 30);
        mul.setBounds(150, 170, 50, 30);
        div.setBounds(210, 170, 50, 30);

        // Add action listeners
        add.addActionListener(this);
        sub.addActionListener(this);
        mul.addActionListener(this);
        div.addActionListener(this);

        // Add components to frame
        add(l1); add(t1);
        add(l2); add(t2);
        add(l3); add(result);
        add(add); add(sub); add(mul); add(div);

        // JFrame settings
        setTitle("Calculator using JFrame");
        setSize(320, 270);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        try {
            double a = Double.parseDouble(t1.getText());
            double b = Double.parseDouble(t2.getText());
            double res = 0;

            if (e.getSource() == add) {
                res = a + b;
            } else if (e.getSource() == sub) {
                res = a - b;
            } else if (e.getSource() == mul) {
                res = a * b;
            } else if (e.getSource() == div) {
                if (b != 0)
                    res = a / b;
                else {
                    result.setText("Error: /0");
                    return;
                }
            }

            result.setText(String.valueOf(res));
        } catch (NumberFormatException ex) {
            result.setText("Invalid Input");
        }
    }

    public static void main(String[] args) {
        new CalculatorSwing();
    }
}
