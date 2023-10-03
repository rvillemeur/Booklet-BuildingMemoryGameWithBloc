## Building card graphical elements
	instanceVariableNames: 'card'
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Elements'
	^ Color lightGray
	^ card
	card := aMgCard
	super initialize.
	self size: 80 @ 80.
	self card: (MgdCardModel new symbol: $a)
	aCanvas fill 
		paint: self backgroundPaint;
		path: self boundsInLocal;
		draw
	aCanvas fill 
		paint: self backgroundPaint;
		path: (aCanvas shape ellipse: self boundsInLocal);
		draw
	^ 12
	| roundedRectangle |
	
	roundedRectangle := aCanvas shape 
		roundedRectangle: self boundsInLocal 
		radii: (BlCornerRadii radius: self cornerRadius).

	aCanvas clip
		by: roundedRectangle
		during: [
			aCanvas fill
				paint: self backgroundPaint;
				path: self boundsInLocal;
				draw. ]
	"nothing for now"
	"nothing for now"
	| roundedRectangle |
	
	roundedRectangle := aCanvas shape 
		roundedRectangle: self boundsInLocal 
		radii: (BlCornerRadii radius: self cornerRadius).

	aCanvas clip
		by: roundedRectangle
		during: [
			aCanvas fill
				paint: self backgroundPaint;
				path: self boundsInLocal;
				draw.
				
			self card isFlipped
				ifTrue: [ self drawFlippedSideOn: aCanvas ]
				ifFalse: [ self drawBacksideOn: aCanvas ] ]
	aCanvas fill
		paint: self backgroundPaint;
		path: self boundsInLocal;
		draw
	| roundedRectangle |
	
	roundedRectangle := aCanvas shape 
		roundedRectangle: self boundsInLocal 
		radii: (BlCornerRadii radius: self cornerRadius).

	aCanvas clip
		by: roundedRectangle
		during: [
			self drawCommonOn: aCanvas.
			self card isFlipped
				ifTrue: [ self drawFlippedSideOn: aCanvas ]
				ifFalse: [ self drawBacksideOn: aCanvas ] ]
	aCanvas stroke
		paint: Color paleBlue;
		path: (aCanvas shape line: 0@0 to: self extent);
		draw.
	aCanvas stroke
		paint: Color paleBlue;
		path: (aCanvas shape line: 0@0 to: self extent);
		draw.

	aCanvas stroke
		paint: Color paleBlue;
		path: (aCanvas shape line: self width @ 0 to: 0@self height);
		draw
cardElement := MgdRawCardElement new.
cardElement card flip.
cardElement
	| font |
	font := aCanvas font
		named: 'Source Sans Pro';
		size: 50;
		build.
	aCanvas text
		font: font;
		paint: Color white;
		string: self card symbol asString;
		draw
	| font origin |
	font := aCanvas font
		named: 'Source Sans Pro';
		size: 50;
		build.
	origin := self extent / 2.0.
	aCanvas text
		baseline: origin;
		font: font;
		paint: Color white;
		string: self card symbol asString;
		draw
	| font origin textPainter metrics |
	font := aCanvas font
		named: 'Source Sans Pro';
		size: 50;
		build.

	textPainter := aCanvas text
		font: font;
		paint: Color white;
		string: self card symbol asString.
	
	metrics := textPainter measure. 
	
	origin := (self extent - metrics textMetrics bounds extent) / 2.0.
	textPainter 
		baseline: origin;
		draw
	| font origin textPainter metrics |
	font := aCanvas font
		named: 'Source Sans Pro';
		size: 50;
		build.

	textPainter := aCanvas text
		font: font;
		paint: Color white;
		string: self card symbol asString.

	metrics := textPainter measure. 

	origin := (self extent - metrics textMetrics bounds extent) / 2.0.
	origin := origin - metrics textMetrics bounds origin.
	textPainter 
		baseline: origin;
		draw
grid := MgdGameElement new.
grid memoryGame: game. 
	instanceVariableNames: 'memoryGame'
	classVariableNames: ''
	package: 'Bloc-MemoryGame-Demo-Elements'
	memoryGame := aMgdGameModel
	^ memoryGame
	super initialize.
	self layout: BlGridLayout horizontal.
	memoryGame := aGameModel.
	
	memoryGame availableCards
		do: [ :aCard | self addChild: (self newCardElement card: aCard) ]
	^ MgdRawCardElement new
	super initialize.
	self layout: BlGridLayout horizontal.
	self
		constraintsDo: [ :aLayoutConstrants | 
			aLayoutConstraints horizontal fitContent.
			aLayoutConstraints vertical fitContent ]
	memoryGame := aGameModel.
	self layout columnCount: memoryGame gridSize.
	memoryGame availableCards
		do: [ :aCard | self addChild: (self newCardElement card: aCard) ]
	super initialize.
	self background: (BlBackground paint: Color gray darker).
	self layout: (BlGridLayout horizontal cellSpacing: 20).
	self
		constraintsDo: [ :aLayoutConstraints | 
			aLayoutConstraints horizontal fitContent.
			aLayoutConstraints vertical fitContent ]