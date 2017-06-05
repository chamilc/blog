
### Poker Card Game Hand Simulator using Python
---
![poker][img7] 
Image from [Don't play the cards, Play the people][link6]</br>

This is part of python learning experience with [matplotlib][link2], [numpy][link3]
and [scipy][link4]. 

After losing lots of money on poker game, what? yeah I said in iPhone games, it was an 
attempt to analyse the probability of winning the money back. 

>In poker, players construct hands of five cards according to predetermined rules,
> which vary according to which variant of poker is being played. These hands are 
>compared using a hand ranking system that is standard across all variants of poker,
> the player with the highest-ranking hand winning that particular deal in most 
>variants of poker. In some variants, the lowest-ranking hand can win or tie. [List of poker hands][link1]

For more details about the poker hand ranking, refer the above wiki link.

I use the following abbreviation for each poker hand rank to display the data. 
<pre>
<code>
 P 	  --> One Pair        : Two cards with the same rank </br>
 TP   --> Two Pair        : Two pairs of cards with the same rank</br>
 3K   --> Three of a Kind : Three cards with the same rank</br>
 S    --> Straight        : Five cards with ranks in sequence</br>
 F    --> Flush           : Five cards with the same suit</br>
 FH   --> Full House      : Three cards with one rank, two cards with another </br>
 4K   --> Four of a Kind  : Four cards with the same rank</br>
 SF   --> Straight Flush  : Five cards in sequence and with the same suit</br>
 SNL  --> Sorry, No Luck </br>
</code>
</pre>

Initially I started to to code the poker hand simulator based on some text book, when I
saw the output which is almost close the theoretical value, I got the idea to plot the
values using some python plotting libraries as a learning  experience. Wow, the data
analysis with python is awesome with powerful libraries like [matplotlib][link2], [numpy][link3]
and [scipy][link4]. You can refer the source code [here.][link5] 

The following results are based on following conditions,
<ul>
	<li>For each shuffling, distribute 7 cards per hand for 7 hands.</li>
	<li>Check each hand's rank at first time before moving any cards.</li>
	<li>Repeat the shuffling for 10000 times and accumulated result was taken.</li>
	<li>Use the python's random.shuffle() api for shuffling the cards.</li>
</ul>
	
Shuffle a deck of cards to each hands.
![Card shuffle code][img4]


Following Bar chart and and Pie chart show the probability of each hand's rank after 
10000 times of card shuffling. 

![Probability Bar Chart][img1]
![Probability Pie Chart][img2]

The following chart shows the consistency of the above result. I tried to repeat the same
test for 10 times, the result was almost identical.
![Number of Occurrences][img3]

Code to plot Pie Chart using MatplotLib 
![Pie Chart using Matplotlib][img5]

In order to verify the simulator result with existing theoretical value, I repeated the 
test with 5 cards per hand. After 10000 times of shuffling, following result shows the 
comparison. Refer this link for theoretical values [Hand's Ranking][link1].
![test][img6]



[link1]: http://en.wikipedia.org/wiki/Hand_rankings
[link2]: http://matplotlib.org
[link3]: http://www.numpy.org
[link4]: http://www.scipy.org/
[link5]: https://github.com/tssutha/PokerSimulator
[link6]: http://www.parade.com/172290/ellenleikind/life-lessons-from-poker-dont-play-the-cards-play-the-people/
[img1]: /geveoblog/img/pokerbar.png "Bar Chart : Probability of each rank after 10000 shuffles"
[img2]: /geveoblog/img/pokerpie.png "Pie Chart : Probability of each rank after 10000 shuffles"
[img3]: /geveoblog/img/pokerline.png "Number of occurrences of each rank in 10000 shuffles, repeated 10 times"
[img4]: /geveoblog/img/codesnipet.png
[img5]: /geveoblog/img/codesnippet2.png
[img6]: /geveoblog/img/compare.png
[img7]: /geveoblog/img/poker.jpg


