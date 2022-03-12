import cards
#import modules 
 
from cards import Card 
from cards import Deck 
from itertools import zip_longest, chain, combinations


def distribute_cards (deck, p1_cards, p2_cards, t_cards, round1):

    if round1 == True:
        for i in range (4):
            p1_cards.append (deck.deal ())#append the cards to the deck by using the deal method 
        for i in range (4):
            p2_cards.append (deck.deal ()) 
        for i in range (4):
            t_cards.append (deck.deal ()) 
    else:
        for i in range (4):
            p1_cards.append (deck.deal ()) 
        for i in range (4):
            p2_cards.append (deck.deal ()) 
	  
def get_card_index (card, cards_list):
#Iterate through the user's hands to see if that list contains the card being played
    x = 0 
    for i in cards_list: 
        x += 1 
        if card.__eq__(i): 
            return x-1 #return the index value of the list  
    return None #return nothing if no cards are found 

def get_matching_cards(card,t_cards):
#Iterate through the table cards to see if the card being played matches any cards on the table
    matching_cards = [] 
    for i in t_cards: 
        if card.equal_value(i): 
            matching_cards.append(i) #if it is append it to a list 
    return matching_cards 
 
def numeric_card(card): 
#Use the method of rank to get the numberical value of the card.
#If it is a face card (king, queen, jack) then return False and if it is a  numeric card or ace return True
    rank = card.rank() 
    if rank <= 10: 
        return True 
    else: 
        return False 
 
def remove_cards(cards_list,cards): 
#Remove the cards from the user 's hand or the table. Also a special edge case for clearing the 
#table when the round ends
    if cards_list == cards:
        #edge case 
        cards_list.clear () 
    for i in cards: 
        try: 
            cards_list.remove (i) 
    except ValueError: 
        continue 

def get_sum_matching_cards (card, t_cards):
#this function return a list of cards that add up to card rank, 
#if the card is Jack, Queen or king, the function returns and empty list
    matching_sum_list=[] 
    numeric_list=[] 
    # make a list of the numeric cards on the table 
    if len(t_cards)>1: 
        for i in t_cards: 
            if numeric_card(i): 
                numeric_list.append(i) 
    # collect pairs of numeric cards that sum to card 
    if len(numeric_list) > 1: 
    # collect combinations of length 2, i.e. pairs, of cards 
    # only if the ranks of the pair sum to card' s rank 
        matching_sum_pair =[seq for seq in combinations (numeric_list, 2) \ if seq[0].rank () + seq[1].rank () == card.rank ()]
        #combine the list of lists into one list 
        matching_sum_list = list (chain (*matching_sum_pair)) 
    return matching_sum_list 
    
def sum_rank (cards):
#Add up the rank of the cards and return it' '' 
    count = 0 
    for i in cards: 
        x = i.rank () 
        count = count + x 
    return count 
 
def jack_play (card, player, pile, basra, t_cards):
'''Check if the table cards it empty, if it is then append the jack to the table 
and remove it from the players hand. If the table is not empty
then since the jack is a special card 
append all the cards to the players pile. If the table is all 
jacks then add a basra to the players score and claear the table''' 
	if not t_cards:
	    t_cards.append (card) 
        player.remove (card) 
	else:
        pile.extend (t_cards) 
        all_jacks = True 
    for i in t_cards: 
        num = i.rank () 
    if num !=11:  
        all_jacks = False 
        break   
    if all_jacks:
        basra.append (card) 
	else:
        pile.append (card) 
    player.remove (card) 
    t_cards.clear()
 

def seven_diamond_play (card, player, pile, basra, t_cards):
'''Similar to jack function in that if the table is empty 
append it to the table. 
Otherwise clear the table and if the cards rank is summed to less than ten add a 
basra to the players score.Clear the table''' 
    count = 0 
    if not t_cards:
        t_cards.append (card) 
        player.remove (card)
    else:
        pile.extend (t_cards) 
        for i in t_cards:
            x = i.rank ()
            count = count + x 
        if count <=10:
            basra.append (card) 
        else: 
            pile.append (card) 
        player.remove (card)    
        t_cards.clear()
 
def play (card, player, pile, basra, t_cards):
'''Use all of the other functions to play the game.'''
    if not t_cards:
        #if the table is empty append it to the table and out of the hand 
        t_cards.append (card) 
        player.remove (card) 
    elif card.rank () == 11:
    #if the card is a jack call jack function 
        jack_play (card, player, pile, basra, t_cards) 
    elif card.rank () == 7 and card.suit () == 2:
    #if seven of diamonds call that function 
        seven_diamond_play (card, player, pile, basra, t_cards)
    elif card.rank () > 11:
    #if the card is a face card call the matching card function 
        matched_cards = get_matching_cards (card, t_cards) 
		if not matched_cards:
        #if no cards match append card to table 
            t_cards.append (card)
		else:
            #if not iterate through that list and take the matched off table into pile 
            for i in matched_cards:
                if i in t_cards:
                    t_cards.remove (i) 
			        pile.append (i) 
            if not t_cards:
            #if it clears the table append it to the basra 
                basra.append (card)
			      else
			    : 
    pile.append (card) 
 
