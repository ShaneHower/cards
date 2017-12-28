import random
from random import randint
from collections import *

class OrderedCounter(Counter, OrderedDict):
    pass

class Card:
    def __init__(self, number, suit):
        self.number = number
        self.suit = suit
    def __str__(self):
        return str(self.number) + ' of ' + str(self.suit)
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

class PlayerDiscard:
    def __init__(self):
        self.new_hand = PlayerHand().get_hand()
        print(self.new_hand)
        number_of_discard = int(input('how many cards would you like to discard? '))
        count = 1
        if number_of_discard == 5:
            #self.new_hand = ['6 of clubs', '2 of clubs', '3 of clubs', '4 of clubs', '5 of clubs']
            self.new_hand = PlayerHand().get_hand()
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

class SplitHand():
    def __init__(self):
        self.player_hand = PlayerDiscard().get_final_hand()
        hand = ' '.join(self.player_hand)
        hand = hand.split('of')
        hand = ' '.join(hand)
        self.hand = hand.split()


    def get_split_hand(self):
        return self.hand


class Game:
    def __init__(self):
        hand = SplitHand().get_split_hand()
        hand_numbers = hand[0::2]
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
        # is the number of cards in hand minus 1(so this would be 4 if we had a 5 card draw). the first statment before 'and' checks the difference
        # the second statment checks for any duplicates and if none come up it returns true.
        if max(read_straight) - min(read_straight) == (4) and len(hand_numbers) == 5 and not any(read_straight.count(x) > 1 for x in read_straight):
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


Game()
