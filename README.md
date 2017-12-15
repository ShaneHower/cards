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
