# Poker
The purpose of this project was to help me understand the concept of object oriented programming and to attempt something I found difficult as a beginner of Python. Initially, the goal of the project was to create a deck of cards. After accomplishing this the next step was to deal a hand to the user and then finally apply the rules of Poker to the program. This iteration of the program is still somewhat unpolished and could be written a little more efficiently. I am editing it as I attempt new projects and learn new techniques.

## Creating a Card
```
class Card:
    def __init__(self, number, suit):
        self.number = number
        self.suit = suit
    def __str__(self):
        return str(self.number) + ' of ' + str(self.suit)
```
creating the object card was first on the list.  This was a class that took two arguments number and suit and returned the string in the format: number of suit (i.e. 5 of hearts). 

## Creating the Deck
```
class Deck:
    def __init__(self):
        self.deck = []
        for suit in ['diamonds', 'hearts', 'clubs', 'spades']:
            for number in range(1,14):
                if number == 1:
                    number = 'ace'
                elif number == 11:
                    number = 'jack'
                elif number == 12:
                    number = 'queen'
                elif number == 13:
                    number = 'king'
                new_card = Card(number, suit)
                self.deck.append(str(new_card))
    def get_list(self):
        return self.deck
```

The deck class created an empty list, self.deck, and used two for loops to input the suit and number into the class Card.  Before inputting the number and suit into Card, I converted 1, 11, 12, and 13 into the royal values.  Once the card was created it appended the new card to self.deck.  this class uses the get_list() function to return self.deck. 

## Creating Hand
```
class PlayerHand:
    def __init__(self):
        self.hand =[]
        draw_card = random.choice(Deck().get_list())
        while len(self.hand) < 5:
            if draw_card not in self.hand:
                self.hand.append(draw_card)
            else:
                draw_card = random.choice(Deck().get_list())

    def get_hand(self):
        return self.hand
```
The PlayerHand class creates an empty list, self.hand, and takes a random card from the deck.  I then use a while loop to check if the drawn card is already in the hand.  If not, the card is appended to the hand.  If it is in the hand already, it sets the draw_card variable to another value in self.deck.

## Discarding 
```
class PlayerDiscard:
    def __init__(self):
        self.new_hand = PlayerHand().get_hand()
        print(self.new_hand)
        number_of_discard = int(input('how many cards would you like to discard? '))
        count = 1
        if number_of_discard == 5:
            self.new_hand = ['ace of clubs', '10 of spades', '10 of diamonds', 'jack of hearts', '10 of hearts']
            #self.new_hand = PlayerHand().get_hand()
        else:
            while count <= number_of_discard:
                replace_card = input('Which card would you like to discard (one at a time please)? ')
                self.new_hand.remove(replace_card)
                new_card = random.choice(Deck().get_list())
                if new_card not in self.new_hand:
                    self.new_hand.append(new_card)
                    count = count + 1
                else:
                    new_card = random.choice(Deck().get_list())
        print(self.new_hand)

    def get_final_hand(self):
        return self.new_hand
```

This class grabs the created hand from PlayerHand() and shows the hand to the user.  It then asks how many cards they would like to discard. if 5 is inputed, it creates a brand new hand.  If any other number is inputted it runs a while loop that asks which card will be discarded.  It removes the inputted name from the list and and grabs another random card from Deck() and appends it if this card is not present.  
