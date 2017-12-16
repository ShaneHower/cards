# cards
The purpose of this project was to help me understand the concept of object oriented programming and to attempt something I found difficult as a beginner of Python.  Initially, the goal of the project was to create a deck of cards.  After accomplishing this the next step was to deal a hand to the user and then finally apply the rules of Poker to the program.  This iteration of the program is still somewhat unpolished and could be written a little more efficiently.  I am editing it as I attempt new projects and learn new techniques.  
## Creating the Deck
```
class Card:
    def __init__(self, suit):
        self.suit = suit
        self.number = 0

    def get_number(self):
        self.number = self.assign_value()
        new_card = str(self.number) + ' of ' + str(self.suit)
        return new_card
 
 ```
I started the program with a class called Card that took only a suit as an argument (i.e hearts, spades, diamonds, clubs).  This class has a function get_number() that assigns the variable self.number by calling a different function assign_value(). get_number will concatenate the strings self.number, 'of', and self.suit so we are left with a card in this format: 'number of suit'.  Now you may notice that the function assign_value does not exist.  We create this function below in a child class called Deck.
```
class Deck(Card):
    def __init__(self, suit):
        Card.__init__(self, suit)

    def assign_value(self):
        new_card = randint(1, 13)
        if new_card == 1:
            new_card = 'ace'
        ....
        return new_card

    def create_deck(self):
        number_of_suit = []
        new_card = self.get_number()
        while len(number_of_suit) <= 12:
            if new_card in deck_of_cards:
                new_card = self.get_number()
            else:
                deck_of_cards.append(new_card)
                number_of_suit.append(str(self.suit))
```
The child class Deck inherited the Card class's arguments and establishes the functions assign_value() and create_deck().   assign_value() randomly generates numbers from 1 to 13 if the number is 1, 11, 12, or 13 we reassign those numbers as ace, jack, queen and king respectively (I omitted the other conversions from the code after ace for formatting purposes).  This is where the function get_number() is obtaining its numerical value.
```
deck_of_cards = []

class Card:
```
The next challenge was generating the deck of cards.  This was handle with the create_deck() function in the Deck class.  What this function does is creates an empty list that will count the number of occurrences of a suit in the deck and pulls our generated card from the get_number() function.  The number_of_suit list is very important because this will be our counter to tell the program to stop generating cards of this suit.  We use the length of this list as a condition for the while loop we are about to implement.  the while loop checks to see if the new_card is in the deck or not.  If it is in the deck it reassigns the value of the card and checks again for any duplicates of the value.  If it isn't in the deck it appends the new card to the list deck_of_cards.  We created this list outside of our first function class Card because we will be using this list later in other classes.    
