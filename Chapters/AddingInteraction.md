
	instanceVariableNames: 'memoryGame'
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Elements'
	memoryGame := aGameModel
	self halt
	^ MgdCardEventListener new
	| aCardEventListener |
	
	memoryGame := aMgdGameModel.
	aCardEventListener := self newCardEventListener memoryGame: aMgdGameModel.
	
	self layout columnCount: memoryGame gridSize.
	
	memoryGame availableCards
		do: [ :aCard | 
			| cardElement |
			cardElement := self newCardElement card: aCard.
			cardElement addEventHandler: aCardEventListener.
			self addChild: cardElement ]
space extent: 420@420.  
game := MgdGameModel numbers.
grid := MgdGameElement new.
grid memoryGame: game. 
space root addChild: grid.
space show 
	memoryGame chooseCard: anEvent currentTarget card
	Transcript show: 'On disappear'; cr
	Transcript show: 'On flipped'; cr
	card := aMgCard.
	card announcer when: MgdCardFlippedAnnouncement send: #onFlipped to: self.
	card announcer when: MgdCardDisappearAnnouncement send: #onDisappear to: self
	Transcript show: 'On flipped'; cr.
	self invalidate
	Transcript show: 'On disappear'; cr.
	self opacity: 0
	Transcript show: 'On disappear'; cr.
	self visibility: BlVisibility hidden
	flipped := aBoolean.
	self notifyFlipped
	| lastCard |

	lastCard := self chosenCards  last.

	self chosenCards 
		allButLastDo: [ :aCard | aCard flip ];
		removeAll;
		add: lastCard