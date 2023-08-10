import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CalculatorApp extends JFrame {
    private JTextField displayField;
    private JButton[] digitButtons;
    private JButton[] operatorButtons;
    private JButton equalsButton;
    private JButton clearButton;

    private double firstOperand;
    private String operator;

    public CalculatorApp() {
        setTitle("Calculator");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        displayField = new JTextField();
        displayField.setEditable(false);
        displayField.setHorizontalAlignment(JTextField.RIGHT);
        add(displayField, BorderLayout.NORTH);

        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new GridLayout(4, 4));

        digitButtons = new JButton[10];
        for (int i = 0; i < 10; i++) {
            digitButtons[i] = new JButton(Integer.toString(i));
            final int digit = i;
            digitButtons[i].addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    displayField.setText(displayField.getText() + digit);
                }
            });
            buttonPanel.add(digitButtons[i]);
        }

        operatorButtons = new JButton[4];
        operatorButtons[0] = new JButton("+");
        operatorButtons[1] = new JButton("-");
        operatorButtons[2] = new JButton("*");
        operatorButtons[3] = new JButton("/");
        ActionListener operatorListener = new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                firstOperand = Double.parseDouble(displayField.getText());
                operator = ((JButton) e.getSource()).getText();
                displayField.setText("");
            }
        };
        for (int i = 0; i < 4; i++) {
            operatorButtons[i].addActionListener(operatorListener);
            buttonPanel.add(operatorButtons[i]);
        }

        equalsButton = new JButton("=");
        equalsButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                double secondOperand = Double.parseDouble(displayField.getText());
                double result = calculateResult(firstOperand, secondOperand, operator);
                displayField.setText(Double.toString(result));
            }
        });
        buttonPanel.add(equalsButton);

        clearButton = new JButton("C");
        clearButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                displayField.setText("");
            }
        });
        buttonPanel.add(clearButton);

        add(buttonPanel, BorderLayout.CENTER);

        pack();
        setLocationRelativeTo(null);
        setVisible(true);
    }

    private double calculateResult(double firstOperand, double secondOperand, String operator) {
        double result = 0;
        switch (operator) {
            case "+":
                result = firstOperand + secondOperand;
                break;
            case "-":
                result = firstOperand - secondOperand;
                break;
            case "*":
                result = firstOperand * secondOperand;
                break;
            case "/":
                result = firstOperand / secondOperand;
                break;
        }
        return result;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new CalculatorApp();
            }
        });
    }
}
