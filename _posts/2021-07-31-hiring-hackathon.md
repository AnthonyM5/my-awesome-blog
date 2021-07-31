---
layout: post
title: "Hiring Hack-a-thon Project"
---

### Hackathons
As part of the journey to learn (and if you're looking for a job), networking is an invaluable activity to help discover new communities, friends, and even job opportunities. One such networking event that is always popular is a hack-a-thon which gathers groups of developers, designers, and engineers with a sprint-like design with an end goal of creating functional software at the end. [Mintbean][1] is a community that often hosts events like their current [Hiring Hackathon][2] for Junior Developers - developers will attempt to complete one of 3 challenges (Front-end, Back-end, or FullStack) to have their names/projects submitted to hiring managers for interviews. The object for this event is to create a card game with rules and paramters outlined further in the resources tab.
#### BlackJack Card Game
For this project I will opt to design a front-end application which requires for us to render the complete UI/UX for the game that can be run locally or deployed on a cloud platform. 
The card game I will be attempting to build is a simple BlackJack (21) variant. I will attempt to follow the general rules outlined by [Bicycle Cards][3], but possibly forgo a betting system and just start with coding the dealer/player logic. 

For this design we will likely need the following:
- Simulate 52 card deck (or multiple decks)
- Designate Players, and Dealer
- Track card values, identifying BlackJack hands
- Dealer Logic => Dealers must hit if their total is <= 16, stand if >= 17

With this framework we will likely go with React.js for the front-end so that we can use components to represent the cards, the players, the dealer, and use local state to keep track of card values + score. This will also give us room to incorporate a betting system as well. 
#### External APIs and Design
The parameters for the game allow for us to utilize external APIs so in order to be efficient we can rely on an API to generate our deck of cards. [Deck of Cards API][4] is a cool API that gives us all the functionality we need - it'll provide for us the abiltiy to recieve as a JSON response a status of a 52 card deck (or multiple decks), and draw functionality with card value, card image, and suit as a response. This robust API essentially handles all of our card simulations down to the images. It has the ability for us to create piles/discards if we want to keep track of those but that may not be necessary. 

From there all we would need to do is design a table background with the ability for players to "sit" and be dealt in. We would also need some buttons for designating when a player wants to hit/stand, (and possibly double down if we want to incorporate the betting system). The values returned from the API will be crucial to the next step of outlining the logic.

#### Logic Requirements
Now that we have the Deck of Cards API out of the way, we can outline how to replicate the rules for BlackJack. First we should have to keep an array of players and dealers so that we can deal a single card to each player on the table individually until all seated players are dealt 2 cards. We can worry about the face up/face down nature of the cards at a later time. We will have to define turns for player/dealer and it may be easier to start with just one player vs the dealer. 

One consideration we will always have to keep in mind is the changing value of the Ace to be 1 or 11. We will have to have a conditional check to see whether the total is above 21 in which case the Ace will automatically count as one. 

Next would be to be able to track the card values, with a note to track BlackJack hands that are automatically winning. The logic for this is the same for dealer or player so we might want to be able to have a default function that checks for BlackJacks - this function will have to have a slightly different effect for the Dealer as they would automatically win (unless players also have BlackJack). 

As part of the parent component we will have to have definitive turns, once all the cards are dealt and before the first turn we will check for BlackJacks, and then initiate the player's first turn. The turn will give the player the option to hit/stay, once this is completed we can traverse the array to the next player or the dealer (dealer being last). 

The dealer logic will automate their turns based on the 17 rule, they will always draw a card if their total is <=16, and they will always stay anytime their hand value is >= 17.
Once the array is traversed (the dealer has made their move) we can have a function to determine the winner, essentially comparing the component states' card values and returning the component that has the highest score as the winner. 

#### Additional Considerations
- Whether or not the application will be made into a full-stack with a back-end that can accommodate user log-ins, validation, and possible multiple users.
- Multiple Players or Multiple Decks (we have the ability to add multiple decks built-in to API)
- Shuffling of the cards, do we want to shuffle mid-way through the deck?
- Animations => Re-rendering image of the deck for every draw and/or shuffle?
- How do we want to render the card drawing? Some considerations [PixiJS][5] to create animations with HTML5

#### Updates
I'll keep track of updates/obstacles that I encounter here, as there will certainly be cases I have not considered or thought through initially.



[1]:https://mintbean.io/meets?sort=upcoming
[2]:https://mintbean.io/meets/7e2331fb-1e0d-4b31-86b9-a46acad877af
[3]:https://bicyclecards.com/how-to-play/blackjack/
[4]:https://deckofcardsapi.com/
[5]:https://www.pixijs.com/