player.remove (card) 
 
elif card.rank () <= 10:
#if it is a numeric card call matching
			      and matching sum functions 
				matched_cards =
				get_matching_cards (card,
						    t_cards) 
				matched_sum_cards =
				get_sum_matching_cards (card,
							t_cards) 
 
if
				matched_cards
			      or matched_sum_cards:
#if either list 
				has something in it run code below 
 for i
				in matched_cards:
#iterate through lists and 
				  remove from table and append the pile 
 if i
				  in t_cards:t_cards.remove (i) 
				      pile.append (i) 
				      for x
				    in matched_sum_cards:
if
					x
				      in t_cards:
t_cards.remove (x) 
 pile.
					  append (x) 
 if
					  not
					t_cards:
#if it clears the table add a basra 
					  basra.append (card) 
					  else
					:
pile.
					    append (card)
					    
					  else
					: 
t_cards.append (card) 
 player.remove (card) 
 
 
def compute_score (p1_pile, p2_pile, basra_1, basra_2):
'' 'Compute the score by comparing the users piles and whoever 
has more gets more points. 
 Compare the basras and calculate the score with that and the 
larger pile.' '' 
 player1_score = 0 
 player2_score = 0 
					    
if len
					  (p1_pile) >= 27: 
player1_score += 30 
 
elif len (p2_pile) >= 27: 
player2_score += 30 
 
elif len (p1_pile) == len (p2_pile):
player1_score += 0 
					      player2_score += 0 
					      
for i
					    in basra_1:
if i
					      .rank () <= 10: 
player1_score += 10 
 elif i.rank () > 11: 
player1_score += 20 
 elif i.rank () == 11:
player1_score += 30 
						  
for
						  i
						in basra_2:
if i
						  .rank () <= 10: 
player2_score += 10 
 elif i.rank () > 11: 
player2_score += 20 
 elif i.rank () == 11: player2_score += 30 
 
return (player1_score, player2_score) 
 
def display_table (t_cards, p1_cards, p2_cards):
''
						      'Display the game table.'
						      '' 
 print ("\n" +
								  36 *
								  "=") 
						      print ("{:^36s}".
							     format
							     ('Player1')) 
						      print (9 * " ", end =
							     ' ') 
 for card
						    in p1_cards:
