# 5-Card-Draw-Poker-Optimal-Play

This project was an effort to determine the 'optimal' strategy for the game of 5 card draw poker where you have only imperfect information at all times. The goal was to directly find the 'best' choice a player should make given their starting hand and the number of other players also playing. It is impossible to have 'perfect' play and win every game, given the lack of complete knowledge and the randomness of drawing cards, so 'best' choice is simply the choice leading to the highest amount of wins per set of games.

## Basic Rules

5 card draw poker begins with each player receiving 5 random cards to make their 'hand'. Then, each player on their turn may choose to remove any or all of their starting cards for new cards from the remainder of the deck. Once all players have had their choice of redrawing cards, the hands are revealed. The hands are then compared based on what their total hand value would be in standard poker rules.

For the purposes of this optimization, a win for the player was the case where they did not have the lowest valued hand among all of the other players playing. I.e. given N players, only the worst hand is determined to be the loser and N-1 are winners.

## Optimization method

Given the relative simplicity of 5 card draw poker, a brute force simulation of games was used to check the effectiveness of a choice on a given hand. To perform the simulation, a basic A.I. strategy was created to mimic how a knowledgable human would play their hand. Each A.I. player in the simulation would perform their hand, then the 'player' would run through every possible card redraw choice available on their turn and tally up their wins on each choice. Roughly 100,000 simulations were performed for each possible hand (ignoring suits) and the general winrate of each choice per hand given was determined then separately analyzed.

## AI Strategy

Humans with poker knowledge generally understand that hands valued a straight or higher are difficult to obtain and are worth keeping over attempting anything better. So, the A.I. strategy kept those hands when rarely gifted.

On a three of a kind hand, the other two cards were always redrawn in attempting a full house.

On a two pair the fifth card is redrawn again attempting a full house.

On a pair the other three cards are always redrawn.

Finally with no specific hand, the highest card is kept and the rest replaced.

## Flushes

Since all but one poker hand is impacted by the suit of a card, two variations of optimization were created. The first to determine optimal choice when ignoring the possibility of a flush, and the other to determine optimal choice when given a hand close to acquiring a flush. This was done to reduce the computational heft of the first simulation, as adding in the suits increasing the hand complexity immensely. This also simplified the problem into two easier methods of sorting a player hand prior to redraw choices.

The flush specific simulation relied on a definition of nearness to a flush being the number of cards suited towards a flush (between 1 and 5). Simulations were performed for a nearness score of 4 (1 card off-suit from being a flush hand). Then, choices were performed and optimal play analayzed from those positions.

The files relating to flushes are the flush specific simulations and analysis, and the other two the general game simulation without accounting for flushes.

## Results

Funny enough, the A.I. strategy first adopted is nearly identical to the best choices the player could make with almost every hand given. The differences came down to edge cases with hands of a pair or a high card. Additionally, there was identifiable strategical differences depending on whether four other players or just one were playing, likely due to the definition of a win/loss. Thus, slightly riskier strategies for higher valued hands were better with more players compared to a one-on-one game.

The strategies in broadstrokes are the following:

Anything higher than a pair, employ the A.I. strategy.

### Pair

Keep the pair always. If the pair is of aces (A) and a king (K) is in hand, keep the king. In most cases keeping an ace along with a non-ace pair was the best choice, so keep an ace high card if given.

When one off of a flush and you have a pair, i.e. one of the pair cards is preventing the flush. Keep the pair, it's too risky.

### High Card

Keep the top 3 highest if they are A,K,Queen (Q).
Keep the top 2 highest if A and Jack (J) to K. Or if K,Q.
Keep top highest always, unless starting hand is among the worst to start with. Which are generally slight variations of 2,3,4,5,7. But only the bottom 5 or so lowest valued high-card hands required a full redraw.

If you are one off of a flush:

Redraw the off-suit card if off-suited card also might create a straight for the hand AND your lowest card is a 6 or higher. Why? I don't know but that's what the numbers tell us.
If there is no possibility of redrawing a straight with the off-suit card, redraw the off-suit card if you have suited A,K and suited J-Q and 8-9 valued cards.
Otherwise, employ the non-flush oriented strategy above.

## Winrate Increase

As a test to determine the effectiveness of these improved choices against the A.I. choices, final simulations were run employing these strategies against 4 A.I. players. Unfortunately, the base strategy is the bulk of the work in winning games, and the extra tweaks make hardly any improvements. A simulation of 5 million such games netted a winrate of 80.47%, whereas perfectly equal play among these five players should average out to 80%. So, a half a percent increase in winrate was achieved.
