pgnreader
===

a chess PGN (Portable Game Notation) file reader (written in golang) (originally forked from github.com:wfreeman/pgn)

[![Build Status](https://travis-ci.org/handcraftsman/pgnreader.png?branch=master)](https://travis-ci.org/handcraftsman/pgnreader)
[![Coverage Status](https://coveralls.io/repos/handcraftsman/pgnreader/badge.png?branch=master)](https://coveralls.io/r/handcraftsman/pgnreader?branch=master)

Normal go install... `go get github.com/handcraftsman/pgnreader`

## minimum viable snippet

```Go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/handcraftsman/pgnreader"
)

func main() {
	f, err := os.Open("polgar.pgn")
	if err != nil {
		log.Fatal(err)
	}
	ps := pgn.NewPGNScanner(f)
	// while there's more to read in the file
	for ps.Next() {
		// scan the next game
		game, err := ps.Scan()
		if err != nil {
			log.Fatal(err)
		}

		// print out tags
		fmt.Println(game.Tags)

		// make a new board so we can get FEN positions
		b := pgn.NewBoard()
		for _, move := range game.Moves {
			// make the move on the board
			b.MakeMove(move)
			// print out FEN for each move in the game
			fmt.Println(b)
		}
	}
}
```

produces output like this for each game in the pgn file:

```
map[Event:Women's Chess Cup
    Site:Dresden GER 
    Round:7.1 
    Black:Polgar,Z 
    Result:1/2-1/2 
    Date:2006.07.08 
    White:Paehtz,E 
    WhiteElo:2438 
    BlackElo:2577 
    ECO:B35]

```

## license

MIT License, see LICENSE file.