print ("{:>3s}".format (str (card)), end = ' ') 
 print () 
 print (9 * " " + " {0[0]:>3d} {0[1]:>3d} {0[2]:>3d} 
{0[3]:>3d}".format (range (4))) 
 table = zip_longest (*[iter (t_cards)] * 4, fillvalue = 0) 
 hline = "\n" + 36 * "-" 
 str_ = hline + '\n ' 
 for row
						      in table:
str_ += 9 * " " 
							  for
							  c
							in range (0, 4):
str_ +=
							    ("{:>3s}".
							     format (str
								     (row[c]))
							     \ 
if row[c] is
							     not 0
							     else
							     ' ')
							    +' ' 
							      str_ += '\n' 
							      str_ += hline + '\n ' 
 print (str_) 
 print (9 * " " + " {0[0]:>3d} {0[1]:>3d} {0[2]:>3d} 
{0[3]:>3d}".format (range (4))) 
 print (9 * " ", end = ' ') 
 for card
							    in p2_cards: 
print ("{:>3s}".format (str (card)), end = ' ') 
 print () 
 print ("{:^36s}".format ('Player2')) 
 print (36 * "=") 
 
 
def main ():
'' 'main function' '' 
 RULES = '' ' 
 Basra Card Game: 
 This game belongs to a category of card games called . 
 Each player in turn matches a card from their hand with 
one (or more) of those 
 lying face-up on the table and then takes them. 
 If the card which the player played out does not match one
of the existing cards, 
 it stays on the table.  To win, you have to collect more points.' '' 
								
print (RULES) 
 
#initalize lists 
								p1_cards =[]
#card in hands player1 
								p2_cards =[]
#card in hands player2 
								t_cards =[]
#card on the floor 
								p1_pile =[]
#for player1 
								p2_pile =[]
#for player2 
								basra_1 =[] 
								basra_2 =[] 
								current_player
								=
								"2" 
								other_player =
								"1" 
								
 
 
deck =
								Deck ()
#set the deck by calling the class 
								
								answer =
								input
								("Would you like to play? y/Y or n/N?")
								
#prompt user and enter loop 
								answer =
								answer.
								lower () 
								while answer
							      !='n':
deck.
								  shuffle
								  ()
#shuffle deck 
								  
								  rounds = 1 
								  print
								  (" ---------Start The game--------")
#prompt user 
								  for the
								  starting round 
 print ("Dealing the cards, 4 cards for each player, 4 cards
on the table") 
 
if rounds
								  == 1:
#distribute cards to table and players 
								    distribute_cards (deck, p1_cards, p2_cards, t_cards, True) 
 deck_len = 40 
 print ("Cards left: ", deck_len)
#display deck length 
								      display_table
								      (t_cards,
								       p1_cards,
								       p2_cards)
								      
 
for i
								    in range (6):
#loop for the rounds in the game 
								      
for
									i
								      in range (8):
#loop for the turns in the game 
									
if
									  current_player
									== "1":
#switch the current 
									  player every time 
 other_player = "1" 
 current_player = "2" 
 current_cards = p2_cards 
 current_basra = basra_2 current_pile = p2_pile 
									  else
									:
other_player = "2" 
 current_player = "1" 
 current_cards = p1_cards 
 current_basra = basra_1 
 current_pile = p1_pile 
 
while True
									  :
#loop to make sure user enters valid 
									    input 
 index = input ("Player {:s} turn: -> 
".format (current_player)) 
 if index
									    .isdigit ():
#if it is a digit then 
make it an integer 
 index = int (index) 
  if index < 0 or index >= 
len (current_cards)
  :
#if it is out of bounds re prompt 
    print ('Please enter a valid card 
index, 0 <= index <= {:d}'.format (len (current_cards) - 1)) 
 continue 
  else
  :
break 
  else
  :
index = index.lower () 
 if index
  == "q":
#if user enters q break 
    out of turns loop 
 break 
if index
    == "q":
#if user enters q break out of
      rounds loop 
 break 
 
card = current_cards[index]
#find the current card
     by indexing the players hand 
       

play (card, current_cards, current_pile, current_basra, t_cards)
#call
play function 
       
display_table (t_cards, p1_cards, p2_cards) 
 if index == 'q'
:
#if user enters q break out of 
  game loop 
 break 
current_pile.extend (t_cards)
#extend the pile with 
     the table cards 
 remove_cards (t_cards, t_cards)
#remove them from the 
  table 
display_table (t_cards, p1_cards, p2_cards)
  deck_len = deck_len - 8
#keep reducing deck length by 
  amount of cards dealed each round 
 if deck_len
<0:
#if deack length is less than zero
  game is over 
 break 
print ("\n------Start new round-----")
#show new 
     round is starting 
       print ("Dealing the cards, 4 cards for each player") 
  print ("Cards left: ", deck_len) 
  
distribute_cards (deck, p1_cards, p2_cards, t_cards, False) 
#distribute cards showing that it is not round one anymore 
display_table (t_cards, p1_cards, p2_cards) 
 if
  index == 'q'
:
#if q break game loop 
  break 
 
player1_score, player2_score =
    
compute_score (p1_pile, p2_pile, basra_1, basra_2)
#compute scores
    print ("player 1: ", player1_score)
#print score 
    print ("player 2: ", player2_score) 
 if player1_score
  >player2_score:
print ("\nPlayer 1 is the winner")
#compare scores 
     and print winner 
       elif player2_score >
       player1_score:
print ("\nPlayer 2 is the winner") 
  else
    :
print ("\nNo winner: equal score") 
 

## useful strings to match tests (put them in a loop to reprompt 
      on bad input) 
## print('Please enter a valid card index, 0 <= index <= 
    {
  :d}
'.format(len(p1_cards)-1)) 
## card_index = input("Player {:d} turn: -> ".format(turn)) 
# 
 deck.reset() 
 p1_cards.clear() # card in hands player1 
 p2_cards.clear() #card in hands player2 
 t_cards.clear() # card on the floor 
 p1_pile.clear() # pile for player1 
 p2_pile.clear() # pile for player2 
 basra_1.clear() # basra for player1 
 basra_2.clear() #basra for player2 
 
 print("Would you like to play? y/Y or n/N?") #reprompt 
and re enter loop  answer = input() 
 
 else: #display closing message 
 print(' Thanks for playing
  .See you again soon.') 
 if index == "q": 
 print(' Thanks for playing
    .See you again soon.') 
 
 
 
# main function, the program' s entry point 
 if __name__
    == "__main__":
main ()
