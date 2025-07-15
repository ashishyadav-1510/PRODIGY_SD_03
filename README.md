# Contact Management System in Python

This is a **console-based Contact Management System** built using Python. It allows users to **add, view, edit, and delete** contact information with full validation and persistent file storage.

## Features

✅ Add new contacts with:

* Valid name format (e.g., `John Doe`)
* 10-digit unique phone number
* Gmail address validation (e.g., `example@gmail.com`)
  ✅ View all contacts in a tabular format
  ✅ Edit existing contact details
  ✅ Delete contacts
  ✅ Serial numbers update automatically
  ✅ Data is saved in a `contacts.txt` file for persistence

## Input Validations

| Field | Validation Rules                                                     |
| ----- | -------------------------------------------------------------------- |
| Name  | Only letters; each word starts with a capital (e.g., "Ashish Yadav") |
| Phone | Exactly 10 digits and must be unique                                 |
| Email | Must end with `@gmail.com`and be unique                            |

## Screenshots

## Code:

![image](https://github.com/ashishyadav-1510/PRODIGY_SD_03/blob/main/screenshot/Screenshot%202025-07-15%20133318.png?raw=true)
![image](https://github.com/ashishyadav-1510/PRODIGY_SD_03/blob/main/screenshot/Screenshot%202025-07-15%20133409.png?raw=true)
![image](https://github.com/ashishyadav-1510/PRODIGY_SD_03/blob/main/screenshot/Screenshot%202025-07-15%20133429.png?raw=true)

## Output:
![image](https://github.com/ashishyadav-1510/PRODIGY_SD_03/blob/main/screenshot/Screenshot%202025-07-15%20133609.png?raw=true)

## Sample File Generated:
![image](https://github.com/ashishyadav-1510/PRODIGY_SD_03/blob/main/screenshot/Screenshot%202025-07-15%20133636.png?raw=true)
## Video
[Video on YouTube](https://youtu.be/gjilwdC_T50)

## Explaination

✅ Importing Required Module
import re
re is Python’s regular expression module.
It is used to perform pattern matching and validation, e.g., checking if the name or email address matches specific rules.

✅ File Name Constant
CONTACTS_FILE = "contacts.txt"
This defines the name of the file where contacts are stored and loaded from.
All contact details will be saved and retrieved from this text file.

✅ Function: load_contacts()
def load_contacts():
    contacts = []
    try:
        with open(CONTACTS_FILE, "r") as file:
            lines = file.readlines()[2:]  # Skip headers (first 2 lines)
            for line in lines:
                parts = line.strip().split(" | ")
                if len(parts) == 4:
                    _, name, phone, email = parts
                    contacts.append({"name": name, "phone": phone, "email": email})
    except FileNotFoundError:
        pass
    return contacts
🔍 What It Does:
Reads the contacts.txt file.
Skips the first 2 lines (headers and separator).
Splits each line using " | " separator.
Converts valid lines into dictionaries with keys: name, phone, and email.
Returns a list of contacts.

✅ Function: save_contacts(contacts)
def save_contacts(contacts):
    with open(CONTACTS_FILE, "w") as file:
        file.write("{:<5} | {:<20} | {:<15} | {:<25}\n".format("No.", "Name", "Phone", "Email"))
        file.write("-" * 75 + "\n")
        for idx, contact in enumerate(contacts, 1):
            file.write("{:<5} | {:<20} | {:<15} | {:<25}\n".format(
                idx, contact['name'], contact['phone'], contact['email']))
🔍 What It Does:
Opens the contact file in write mode ("w"), so it overwrites each time.
Writes a header with column names and a line separator.
Writes each contact line-by-line with proper formatting and serial numbers.
Uses .format() to ensure columns are aligned.

✅ Function: is_unique(field, value, contacts, current_index=None)
def is_unique(field, value, contacts, current_index=None):
    for idx, contact in enumerate(contacts):
        if idx == current_index:
            continue
        if contact[field] == value:
            return False
    return True
🔍 What It Does:
Ensures no duplicate phone or email exists in the list.
Skips the current contact if editing (to avoid comparing with itself).
Returns False if the value exists already in any contact; otherwise True.

✅ Function: input_contact_fields(...)
def input_contact_fields(contacts, existing=None, current_index=None):
This is the core input and validation function used during add or edit.

🔍 Name Input Section:
while True:
    name = input(f"Enter Name {existing['name'] if existing else ''}: ").strip()
    if not name and existing:
        name = existing['name']
    else:
        name = name.title()
        if not re.fullmatch(r"[A-Z][a-z]*([ ][A-Z][a-z]*)*", name):
            print("Something Wrong!!Invalid name.\nUse only letters.")
            continue
Prompts the user to enter the name.
Accepts empty input only if editing (keeps the old name).
Uses re.fullmatch(...) to check if the name:
Starts with a capital letter.
Contains only letters.
Can have multiple capitalized words (e.g., Ashish Yadav).

🔍 Phone Number Section:
while True:
    phone = input(f"Enter Mobile Number {existing['phone'] if existing else ''}: ").strip()
    if not phone and existing:
        phone = existing['phone']
        break
    if not phone.isdigit() or len(phone) != 10:
        print("Something wrong!!Invalid phone number.\nEnter exactly 10 digits.")
        continue
    if not is_unique("phone", phone, contacts, current_index):
        print("This phone number already exists.")
        continue
    break
Accepts phone number input.
Must be exactly 10 digits.
Must be unique (no two contacts can share the same number).
isdigit() ensures all characters are numbers.

🔍 Email Section:
while True:
    email = input(f"Enter Email Address {existing['email'] if existing else ''}: ").strip()
    if not email and existing:
        email = existing['email']
        break
    if not re.fullmatch(r"[a-zA-Z0-9._%+-]+@gmail\.com", email):
        print("Something wrong!!Invalid email format.\nMust be a valid Gmail address(example@gmail.com).")
        continue
    if not is_unique("email", email, contacts, current_index):
        print("This email ID already exists.")
        continue
    break
Validates the email format using regex.
Only Gmail addresses are allowed (e.g., abc@gmail.com).
Email must also be unique.

🔍 Return the Validated Contact:
return {"name": name, "phone": phone, "email": email}
✅ Add Contact
def add_contact(contacts):
    contact = input_contact_fields(contacts)
    contacts.append(contact)
    save_contacts(contacts)
    print("Contact Added Successfully!")
🔍 What It Does:
Gets input with validation.
Appends it to the list.
Saves all contacts to file.
Displays success message.

✅ View Contacts
def view_contacts(contacts):
    if not contacts:
        print("No contacts Present")
        return
    print("\n*** Contact List ***")
    print("{:<5} {:<20} {:<15} {:<25}".format("No.", "Name", "Phone", "Email"))
    print("-" * 70)
    for i, contact in enumerate(contacts, 1):
        print("{:<5} {:<20} {:<15} {:<25}".format(i, contact['name'], contact['phone'], contact['email']))
🔍 What It Does:
If no contacts, shows message.
Otherwise, prints a formatted table of contacts using .format().

✅ Edit Contact
def edit_contact(contacts):
    view_contacts(contacts)
    try:
        index = int(input("Enter Contact Serial Number To Edit: ")) - 1
        if 0 <= index < len(contacts):
            contacts[index] = input_contact_fields(contacts, existing=contacts[index], current_index=index)
            save_contacts(contacts)
            print("Contact Updated Successfully.")
        else:
            print("Invalid serial number.")
    except ValueError:
        print("Invalid input.")
🔍 What It Does:
Shows the contact list.
Asks for serial number (1-based index).
Uses input_contact_fields() to update the fields with validation.
Replaces the old contact with new.
Saves to file again.

✅ Delete Contact
def delete_contact(contacts):
    view_contacts(contacts)
    try:
        index = int(input("Enter Contact Serial Number To Delete: ")) - 1
        if 0 <= index < len(contacts):
            removed = contacts.pop(index)
            save_contacts(contacts)
            print(f"Deleted contact: {removed['name']}")
        else:
            print("Invalid serial number.")
    except ValueError:
        print("Invalid input.")
🔍 What It Does:
Shows contacts.
Asks for serial number to delete.
Removes that contact from the list using pop().
Saves updated list.
Displays which contact was deleted.

✅ Main Menu Loop
def main():
    contacts = load_contacts()
    options = {
        "1": add_contact,
        "2": view_contacts,
        "3": edit_contact,
        "4": delete_contact
    }

    while True:
        print("\n#### Contact Management System ####")
        print("1. Add Contact")
        print("2. View Contacts")
        print("3. Edit Contact")
        print("4. Delete Contact")
        print("5. Exit")

    choice = input("Choose an option (1-5): ").strip()

    if choice in options:
            options[choice](contacts)
        elif choice == "5":
            save_contacts(contacts)
            print("Exited Successfully")
            break
        else:
            print("Invalid Choice!!")
🔍 What It Does:
Loads contacts from file.
Shows menu in a loop until user chooses to exit.
Calls the correct function based on user input.
Always ensures data is saved before exit.

✅ Entry Point
if __name__ == "__main__":
    main()
Ensures the main() function runs only when this script is executed directly, not when imported elsewhere.

## Author

**Ashish Yadav**
