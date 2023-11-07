import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.LinkedList;
import java.util.Queue;
import javax.swing.*;


public class CovidMonitor extends JFrame{
    
    private static Queue<Patient> queue = new LinkedList<>();
    private JTextArea textArea;
    private JButton addButton;
    private JButton processButton;

    public CovidMonitor(){
        setTitle("Covid-19 Monitor");
        setSize(400, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JLabel titleLabel = new JLabel("Covid-19 Patient Monitoring", SwingConstants.CENTER);
        titleLabel.setFont(titleLabel.getFont().deriveFont(24.0f));
        add(titleLabel, BorderLayout.NORTH);

        JLabel helloLabel = new JLabel("PHINMAED Case Program", SwingConstants.LEFT);
        add(helloLabel, BorderLayout.SOUTH);

        textArea = new JTextArea();
        textArea.setEditable(false);
        add(new JScrollPane(textArea), BorderLayout.CENTER);

        addButton = new JButton("Add Patient");
        addButton.addActionListener(new AddButtonListener());

        processButton = new JButton("Process Patient");
        processButton.addActionListener(new ProcessButtonListener());

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(addButton);
        buttonPanel.add(processButton);
        add(buttonPanel, BorderLayout.SOUTH);

        setVisible(true);
    }

    private class AddButtonListener implements ActionListener{
        @Override
        public void actionPerformed(ActionEvent e) {
            String firstName = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's firt name: ");
            String lastName = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's last name: ");
            String symptons = JOptionPane.showInputDialog(CovidMonitor.this, "Enter the patient's symptoms: ");
            queue.add(new Patient(firstName, lastName, symptons));
            textArea.append("Added patient: " + firstName + " " +  lastName +  "(" + symptons + ")\n");
        }
    }

    private class ProcessButtonListener implements ActionListener{
        @Override
        public void actionPerformed(ActionEvent e) {
            if (!queue.isEmpty()) {
                Patient patient = queue.poll();
                textArea.append("Processing patient: " + patient.getFirstName() + " " + patient.getLastName() + "(" + patient.getSymptoms() + ")\n");
            } else {
                textArea.append("No patients to process\n");
            }
        }
        
    }
}

public class Main {
    public static void main(String[] args) {
        CovidMonitor monitor = new CovidMonitor();
        Patient patient = new Patient(null, null, null);
    }  
}


public class Patient {
    private String firstName;
    private String lastName;
    private String symptoms;

    public Patient(String firstName, String lastName, String symptoms){
        this.firstName = firstName;
        this.lastName = lastName;
        this.symptoms = symptoms;
    }

    public String getFirstName(){
        return firstName;
    }

    public String getLastName(){
        return lastName;
    }

     public String getSymptoms(){
        return symptoms;
    }
}
