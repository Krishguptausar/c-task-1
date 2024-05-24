# c-task-1
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <fstream>

using namespace std;

struct Book {
    string title;
    string author;
    string ISBN;
    bool isAvailable;

    Book(string t, string a, string i, bool avail)
        : title(t), author(a), ISBN(i), isAvailable(avail) {}
};

class Library {
private:
    vector<Book> catalog;

public:
    void addBook();
    void removeBook();
    void checkAvailability();
    void displayCatalog();
    void saveCatalog(const string& filename);
    void loadCatalog(const string& filename);
};

void Library::addBook() {
    string title, author, ISBN;
    cout << "Enter book title: ";
    cin.ignore();
    getline(cin, title);
    cout << "Enter book author: ";
    getline(cin, author);
    cout << "Enter book ISBN: ";
    getline(cin, ISBN);

    catalog.push_back(Book(title, author, ISBN, true));
    cout << "Book added successfully!" << endl;
}

void Library::removeBook() {
    string ISBN;
    cout << "Enter book ISBN to remove: ";
    cin.ignore();
    getline(cin, ISBN);

    auto it = remove_if(catalog.begin(), catalog.end(), [&](Book& book) {
        return book.ISBN == ISBN;
    });

    if (it != catalog.end()) {
        catalog.erase(it, catalog.end());
        cout << "Book removed successfully!" << endl;
    } else {
        cout << "Book not found!" << endl;
    }
}

void Library::checkAvailability() {
    string ISBN;
    cout << "Enter book ISBN to check availability: ";
    cin.ignore();
    getline(cin, ISBN);

    for (const auto& book : catalog) {
        if (book.ISBN == ISBN) {
            cout << "Book: " << book.title << " by " << book.author << endl;
            cout << "Availability: " << (book.isAvailable ? "Available" : "Not Available") << endl;
            return;
        }
    }
    cout << "Book not found!" << endl;
}

void Library::displayCatalog() {
    cout << "Library Catalog:" << endl;
    for (const auto& book : catalog) {
        cout << "Title: " << book.title << ", Author: " << book.author << ", ISBN: " << book.ISBN
             << ", Availability: " << (book.isAvailable ? "Available" : "Not Available") << endl;
    }
}

void Library::saveCatalog(const string& filename) {
    ofstream outFile(filename);
    for (const auto& book : catalog) {
        outFile << book.title << "," << book.author << "," << book.ISBN << "," << book.isAvailable << endl;
    }
    outFile.close();
    cout << "Catalog saved to file!" << endl;
}

void Library::loadCatalog(const string& filename) {
    ifstream inFile(filename);
    if (!inFile) {
        cout << "Error opening file!" << endl;
        return;
    }

    catalog.clear();
    string title, author, ISBN, avail;
    while (getline(inFile, title, ',') && getline(inFile, author, ',') && getline(inFile, ISBN, ',') && getline(inFile, avail)) {
        bool isAvailable = (avail == "1");
        catalog.push_back(Book(title, author, ISBN, isAvailable));
    }
    inFile.close();
    cout << "Catalog loaded from file!" << endl;
}

int main() {
    Library library;
    char choice;
    string filename = "catalog.txt";

    library.loadCatalog(filename);

    do {
        cout << "\nLibrary Management System" << endl;
        cout << "1. Add Book" << endl;
        cout << "2. Remove Book" << endl;
        cout << "3. Check Book Availability" << endl;
        cout << "4. Display Catalog" << endl;
        cout << "5. Save Catalog" << endl;
        cout << "6. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case '1':
                library.addBook();
                break;
            case '2':
                library.removeBook();
                break;
            case '3':
                library.checkAvailability();
                break;
            case '4':
                library.displayCatalog();
                break;
            case '5':
                library.saveCatalog(filename);
                break;
            case '6':
                library.saveCatalog(filename);
                cout << "Exiting..." << endl;
                break;
            default:
                cout << "Invalid choice! Please try again." << endl;
                break;
        }
    } while (choice != '6');

    return 0;
}
