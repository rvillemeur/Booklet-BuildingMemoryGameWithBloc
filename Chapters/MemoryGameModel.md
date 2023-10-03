## Objectives of this book
grid := MgdGameElement new.
grid memoryGame: game.	

space := BlSpace new. 
space extent: 420@420.
space root addChild: grid.
space show 
   baseline: 'Bloc';
   repository: 'github://pharo-graphics/Bloc/src';
   load
    baseline: 'BlocTutorials';
    repository: 'github://pharo-graphics/Tutorials/src';
    load
	instanceVariableNames: 'symbol flipped announcer'
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Model'
	super initialize.
	flipped := false
	symbol := aCharacter
	^ symbol
	^ flipped
	^ announcer ifNil: [ announcer := Announcer new ]
	flipped := flipped not.
	self notifyFlipped
	self notifyDisappear
	self announcer announce: MgdCardFlippedAnnouncement new
	self announcer announce: MgdCardDisappearAnnouncement new
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Events'
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Events'
	aStream
		nextPutAll: 'Card';
		nextPut: Character space;
		nextPut: $(;
		nextPut: self symbol;
		nextPut: $)
	instanceVariableNames: 'availableCards chosenCards'
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Model'
	super initialize.
	availableCards := OrderedCollection new.
	chosenCards := OrderedCollection new
	^ availableCards
	^ chosenCards
	"Return grid size, total amount of cards is gridSize^2"
	^ 4
	"How many chosen cards should match in order for them to disappear"
	^ 2
	"Return how many cards there should be depending on grid size"
	^ self gridSize * self gridSize

	self
		assert: [ characters size = (self cardsCount / self matchesCount) ]
		description: [ 'Amount of characters must be equal to possible all combinations' ].
	availableCards := (characters asArray collect: [ :aSymbol | 
		(1 to: self matchesCount) collect: [ :i |
			 MgdCardModel new symbol: aSymbol ] ]) 
			    flattened shuffled asOrderedCollection
	(self chosenCards includes: aCard) 
		ifTrue: [ ^ self ].
	self chosenCards add: aCard.
	aCard flip.
	self shouldCompleteStep
		ifTrue: [ ^ self completeStep ].
	self shouldResetStep
		ifTrue: [ self resetStep ]
	^ self chosenCards size = self matchesCount 
		and: [ self chosenCardMatch ]
	| firstCard |
	firstCard := self chosenCards first.
	^ self chosenCards allSatisfy: [ :aCard | 
		aCard isFlipped and: [ firstCard symbol = aCard symbol ] ]
	self chosenCards 
		do: [ :aCard | aCard disappear ];
		removeAll.
	^ self chosenCards size > self matchesCount
	|lastCard|
	lastCard := self chosenCards  last.
	self chosenCards 
		allButLastDo: [ :aCard | aCard flip ];
		removeAll;
		add: lastCard