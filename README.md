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
Creating the object card was first on the list.  This was a class that took two arguments number and suit and returned the string in the format: number of suit (i.e. 5 of hearts). 

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

## Preparing the Hand for Interpretation
```
class SplitHand():
    def __init__(self):
        self.player_hand = PlayerDiscard().get_final_hand()
        hand = ' '.join(self.player_hand)
        hand = hand.split('of')
        hand = ' '.join(hand)
        self.hand = hand.split()


    def get_split_hand(self):
        return self.hand
```
This class's purpose is to split the array so I no longer have cards like '5 of hearts', but instead I have a list of numbers and suits which are separate from one another.  This step is important because in order to interpret my hand I need to be able to isolate specific words in the list.  I needed to join the list into a string, then split it back into a list while taking out the 'of' values.  I then rejoined the list and split it once more so that every word was seperated. 

## Interpreting the Hand
```
class Game:
    def __init__(self):
        hand = SplitHand().get_split_hand()
        print(hand)
        hand_numbers = hand[::2]
        hand_suit = hand[1::2]
        number_count = OrderedCounter(hand_numbers)
        suit_count = OrderedCounter(hand_suit)
        hand_reveal = []
        read_straight = []

        #multiple occurrence hands
        for i in hand_numbers:
            if number_count[i] == 2:
                hand_reveal.append('2 of a kind!')
                hand_numbers.remove(i)
                number_count = OrderedCounter(hand_numbers)
            # below might be an issue for 3 of a kind it'll append 3 of a kind twice to hand_reveal
            elif number_count[i] == 3:
                hand_reveal.append('3 of a kind!')
                hand_numbers.remove(i)
            elif number_count[i] == 4:
                hand_reveal.append('4 of a kind!')
                break
        #Flush
        for i in hand_suit:
            if suit_count[i] == 5:
                hand_reveal.append('Flush!')
                break

        #straight
        for i in hand_numbers:
            if i == 'jack':
                read_straight.append(11)
            elif i == 'queen':
                read_straight.append(12)
            elif i == 'king':
                read_straight.append(13)
            elif i == 'ace':
                read_straight.append(14)
            else:
                read_straight.append(int(i))

        # accounts for the aces ability to be a 1 or 14 depending on its position in a straight
        for i in read_straight:
            if i == 14:
                if min(read_straight) == 2:
                    read_straight.remove(14)
                    read_straight.append(1)

        # if there are no duplicates, the list would have to be conesecutive if the difference between the highest and lowest number
        # is the number of cards in hand minus 1(so this would be 4 if we had a 5 card draw). the first statment before 'and' 
        # checks the difference
        # the second statment checks for any duplicates and if none come up it returns true.
        if max(read_straight) - min(read_straight) == (4) and (not any(read_straight.count(x) > 1 for x in read_straight)):
            if min(read_straight) == 10 and max(read_straight) == 14:
                hand_reveal.append('Royal')
            else:
                hand_reveal.append('Straight!')
        
        #read the combination hands (i.e. royal flush, full house, ...)
        if len(hand_reveal) > 2:
            if (hand_reveal[0] == '2 of a kind!' and hand_reveal[1] == '3 of a kind!' and hand_reveal[2] == '3 of a kind!') or (
                    hand_reveal[0] == '3 of a kind!' and hand_reveal[1] == '3 of a kind!' and hand_reveal[2] == '2 of a kind!'):
                print('Full House!')

        elif len(hand_reveal)>1:
            if hand_reveal[0] == '2 of a kind!' and hand_reveal[1] == '2 of a kind!':
                print('2 Pair!')
            elif (hand_reveal[0] == 'Flush!' and hand_reveal[1] == 'Straight!') or (
                    hand_reveal[0] == 'Straight!' and hand_reveal[1] == 'Flush!'):
                print('Straight Flush!')
            elif (hand_reveal[0] == 'Flush!' and hand_reveal[1] == 'Royal') or (
                    hand_reveal[0] == 'Royal' and hand_reveal[1] == 'Flush!'):
                print('Royal Flush!')
            #this is how i resolved the duplicates of '3 of a kind!'
            elif hand_reveal[0] == '3 of a kind!' and hand_reveal[1] == '3 of a kind!':
                print('3 of a kind!')

        elif len(hand_reveal) == 1:
            if hand_reveal[0] == 'Royal':
                print('Straight!')
            else:
                print(''.join(hand_reveal))

        else:
            print('you have nothing')

        print(hand_reveal)
 ```
