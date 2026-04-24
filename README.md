//Sam Smith
//Project 3 - Grocery Tracker
//CS210 Programming Languages

// Summarize the project and what problem it was solving?
   This project had many different moving parts and included a text file that needed to be read and return data as part of the output. 
   No calculations were needed, but understanding how to code new logic. I can see many application where this type of coding could be used 
   in Data collection and analysis. 
   
   What did you do particularly well?
   I felt I did a good job in seperating the program into logical sections that allow for readability and logical format.This program allowed me 
   focus on clean design and logical output. 
   
   Where could you enhance your code? How would these improvements make your code more efficient, secure, and so on?
   I found through this course that my logic in error handling and writing logic to handle such errors. Overall, I think that outside of this class, I need to continue to 
   challenge myself in writing code in area where I feel I need to develop clear thought. 
   
   Which pieces of the code did you find most challenging to write, and how did you overcome this? What tools or resources are you adding to your support network?
   In each piece of coding thoughout this course, debugging errors and rethinking my logic as I code. At times, I had to rethink my process as I attemped to write the code which helped.
   
   What skills from this project will be particularly transferable to other projects or course work?
   I, personally, was at a crossroads I was unsure about my ability early in Computer Science. This course has taught me that coding is about the process of learning and experience and 
   understanding the logic of coding. Planning and preperation was a big learning focus for me. 
   
   How did you make this program maintainable, readable, and adaptable?
   I wanted to make sure that I followed the correct naming convention and worked to ensure that I was clearly commenting the sections of the code
   and that allowed me to think through this coding challenge. 
   

#include <iostream>
#include <fstream>
#include <string>
#include <map>
#include <limits>

class GroceryTracker {
private:
    std::map<std::string, int> itemFrequency;
    std::string inputFileName;
    std::string backupFileName;

public:
    // Constructor
    GroceryTracker(const std::string& input, const std::string& backup)
        : inputFileName(input), backupFileName(backup) {
    }

    // Load items from input file
    void LoadData() {
        std::ifstream inFile(inputFileName);
        if (!inFile.is_open()) {
            std::cout << "Error: Could not open input file: "
                << inputFileName << std::endl;
            return;
        }

        std::string item;
        while (inFile >> item) {
            // Increment count for each item
            itemFrequency[item]++;
        }

        inFile.close();
    }

    // Write frequency map to backup file
    void WriteBackup() const {
        std::ofstream outFile(backupFileName);
        if (!outFile.is_open()) {
            std::cout << "Error: Could not create backup file: "
                << backupFileName << std::endl;
            return;
        }

        for (const auto& pair : itemFrequency) {
            outFile << pair.first << " " << pair.second << std::endl;
        }

        outFile.close();
    }

    // Return frequency for a specific item
    int GetItemFrequency(const std::string& item) const {
        auto it = itemFrequency.find(item);
        if (it != itemFrequency.end()) {
            return it->second;
        }
        return 0;
    }

    // Print all items and their frequencies
    void PrintAllFrequencies() const {
        for (const auto& pair : itemFrequency) {
            std::cout << pair.first << " " << pair.second << std::endl;
        }
    }

    // Print histogram using asterisks
    void PrintHistogram() const {
        for (const auto& pair : itemFrequency) {
            std::cout << pair.first << " ";
            for (int i = 0; i < pair.second; ++i) {
                std::cout << "*";
            }
            std::cout << std::endl;
        }
    }
};

// Simple input validation for menu choice
int GetMenuChoice() {
    int choice;
    while (true) {
        std::cout << "\nMenu Options:\n";
        std::cout << "1. Look up item frequency\n";
        std::cout << "2. Print item frequency list\n";
        std::cout << "3. Print item frequency histogram\n";
        std::cout << "4. Exit\n";
        std::cout << "Enter your choice (1-4): ";

        if (std::cin >> choice && choice >= 1 && choice <= 4) {
            return choice;
        }

        std::cout << "Invalid input. Please enter a number between 1 and 4.\n";
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    }
}

int main() {
    // File names per project description
    const std::string inputFile = "Project-Three.txt";
    const std::string backupFile = "frequency.dat";

    GroceryTracker tracker(inputFile, backupFile);

    // Load data and create backup at program start
    tracker.LoadData();
    tracker.WriteBackup();

    bool running = true;
    while (running) {
        int choice = GetMenuChoice();

        switch (choice) {
        case 1: {
            std::cout << "Enter the item to look up: ";
            std::string item;
            std::cin >> item;  
            int freq = tracker.GetItemFrequency(item);
            std::cout << item << " appears " << freq << " time(s)." << std::endl;
            break;
        }
        case 2:
            tracker.PrintAllFrequencies();
            break;
        case 3:
            tracker.PrintHistogram();
            break;
        case 4:
            running = false;
            break;
        default:
            // Should never hit this because of validation
            break;
        }
    }

    std::cout << "Goodbye.\n";
    return 0;
}

