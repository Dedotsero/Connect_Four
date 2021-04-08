# Connect Four Assignment

These steps will help guide you through creatisng a game.

1. Create a file named game.js in the same directory as the connect-four.js file.

2. In that file, declare and export a class named Game that has a constructor that takes the names of the two players and sets them to instance variables on the object. (Remember that creating instance variables requires the use of the this keyword, such as this.name1 = playerOneName;, for example, creates an instance variable named name1 and sets it to the value of playerOneName.)
3. A method named getName() that returns a string of "Player 1 Name vs. Player 2 Name".
4. In `index.html`:
   - Add the `"is-invisible"` class to `#board-holder`.
   - Add the `"disabled"` attribute to `#new-game`.
5. In `connect-four.js`:
   - At the top of the file, import the Game class from the file at the path `v./game.js` using `import { Game } from './game.js';` because you want to load a file that will contain the Game class. Remember that you have to use the ".js" on the file name because you're loading these directly in the browser.
   - Next declare a global variable named game and set it to `undefined`.
   - Add an event handler for the window's `"DOMContentLoaded"` event. In that handler, have it:
   - Create an event handler for the "keyup" event of #player-1-name. In the event handler, have it set #new-game's "disabled" property to false if both #player-1-name and #player-2-name have non-empty content, enable #new-game. Otherwise, disable #new-game.
   - Create an event handler for the "keyup" event of #player-2-name. In this event handler, do the exact same thing you did in the #player-1-name handler. Now, think, did you copy and paste some code? If so, can you refactor it somehow to remove duplication because the intended behavior is identical?
   - Create an event handler for the "click" event of #new-game that, when clicked:
     - Sets the global variable game to a new instance of the Game class passing in the two players' names.
     - Sets the values of the two player name input elements to empty strings.
     - Sets the disabled property on #new-game to true, thereby disabling it. (See if you have this functionality already written somewhere and, if you do, somehow reuse it so prevent code duplication.)
     - Calls a function named updateUI().
   - Declare a function named `updateUI()` after the game variable declaration and before the event listener for "DOMContentLoaded". In that function, put the following logic:
     - If game is undefined, have it add the "is-invisible" class to #board-holder.
     - If game is not undefined have it remove the "is-invisible" class from #board-holder and set the inner HTML of the #game-name element to the value returned by the getName() function of the object stored in the game variable.
6. In `game.js`:
   - In the constructor of the Game class, create a new instance variable to track the current player and set it to 1 to indicate that its the first player.
   - Create a method called playInColumn() that will handle the click of the user. In that method, change the value of the current player to the other player. If it was 1, then make it 2; otherwise, make it 1.
7. In `connect-four.js`:
   - In the event handler for "DOMContentLoaded", register an event handler for the "click" event on #click-targets.
   - In the #click-targets event handler that you just created, have it call the playInColumn method of the object in the global variable game. Then, have it call the updateUI method.
   - In the updateUI function, in the portion of the code that uses the game object, have it get the current player from the instance variable that you created in the constructor of the Game class. Use that value to determine if you need to add "red" and remove "black" from the #click-targets element for player two, or if you need to add "black" and remove "red" for player one from that element.
