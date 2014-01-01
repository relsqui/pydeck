## cribbage-scorer

Some basic card classes and a cribbage scoring library.


cards.py contains:

* strings `RANKS` ("234568789TJQK") and `SUITS` ("cdhs")
* dictionaries `RANK_NAMES` and `SUIT_NAMES` mapping the above to capitalized
  words.
* the `Card` class, with no methods besides initializers and comparators.
  `Card`s should be initialized with a two-character string consisting of
  rank + suit, e.g. `Card("Jh")` would create the Jack of Hearts.
* the `Hand` class. `Hand`s can have `Card`s `.append()`ed to them, or lists of
  `Card`s `.extend()`ed to them. You can `.sort()` or `.shuffle()` them in
  place, `.pop()` one off, `.deal()` several off, or (non-destructively) return
  sublists of `Card`s `.by_rank()` or `.by_suit()`.
* `make_deck()` returns a `Hand` consisting of a standard deck of 52 `Card`s.
  `make_deck(shuffle=False)` will keep them in order.

When called from the command line, cards.py just deals a hand of 13 cards and
prints it.
If you want to use a nonstandard deck, just assign a character to each
rank/suit and override the constants.


cribbage.py contains:

* `score_hand()`, which takes a `Hand`, an optional `turned=Card`, and optional
  booleans `crib` and `dealer` (for scoring nobs, heels, and flushes; both
  default to false). its components are:
  * `score_pairs()`, `score_fifteens()`, and `score_runs()`, which each take a
    `Hand` and return a partial score
  * `check_flush()`, which takes a `Hand` and returns whether all cards in it
     have the same suit
* `value()`, which takes a `Card` and returns its point value in cribbage

cribbage.py overrides `cards.RANKS`, just to reorder them so A is low.
When called from the command line, cribbage.py deals a hand of four cards,
turns a card, chooses randomly whether it's the dealer and whether it's
counting a crib, and scores the hand.


I wrote this because I wanted to know what the cribbage score for a whole deck
of cards was. Here's the answer, if you're curious:

```
relsqui@barrett:~/cribbage-scorer$ cat score_the_deck.py 
#!/usr/bin/python

import cards, cribbage

score = cribbage.score_hand(cards.make_deck())
for k, v in score.items():
    print k, "for", v
print "total:", sum(score.values())


relsqui@barrett:~/cribbage-scorer$ ./score_the_deck.py 
fifteens for 34528
runs for 872415232
pairs for 156
total: 872449916
```
