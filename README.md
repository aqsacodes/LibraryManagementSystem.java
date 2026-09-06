# LibraryManagementSystem.java
A Java console app that helps students find answers to university queries including admissions, fees, results, hostel, and exams. Features keyword search and a clean menu interface. Built as part of BS Computer Science coursework at QUEST Nawabshah.

import java.util.ArrayList;
import java.util.Scanner;

// Book class
class Book {
    private String title;
    private String author;
    private String isbn;
    private boolean isAvailable;

    public Book(String title, String author, String isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.isAvailable = true;
    }

    public String getTitle() { return title; }
    public String getAuthor() { return author; }
    public String getIsbn() { return isbn; }
    public boolean isAvailable() { return isAvailable; }
    public void setAvailable(boolean available) { isAvailable = available; }

    @Override
    public String toString() {
        return "Title: " + title + " | Author: " + author +
               " | ISBN: " + isbn + " | Status: " + (isAvailable ? "Available" : "Borrowed");
    }
}

// User class
class User {
    private String name;
    private String userId;
    private ArrayList<Book> borrowedBooks;

    public User(String name, String userId) {
        this.name = name;
        this.userId = userId;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getName() { return name; }
    public String getUserId() { return userId; }
    public ArrayList<Book> getBorrowedBooks() { return borrowedBooks; }

    public void borrowBook(Book book) {
        borrowedBooks.add(book);
    }

    public void returnBook(Book book) {
        borrowedBooks.remove(book);
    }
}

// Library class
class Library {
    private ArrayList<Book> books;
    private ArrayList<User> users;

    public Library() {
        books = new ArrayList<>();
        users = new ArrayList<>();
    }

    public void addBook(Book book) {
        books.add(book);
        System.out.println("Book added: " + book.getTitle());
    }

    public void addUser(User user) {
        users.add(user);
        System.out.println("User registered: " + user.getName());
    }

    public Book findBook(String isbn) {
        for (Book book : books) {
            if (book.getIsbn().equals(isbn)) return book;
        }
        return null;
    }

    public User findUser(String userId) {
        for (User user : users) {
            if (user.getUserId().equals(userId)) return user;
        }
        return null;
    }

    public void borrowBook(String userId, String isbn) {
        User user = findUser(userId);
        Book book = findBook(isbn);

        if (user == null) {
            System.out.println("User not found!");
            return;
        }
        if (book == null) {
            System.out.println("Book not found!");
            return;
        }
        if (!book.isAvailable()) {
            System.out.println("Sorry, this book is already borrowed.");
            return;
        }

        book.setAvailable(false);
        user.borrowBook(book);
        System.out.println(user.getName() + " borrowed: " + book.getTitle());
    }

    public void returnBook(String userId, String isbn) {
        User user = findUser(userId);
        Book book = findBook(isbn);

        if (user == null || book == null) {
            System.out.println("Invalid user or book.");
            return;
        }

        book.setAvailable(true);
        user.returnBook(book);
        System.out.println(user.getName() + " returned: " + book.getTitle());
    }

    public void displayAllBooks() {
        System.out.println("\n===== ALL BOOKS =====");
        if (books.isEmpty()) {
            System.out.println("No books in library.");
            return;
        }
        for (Book book : books) {
            System.out.println(book);
        }
    }

    public void displayUserBooks(String userId) {
        User user = findUser(userId);
        if (user == null) {
            System.out.println("User not found!");
            return;
        }
        System.out.println("\n===== BOOKS BORROWED BY " + user.getName() + " =====");
        if (user.getBorrowedBooks().isEmpty()) {
            System.out.println("No books currently borrowed.");
            return;
        }
        for (Book book : user.getBorrowedBooks()) {
            System.out.println(book);
        }
    }
}

// Main class
public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        Scanner scanner = new Scanner(System.in);

        // Sample data
        library.addBook(new Book("Java Programming", "James Gosling", "ISBN001"));
        library.addBook(new Book("Data Structures", "Mark Allen", "ISBN002"));
        library.addBook(new Book("OOP Concepts", "Grady Booch", "ISBN003"));

        library.addUser(new User("Aqsa Waheed", "U001"));
        library.addUser(new User("Ali Ahmed", "U002"));

        int choice;
        do {
            System.out.println("\n===== LIBRARY MANAGEMENT SYSTEM =====");
            System.out.println("1. Display All Books");
            System.out.println("2. Borrow a Book");
            System.out.println("3. Return a Book");
            System.out.println("4. View My Borrowed Books");
            System.out.println("0. Exit");
            System.out.print("Enter choice: ");
            choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1:
                    library.displayAllBooks();
                    break;
                case 2:
                    System.out.print("Enter your User ID: ");
                    String uid = scanner.nextLine();
                    System.out.print("Enter Book ISBN: ");
                    String isbn = scanner.nextLine();
                    library.borrowBook(uid, isbn);
                    break;
                case 3:
                    System.out.print("Enter your User ID: ");
                    String uid2 = scanner.nextLine();
                    System.out.print("Enter Book ISBN: ");
                    String isbn2 = scanner.nextLine();
                    library.returnBook(uid2, isbn2);
                    break;
                case 4:
                    System.out.print("Enter your User ID: ");
                    String uid3 = scanner.nextLine();
                    library.displayUserBooks(uid3);
                    break;
                case 0:
                    System.out.println("Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice!");
            }
        } while (choice != 0);

        scanner.close();
    }
}

