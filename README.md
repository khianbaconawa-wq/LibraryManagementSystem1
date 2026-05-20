# LibraryManagementSystem1
# ==============================
# LIBRARY MANAGEMENT SYSTEM
# ==============================

books = []
members = []
loans = []


# ADD BOOK
def add_book():
    print("\n--- ADD BOOK ---")

    title = input("Enter Book Title: ").strip()
    author = input("Enter Author: ").strip()

    try:
        quantity = int(input("Enter Quantity: "))

        if quantity <= 0:
            print("Quantity must be greater than 0!\n")
            return

    except ValueError:
        print("Invalid quantity!\n")
        return

    # CHECK IF BOOK ALREADY EXISTS
    for book in books:
        if book["title"].lower() == title.lower():
            book["quantity"] += quantity
            print("Book already exists. Quantity updated!\n")
            return

    book = {
        "title": title,
        "author": author,
        "quantity": quantity
    }

    books.append(book)

    print("Book added successfully!\n")


# REGISTER MEMBER
def register_member():
    print("\n--- REGISTER MEMBER ---")

    name = input("Enter Member Name: ").strip()
    member_id = input("Enter Member ID: ").strip()

    # CHECK DUPLICATE MEMBER ID
    for member in members:
        if member["id"] == member_id:
            print("Member ID already exists!\n")
            return

    member = {
        "name": name,
        "id": member_id
    }

    members.append(member)

    print("Member registered successfully!\n")


# BORROW BOOK
def borrow_book():
    print("\n--- BORROW BOOK ---")

    member_name = input("Enter Member Name: ").strip()
    book_title = input("Enter Book Title: ").strip()

    # CHECK IF MEMBER EXISTS
    member_found = False

    for member in members:
        if member["name"].lower() == member_name.lower():
            member_found = True
            break

    if not member_found:
        print("Member not registered!\n")
        return

    # CHECK BOOK
    for book in books:
        if book["title"].lower() == book_title.lower():

            if book["quantity"] > 0:

                loan = {
                    "member": member_name,
                    "book": book["title"]
                }

                loans.append(loan)

                book["quantity"] -= 1

                print("Book borrowed successfully!\n")
                return

            else:
                print("Book is unavailable!\n")
                return

    print("Book not found!\n")


# RETURN BOOK
def return_book():
    print("\n--- RETURN BOOK ---")

    member_name = input("Enter Member Name: ").strip()
    book_title = input("Enter Book Title: ").strip()

    for loan in loans:

        if (
            loan["member"].lower() == member_name.lower()
            and
            loan["book"].lower() == book_title.lower()
        ):

            loans.remove(loan)

            for book in books:
                if book["title"].lower() == book_title.lower():
                    book["quantity"] += 1
                    break

            print("Book returned successfully!\n")
            return

    print("Loan record not found!\n")


# VIEW BOOKS
def view_books():
    print("\n--- BOOK LIST ---")

    if not books:
        print("No books available.\n")
        return

    for i, book in enumerate(books, start=1):
        print(f"{i}. {book['title']} | {book['author']} | Quantity: {book['quantity']}")

    print()


# VIEW MEMBERS
def view_members():
    print("\n--- MEMBER LIST ---")

    if not members:
        print("No members registered.\n")
        return

    for i, member in enumerate(members, start=1):
        print(f"{i}. {member['name']} | ID: {member['id']}")

    print()


# VIEW LOANS
def view_loans():
    print("\n--- LOAN RECORDS ---")

    if not loans:
        print("No loan records.\n")
        return

    for i, loan in enumerate(loans, start=1):
        print(f"{i}. {loan['member']} borrowed '{loan['book']}'")

    print()


# MAIN MENU
while True:

    print("====== LIBRARY MANAGEMENT SYSTEM ======")
    print("1. Add Book")
    print("2. Register Member")
    print("3. Borrow Book")
    print("4. Return Book")
    print("5. View Books")
    print("6. View Members")
    print("7. View Loans")
    print("8. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        add_book()

    elif choice == "2":
        register_member()

    elif choice == "3":
        borrow_book()

    elif choice == "4":
        return_book()

    elif choice == "5":
        view_books()

    elif choice == "6":
        view_members()

    elif choice == "7":
        view_loans()

    elif choice == "8":
        print("Thank you for using Library Management System!")
        break

    else:
        print("Invalid choice!\n")