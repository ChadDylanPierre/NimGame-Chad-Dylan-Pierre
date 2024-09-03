// NimGame.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include <iostream>
#include <iomanip>
#include <random>
#include <array>
#include <ctime>
#include <string>
using namespace std;

// Global variables

int choice, tokens, tokens_left;



// Function to initialize piles with random number of tokens

void Init_piles(array<int, 5>& piles, int tokens)
{
    // Initialize each pile with 1 token

    for (int i = 0; i < 5; i++)
    {
        piles[i] = 1;
    }

    // Seed for random number generation

    int seed1 = time(0);

    default_random_engine generator(seed1);

    uniform_int_distribution<int> rand(0, 4);

    // Distribute remaining tokens randomly among piles

    for (int i = (tokens - 5); i > 0; i--)
    {
        int index_pile = rand(generator);

        piles[index_pile]++;

    }

}
// Function to draw the piles

void Draw_piles(array<int, 5> piles)
{
    for (int i = 0; i < 5; i++)
    {
        // Print the pile label

        if (i == 0)
            cout << "A : ";

        else if (i == 1)

            cout << "B : ";

        else if (i == 2)

            cout << "C : ";

        else if (i == 3)

            cout << "D : ";

        else if (i == 4)

            cout << "E : ";

        // Print the number of tokens in the pile

        for (int j = 1; j <= piles[i]; j++)

        {
            cout << "O"; // Representing tokens with 'O'
        }

        cout << endl;
    }
}
// Function to check if the game has ended

bool EndOfGame(array<int, 5>piles)

{
    // Check if all piles are empty

    int pile1 = piles[0];

    int pile2 = piles[1];

    int pile3 = piles[2];

    int pile4 = piles[3];

    int pile5 = piles[4];

    if (pile1 == 0 && pile2 == 0 && pile3 == 0 && pile4 == 0 && pile5 == 0)

        return false; // Game ended

    else

        return true; // Game still ongoing

}

// Function for player vs player mode

void playerVplayer()
{
    string plyr1;

    string plyr2;

    // Input player names

    cout << "Now enter your names (with a space in between): ";

    cin >> plyr1 >> plyr2;

    // Input number of tokens

    cout << "Now choose a number of tokens (at least 5): ";

    cin >> tokens;

    array<int, 5> piles = { 1, 2, 3, 4, 5 };

    if (tokens < 5)
    {
        cout << "Please enter a number of tokens greater than or equal to 5: " << endl;

        cin >> tokens;
    }

    // Initialize piles

    Init_piles(piles, tokens);

    // Draw initial piles

    Draw_piles(piles);

    bool turn = false;

    // Main game loop

    while (EndOfGame(piles))

    {

        // Switch turns between players

        if (turn)

            cout << "Turn of: " << plyr1 << endl;

        else

            cout << "Turn of: " << plyr2 << endl;

        char row;

        int num = 0;

        // Prompt user for pile and number of tokens to remove

        cout << "Choose which pile and if you want to remove 1 or 2 tokens (nothing more and nothing less, ex B2): ";

        cin >> row >> num;

        int Heap;

        // Validate number of tokens input

        while (num != 1 && num != 2)
        {
            cout << "Invalid number of tokens. Please choose 1 or 2: ";
            cin >> num;
        }

        // Update the chosen pile

        switch (row)
        {
        case 'A':

            Heap = 0;

            if (piles[Heap] < num)

                cout << "Invalid move. Try again." << endl;

            else
            {
                piles[Heap] -= num;

                turn = !turn; // Only switch turn if move was valid
            }
            break;

        case 'B':

            Heap = 1;

            if (piles[Heap] < num)

                cout << "Invalid move. Try again." << endl;

            else
            {
                piles[Heap] -= num;

                turn = !turn; // Only switch turn if move was valid
            }

            break;

        case 'C':

            Heap = 2;

            if (piles[Heap] < num)

                cout << "Invalid move. Try again." << endl;

            else
            {
                piles[Heap] -= num;

                turn = !turn; // Only switch turn if move was valid

            }

            break;
        case 'D':

            Heap = 3;

            if (piles[Heap] < num)

                cout << "Invalid move. Try again." << endl;

            else
            {
                piles[Heap] -= num;

                turn = !turn; // Only switch turn if move was valid

            }

            break;

        case 'E':

            Heap = 4;

            if (piles[Heap] < num)

                cout << "Invalid move. Try again." << endl;

            else
            {
                piles[Heap] -= num;

                turn = !turn; // Only switch turn if move was valid

            }

            break;

        default:

            cout << "Invalid choice. Try again!" << endl;

        }
        // Draw updated piles

        Draw_piles(piles);

    }

    // Declare winner

    if (!turn)

        cout << "The winner is " << plyr1 << " !!" << endl;

    else

        cout << "The winner is " << plyr2 << " !!" << endl;

}

