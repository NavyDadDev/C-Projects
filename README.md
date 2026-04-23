//Sam Smith
//Project 3 - Grocery Tracker
//CS210 Programming Languages


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

