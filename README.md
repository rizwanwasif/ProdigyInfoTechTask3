# ProdigyInfoTechTask3
import java.io.*;
import java.util.*;

class Contact {
    private String name;
    private String phoneNumber;
    private String email;

    public Contact(String name, String phoneNumber, String email) {
        this.name = name;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Phone: " + phoneNumber + ", Email: " + email;
    }
}

class ContactManager {
    private List<Contact> contacts;
    private final String fileName = "contacts.txt";

    public ContactManager() {
        contacts = new ArrayList<>();
        loadContactsFromFile();
    }

    private void loadContactsFromFile() {
        try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",");
                if (data.length == 3) {
                    contacts.add(new Contact(data[0], data[1], data[2]));
                }
            }
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
    }

    private void saveContactsToFile() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(fileName))) {
            for (Contact contact : contacts) {
                bw.write(contact.getName() + "," + contact.getPhoneNumber() + "," + contact.getEmail());
                bw.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error writing file: " + e.getMessage());
        }
    }

    public void addContact(String name, String phoneNumber, String email) {
        contacts.add(new Contact(name, phoneNumber, email));
        saveContactsToFile();
    }

    public void viewContacts() {
        if (contacts.isEmpty()) {
            System.out.println("No contacts found.");
        } else {
            for (int i = 0; i < contacts.size(); i++) {
                System.out.println((i + 1) + ". " + contacts.get(i));
            }
        }
    }

    public void editContact(int index, String name, String phoneNumber, String email) {
        if (index >= 0 && index < contacts.size()) {
            Contact contact = contacts.get(index);
            contact.setName(name);
            contact.setPhoneNumber(phoneNumber);
            contact.setEmail(email);
            saveContactsToFile();
        } else {
            System.out.println("Invalid contact index.");
        }
    }

    public void deleteContact(int index) {
        if (index >= 0 && index < contacts.size()) {
            contacts.remove(index);
            saveContactsToFile();
        } else {
            System.out.println("Invalid contact index.");
        }
    }
}

public class ContactManagerApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ContactManager manager = new ContactManager();

        while (true) {
            System.out.println("\nContact Manager:");
            System.out.println("1. Add New Contact");
            System.out.println("2. View Contacts");
            System.out.println("3. Edit Contact");
            System.out.println("4. Delete Contact");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");

            int option = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (option) {
                case 1:
                    System.out.print("Enter name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter phone number: ");
                    String phoneNumber = scanner.nextLine();
                    System.out.print("Enter email: ");
                    String email = scanner.nextLine();
                    manager.addContact(name, phoneNumber, email);
                    break;
                case 2:
                    manager.viewContacts();
                    break;
                case 3:
                    manager.viewContacts();
                    System.out.print("Enter contact number to edit: ");
                    int editIndex = scanner.nextInt() - 1;
                    scanner.nextLine();  // Consume newline
                    System.out.print("Enter new name: ");
                    String newName = scanner.nextLine();
                    System.out.print("Enter new phone number: ");
                    String newPhoneNumber = scanner.nextLine();
                    System.out.print("Enter new email: ");
                    String newEmail = scanner.nextLine();
                    manager.editContact(editIndex, newName, newPhoneNumber, newEmail);
                    break;
                case 4:
                    manager.viewContacts();
                    System.out.print("Enter contact number to delete: ");
                    int deleteIndex = scanner.nextInt() - 1;
                    manager.deleteContact(deleteIndex);
                    break;
                case 5:
                    System.out.println("Exiting Contact Manager...");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}