// All the following functions are for the PC vs user part



//This function displays all the piles and the tokens they have
void displayGame(vector<int>& pile) {
    char pile_letter = 'A';
    for (int i = 0; i < pile.size(); i++) {
        cout << pile_letter++ << ": ";
        for (int j = 0; j < pile[i]; j++) {
            cout << "O";
        }
        cout << endl;
    }
}
//This function passes through the vector at every turn to see if there is any tokens remaining and who won
bool check_pilesempty(vector<int>& pile) {
    for (int item : pile) {
        if (item > 0) return true;
    }
    return false;
}
//this function distributes the tokens according to the function given in the assignment
void distributeTokens(vector<int>& pile) {
    int j = 1;
    for (int i = 0; i < pile.size();i++) {

        pile[i] = floor(10 * exp(-(pow(((j)-5), 2) / 10.89)));
        j++;
    }

}
//Function checks if user or PC is not taking more tokens than there are in a pile and if the computer is taking tokens from the avaible indexes of the vector
bool allowed_move(const vector<int>& pile, int pileindex, int tokens) {

    if (pile[pileindex] < tokens ||  pileindex >= pile.size())
        return false;

         return true;

}
// function where the user chooses the pile and how many token he will take from it. Uses the previous function to force right move. 
void userturn(vector<int>& pile) {
    char pileletter;
    int tokens;
    int pileindex;


    cout << " Enter the  letter of the pile and the number of token you want to remove from it (ex. B2 ). ";
    while (true) {

        cin >> pileletter >> tokens;
        pileindex = pileletter - 'A';

        if (tokens == 1 || tokens == 2) {
            if (allowed_move(pile, pileindex, tokens)) {
                pile[pileindex] -= tokens;
                break;
            }
            else {
                cout << "Invalid move. Try again: ";
            }
        }
        else {
            cout << "Invalid number of tokens. Enter the pile (A-I) and the number of token you want to remove from it. ";
        }
    }
}

//function that makes the PC select a letter of a pile and the amount of tokens that will be taken at random.
void computerturn(vector<int>& pile) {

    int pileindex;
    int tokens;
    default_random_engine engine(static_cast<unsigned int>(time(0)));
    uniform_int_distribution<unsigned int> random(0, pile.size() - 1);

    uniform_int_distribution<unsigned int> random1_2(1, 2);
    do {



        pileindex = random(engine);
        tokens = random1_2(engine);

    } while (!(allowed_move(pile, pileindex, tokens)));
    pile[pileindex] -= tokens;
    cout << endl << "The computer removed " << tokens << " tokens out of pile " << char('A' + pileindex) << endl;

}




int main() {

    int choice;

    char answ;

    cout << "Welcome to the Nim game\n";
  //loops the display menu until the user doesn't want to play anymore
    while (true) {

        cout << "Choose an option among these:\n";

        cout << "1- Two players (User vs User)\n";

        cout << "2- Two players (User vs PC)\n";

        cout << "3- Exit\n";

        cin >> choice;



        if (choice == 3) {

            cout << "Bye\n";

            break;

        }
        else if (choice == 1) {

            playerVplayer();

        }

        else if (choice == 2) {
            int pilenumber;
            do {
                cout << "Choose a number between 2 and 10 that will decide the amount of piles there is.";
                cin >> pilenumber;
            } while (pilenumber > 10 || pilenumber < 2);
            //gives the of vector pile 
            vector<int> pile(pilenumber);
            distributeTokens(pile);
            displayGame(pile);

            //game goes on until no tokens left
            while (check_pilesempty(pile)) {
                cout <<endl<< "It's your turn : ";
                userturn(pile);
                displayGame(pile);
                //checks at the end of turn if user made last move
                if (!(check_pilesempty(pile))) {
                    cout << " You won!" << endl;
                    break;
                }

                computerturn(pile);
                displayGame(pile);

                if (!(check_pilesempty(pile))) {
                    cout << "The computer won,better luck next time...";
                    break;
                }

            }


        }

        cout << endl<<"Would you like to play again? (y/n): ";

        cin >> answ;

        if (answ != 'y')

            break;

    }

    cout << "Thank you!\n";

    return 0;

}




















