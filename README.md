
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <ctime>
#include <cstdlib>
#include <ncurses.h>

using namespace std;

struct Question {
    string questionText;
    string answer;
    int difficultyLevel; // 1 for easy, 2 for medium, 3 for hard
};

void addQuestion(vector<Question>& questions, const string& qText, const string& ans, int difficulty) {
    Question q;
    q.questionText = qText;
    q.answer = ans;
    q.difficultyLevel = difficulty;
    questions.push_back(q);
}

void displayQuestion(WINDOW* win, const string& qText) {
    wprintw(win, "%s\n", qText.c_str());
    wrefresh(win);
}

int main() {
    srand(static_cast<unsigned int>(time(0)));

    // Initialize ncurses
    initscr();
    cbreak();
    noecho();
    keypad(stdscr, TRUE);

    WINDOW* win = newwin(20, 80, 0, 0);
    box(win, 0, 0);
    refresh();
    wrefresh(win);

    vector<Question> questions;

    // Add questions here...

    // Main loop
    int ch;
    do {
        // Clear window
        wclear(win);
        box(win, 0, 0);

        // Display question
        int index = rand() % questions.size();
        displayQuestion(win, questions[index].questionText);

        // Wait for user input
        ch = getch();

        // Display answer if user presses 'a'
        if (ch == 'a') {
            wprintw(win, "Answer: %s\n", questions[index].answer.c_str());
        }

        // Refresh window
        wrefresh(win);

    } while (ch != 'q'); // Press 'q' to quit

    // Cleanup
    endwin();

    return 0;
}
