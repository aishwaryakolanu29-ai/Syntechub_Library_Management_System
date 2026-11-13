# Library Management System

## Project Overview
The Library Management System is a Java-based
command-line application used to manage books
in a library.

## Features
- Add new books
- Remove books
- Search books using Book ID
- Display all available books
- Prevent duplicate Book IDs
- Validate user inputs
- Save book details using file handling
- Load saved book details when restarting

## Technologies Used
- Java
- ArrayList
- File Handling
- Exception Handling
- Object-Oriented Programming

## Project Structure

```text
Library Management System/
│
├── src/
│ ├── Main.java
│ ├── Book.java
│ └── Library.java
│
└── README.md
```

## How to Run
1. Download or clone this repository.
2. Open the project in Eclipse or another Java IDE.
3. Run Main.java.
4. Select the required option from the menu.

## Developed For
Syntecxhub Java Programming Internship

## Author
Your Name


On Fri, 13 Nov, 2026, 22:16 Aishwarya Kolanu, <aishwaryakolanu29@gmail.com> wrote:

import java.util.ArrayList;
import java.io.*;

public class Library {

    private ArrayList<Book> books = new ArrayList<>();

    private final String FILE_NAME = "books.txt";

    // Add a book
    public boolean addBook(Book book) {

        for (Book b : books) {
            if (b.getBookId() == book.getBookId()) {
                System.out.println("Book ID already exists!");
                return false;
            }
        }

        books.add(book);
        System.out.println("Book added successfully!");
        return true;
    }

    // Remove a book
    public boolean removeBook(int id) {

        for (Book b : books) {
            if (b.getBookId() == id) {
                books.remove(b);
                System.out.println("Book removed successfully!");
                return true;
            }
        }

        System.out.println("Book not found!");
        return false;
    }

    // Search for a book
    public void searchBook(int id) {

        for (Book b : books) {
            if (b.getBookId() == id) {
                System.out.println("Book found!");
                b.displayBook();
                return;
            }
        }

        System.out.println("Book not found!");
    }

    // Display all books
    public void displayBooks() {

        if (books.isEmpty()) {
            System.out.println("No books available!");
            return;
        }

        System.out.println("\n======= BOOK DETAILS =======");

        for (Book b : books) {
            b.displayBook();
        }
    }

    // Save books into a file
    public void saveBooks() throws IOException {

        try (BufferedWriter writer =
                new BufferedWriter(new FileWriter(FILE_NAME))) {

            for (Book b : books) {
                writer.write(b.getBookId() + "|" +
                        b.getBookName() + "|" +
                        b.getAuthorName());

                writer.newLine();
            }
        }
    }

    // Load books from a file
    public void loadBooks() throws IOException {

        File file = new File(FILE_NAME);

        if (!file.exists()) {
            return;
        }

        try (BufferedReader reader =
                new BufferedReader(new FileReader(file))) {

            String line;

            while ((line = reader.readLine()) != null) {

                String[] data = line.split("\\|", -1);

                if (data.length == 3) {
                    try {
                        int id = Integer.parseInt(data[0]);

                        if (id > 0 && !data[1].isEmpty()
                                && !data[2].isEmpty()) {

                            boolean duplicate = false;

                            for (Book b : books) {
                                if (b.getBookId() == id) {
                                    duplicate = true;
                                    break;
                                }
                            }

                            if (!duplicate) {
                                books.add(new Book(
                                        id, data[1], data[2]));
                            }
                        }

                    } catch (NumberFormatException e) {
                        System.out.println(
                                "Skipping invalid book record.");
                    }
                }
            }
        }
    }
}