8. In the same directory as your connect-four.js and game.js files, create a new file named column.js. (Note that you don't have to do this, this one class per file thing. It is generally considered best practice to do it so that you know where to go look for a specific class rather than having to search through a long JavaScript file to see if a class definition is in there.)

---

### The following instructions will cause you to think hard about how you can and want to do this. If you struggle with this, see the hints in the next task for two different ways to implement this, each with their own trade offs.

## In the column.js file:

Create and export a class named Column.
Create a constructor that will create a way to manage the tokens stored in the column.
Create a method named add that takes a player number and stores it in the "bottom-most" entry in the column.
Create a method named getTokenAt which takes a row index number between 0 and 5 and returns null if there's no token there, 1 if player one's token is there, or 2 if player two's token is there.

## In game.js:

Add to the Game constructor a new instance variable named columns and initialize it to an array of seven Column objects.
Modify the playInColumn method so that it takes the index of the column in which to play, uses that index to select the correct column from the array of columns, and calls the add method on that column object passing in the number of the current player. Make sure that you leave the toggling of the current player from one to two and back, again, in the method at the end of it.
Add a method named getTokenAt that takes the row index and the column index. Use the column index to get the correct column from the columns array. Then, call the getTokenAt method on the column object passing in just the row number. Return the return value of that function.

Now that you have the Game and Column classes ready to go, you can hook them up to the events that you're already capturing. In connect-four.js, in the event handler for click targets that you already have, before the call to the playInColumn method, you need to get parse the number of the click target that the player clicked on. Make sure your event handler is getting the event object in its parameter list. Then, access the "id" property of the "target" property of the click event. If it's a click that you want to handle, make sure that id value starts with the string "column-". If it does, then use Number.parseInt to convert the last character of the id into a number. Pass that number into the playInColumn method.

In the updateUI method, it's now time to show the tokens in the board. Create a for loop that will loop through the values from zero to five, inclusive; that will be the row index. Then, inside that for-block, create another for loop that loops from the values zero to six, inclusive; that will be the column index. Now, you have a row index and a column index to use to update the board. Inside the inner loop:

Select the element #square-«row»-«column» using the row and column indexes that you have.
Use the getTokenAt method on the Game object stored in the global game variable. The value that gets returned from getTokenAt will determine what you should do:
First, clear out the inner HTML of the square you selected in the previous step by setting it to an empty string
If the value returned by getTokenAt is 1, then create a "div" element, make sure it has both the "token" and "black" classes, and add it as the child to the square.
If the value returned by getTokenAt is 2, then create a "div" element, make sure it has both the "token" and "red" classes, and add it as the child to the square.

Now, you need to figure out if a column is full for two reasons:

In the "model": You don't want to add a token to a column that is already full.
In the UI: You want to change the appearance of the click targets.
You need to do something about the behavior of a column. Since you have a Column class already, you can just do it in there! See how this object-oriented thing makes it easy to determine where to put new features!?!?

## In the Column class:

Depending on how you implemented it, just don't add the token if there is no available slot for it.
Add a method named isFull and have it return true if there are no more available slots (that is, there are already six tokens in it).
Since the updateUI is going to need to know if a column is full and the updateUI method only knows about the Game object and none of the Column objects (because you're following the Law of Demeter with respect to your own code), add an isColumnFull method that takes a column index between 0 and 6, inclusive, and returns the value of the isFull method invoked on the appropriate Column object stored in the columns array.

In the updateUI method, create a for loop that iterates over the values from 0 to 6, inclusive. For each value:

Select the element with the id of "column-«column index»".
If the value returned from the isColumnFull method on the Game object is true, then add the "full" class to the element selected in the previous step.
If the value returned from the isColumnFull method on the Game object is false, then remove the "full" class to the element selected in the previous step.

In this case, you will just need to check if all of the columns are full. You already have the isFull method on each Column object, it should be a snap to get a tie determined.

In terms of responsibility, it seems like the Game should know whether or not the game is in a tie. So, you'll put this behavior in the Game class.

This seems like a new instance variable to carry whether the game is over. Declare an instance variable in the constructor named winnerNumber. You'll set that to 1 if Player One wins, 2 if Player Two wins, and 3 if both win (that is, it is a tie). Set winnerNumber in the constructor equal to 0 to indicate that no one has won.

You need to check for a tie after every play. You have a method named playInColumn in the Game class, so this seems like a nice place to check for a tie. Rather than putting all of the code in the playInColumn method, call a new method named checkForTie at the end of the playInColumn method.

Now, declare the checkForTie method. In there, check to see if every column is full. If so, set the instance variable winnerNumber to 3.

In the getName method, check to see if the instance variable winnerNumber has the value of 3. If it does, have it return the message "«Player one name» ties with «Player two name»!" Otherwise, just let it return the "vs" message it already returns.
