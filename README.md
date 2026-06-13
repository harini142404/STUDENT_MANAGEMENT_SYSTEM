# STUDENT_MANAGEMENT_SYSTEM

import java.awt.*;
import javax.swing.*;
import javax.swing.table.DefaultTableModel;

public class StudentManagementSystem extends JFrame {
    private JTextField idField, nameField, marksField;
    private JTable table;
    private DefaultTableModel model;
    public StudentManagementSystem() {
        setTitle("Student Management System");
        setSize(850, 500);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());
        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10));
        inputPanel.add(new JLabel("Student ID:"));
        idField = new JTextField();
        inputPanel.add(idField);
        inputPanel.add(new JLabel("Student Name:"));
        nameField = new JTextField();
        inputPanel.add(nameField);
        inputPanel.add(new JLabel("Marks:"));
        marksField = new JTextField();
        inputPanel.add(marksField);
        add(inputPanel, BorderLayout.NORTH);
        model = new DefaultTableModel();
        model.addColumn("ID");
        model.addColumn("Name");
        model.addColumn("Marks");
        model.addColumn("Grade");
        table = new JTable(model);
        JScrollPane scrollPane = new JScrollPane(table);
        add(scrollPane, BorderLayout.CENTER);
        JPanel buttonPanel = new JPanel();
        JButton addBtn = new JButton("Add");
        JButton viewBtn = new JButton("View");
        JButton searchBtn = new JButton("Search");
        JButton updateBtn = new JButton("Update");
        JButton deleteBtn = new JButton("Delete");
        buttonPanel.add(addBtn);
        buttonPanel.add(viewBtn);
        buttonPanel.add(searchBtn);
        buttonPanel.add(updateBtn);
        buttonPanel.add(deleteBtn);
        add(buttonPanel, BorderLayout.SOUTH);
        addBtn.addActionListener(e -> addStudent());
        viewBtn.addActionListener(e -> viewStudents());
        searchBtn.addActionListener(e -> searchStudent());
        updateBtn.addActionListener(e -> updateStudent());
        deleteBtn.addActionListener(e -> deleteStudent());
        setVisible(true);
    }
    private String calculateGrade(double marks) {
        if (marks >= 90)
            return "A";
        else if (marks >= 80)
            return "B";
        else if (marks >= 70)
            return "C";
        else
            return "F";
    }
    private void addStudent() {
        try {
            int id = Integer.parseInt(idField.getText());
            String name = nameField.getText();
            double marks = Double.parseDouble(marksField.getText());
            String grade = calculateGrade(marks);
            model.addRow(new Object[]{
                    id,
                    name,
                    marks,
                    grade
            });
            clearFields();
            JOptionPane.showMessageDialog(
                    this,
                    "Student Added Successfully");
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(
                    this,
                    "Please enter valid data");
        }
    }
    private void viewStudents() {
        if (model.getRowCount() == 0) {
            JOptionPane.showMessageDialog(
                    this,
                    "No Student Records Found");
        }
        else {
            JOptionPane.showMessageDialog(
                    this,
                    "Students displayed in table");
        }
    }
    private void searchStudent() {
        try {
            int searchId =
                    Integer.parseInt(idField.getText());
            boolean found = false;
            for (int i = 0; i < model.getRowCount(); i++) {
                int id =
                        Integer.parseInt(
                                model.getValueAt(i, 0)
                                        .toString());
                if (id == searchId) {
                    table.setRowSelectionInterval(i, i);
                    nameField.setText(
                            model.getValueAt(i, 1)
                                    .toString());
                    marksField.setText(
                            model.getValueAt(i, 2)
                                    .toString());
                    found = true;
                    JOptionPane.showMessageDialog(
                            this,
                            "Student Found");
                    break;
                }
            }
            if (!found) {
                JOptionPane.showMessageDialog(
                        this,
                        "Student Not Found");
            }
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(
                    this,
                    "Enter valid Student ID");
        }
    }
    private void updateStudent() {
        int row = table.getSelectedRow();
        if (row == -1) {
            JOptionPane.showMessageDialog(
                    this,
                    "Search or Select a Student First");
            return;
        }
        try {
            int id =
                    Integer.parseInt(idField.getText());
            String name =
                    nameField.getText();
            double marks =
                    Double.parseDouble(
                            marksField.getText());
            String grade =
                    calculateGrade(marks);
            model.setValueAt(id, row, 0);
            model.setValueAt(name, row, 1);
            model.setValueAt(marks, row, 2);
            model.setValueAt(grade, row, 3);
            JOptionPane.showMessageDialog(
                    this,
                    "Student Updated Successfully");
            clearFields();
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(
                    this,
                    "Invalid Data");
        }
    }
    private void deleteStudent() {
        int row = table.getSelectedRow();
        if (row == -1) {
            JOptionPane.showMessageDialog(
                    this,
                    "Select a Student First");
            return;
        }
        model.removeRow(row);
        JOptionPane.showMessageDialog(
                this,
                "Student Deleted Successfully");
        clearFields();
    }
    private void clearFields() {
        idField.setText("");
        nameField.setText("");
        marksField.setText("");
    }
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() ->
                new StudentManagementSystem());
    }
}
