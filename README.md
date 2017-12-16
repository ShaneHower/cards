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
I started the program with a class called Card that took only a suit as an argument (i.e hearts, spades, diamonds, clubs).  This class has a function get_number() that assigns the variable self.number by calling a different function assign_value(). get_number() will concatenate the strings self.number, 'of', and self.suit so we are left with a card in this format: 'number of suit'.  Now you may notice that the function assign_value does not exist.  We create this function below in a child class called Deck.
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
The next challenge was generating the deck of cards.  This was handled through the create_deck() function in the Deck class.  What this function does is creates an empty list for the purpose of counting the number of occurrences of a suit. It also pulls our generated card from the get_number() function.  The number_of_suit list is very important because this will be our counter to tell the program to stop generating cards once the number of suits exceeds 13.  We use the length of this list as a condition for the while loop we are about to implement.  the while loop checks to see if the new_card is in the deck.  If it is in the deck, it reassigns the value of the card and checks again for any duplicates of the value.  If it isn't in the deck it appends the new card to the list deck_of_cards.  We created this list outside of our first function class Card because we will be using this list later in other classes.    

## Dealing a Hand to the User
```
class PlayerHand:
    def __init__(self, card_num):
        self.card_num = card_num

    def get_hand(self):
        hand = []
        draw_card = random.choice(deck_of_cards)
        while len(hand) < int(self.card_num):
            if draw_card not in hand:
                hand.append(draw_card)
                #takes cards out of deck so they cannote be repicked during the next draw phase.
                deck_of_cards.remove(draw_card)
            else:
                draw_card = random.choice(deck_of_cards)
        return hand
```
We create a new class called PlayerHand that takes a number as it's argument.  The number signifies how many cards the user will be dealt.  In PlayerHand we establish a class called get_hand() which creates an empty list (which is the user's hand) and assigns a random element from deck_of_cards to the variable draw_card.  While the player's hand is less than the number of cards we will deal to them, we append draw_card to the empty list hand and remove the value from deck_of_cards (removing the value will become important later).
## Discarding
```
class Replace(PlayerHand):
    def __init__(self, card_num):
        PlayerHand.__init__(self, card_num)

    def replace_cards(self):
        x = 1
        hand = self.get_hand()
        print(hand)
        number_cards = int(input('how many cards would you like to discard?:'))
        if number_cards == self.card_num:
            #hand = ['3 of spades', '3 of clubs', '3 of hearts', '2 of diamonds', '2 of spades']
            hand = self.get_hand()
        else:
            while x <= number_cards:
                replace_card = input('which card would you like to replace (one at a time please)?')
                for i in hand:
                    if replace_card == i:
                        hand.remove(i)
                        hand.append(random.choice(deck_of_cards))
                x = x + 1
        print(hand)
```
In Poker, an important aspect of the game is being able to discard unwanted cards.  To add this aspect of the game into our program we create yet another class called Replace which is a child class of PlayerHand.  In Replace we define a function called replace_cards() which asks the user how many cards they would like to discard from their hand.  We store this integer in the variable number_cards.  If they wish to discard their entire hand we call the function get_hand() once again and replace all the cards.  Otherwise we create another while loop that tracks the number of times we've entered a card value.  Inside the loop we remove the unwanted card from hand and append a new value from the deck.  This is why we removed the cards from the deck in the get_hand() function.  It ensures we will not pick a card that has already been dealt.  Once the number of entries is equal to the number the user inputted into number_cards variable the loop ends. 


