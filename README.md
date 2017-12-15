# cards
The purpose of this project was to help me understand the concept of object oriented programming and to attempt something I found difficult as a beginner of Python.  Innitially, the goal of the project was to create a deck of cards.  After accomplishing this the next step was to deal a hand to the user and then finally apply the rules of Poker to the program.  This iteration of the program is still somewhat unpolished and could be written a little more efficiently.  I'm am editing it as I attempt new projects and learn new techniques. 
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
I started the program with a class called Card that took only a suit as an argument (i.e hearts, spades, diamonds, clubs).  This class has a function get_number() that assigns the variable self.number by calling a different function assign_value() and concatenates the strings so we are left with a card in the format of 'number of suit'.  This function just created a card.  Now you may notice that the function assign_value does not exist.  We create this function below in a child class called Deck.
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
The child class Deck inherites the Card class's arguments and establishes the functions assign_value() and create_deck().  assign_value() randomly generates numbers 
