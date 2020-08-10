# TIC-TAC-TOE Tutorial Intro to React

I have finished up the oficial React Tutorial from [reactjs.org](https://reactjs.org/tutorial/tutorial.html) in this I will add my solutions to the additional excercises.

## Usage

This repository use yarn.

with ``` yarn start ``` command. 

## Additional Excercises

Here are the additional excercies 

### 1. Display the location for each move in the format (col, row) in the move history list.
## Code

In handleClick() function add an element to save latest move with the value that is clicked.

```
handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    ...
    this.setState({
      history: history.concat([{
        squares: squares,
        latestMove: i,
      }]),
      ...
    });
  }
```

Then we need to move to modify render function in order to display correctly our new element (col,row). We need to add two elements col and row and then append them to desc in order to display them.

```
render() {
    ...
    const winner = calculateWinner(current.squares);
    const moves = history.map((step, move) => {
      const latestMove = step.latestMove;
      const col = 1 + latestMove % 3;
      const row = 1 + Math.floor(latestMove / 3);
      const desc = move ?
        'Go to move #' + move + ' (' + col + ',' + row + ')':
        'Go to game start';
      ...
      );
    });
    ...
  }
```

#### 2. Bold the currently selected item in the move list.
#### 3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
#### 4. Add a toggle button that lets you sort the moves in either ascending or descending order.
#### 5. When someone wins, highlight the three squares that caused the win.
#### 6. When no one wins, display a message about the result being a draw. 