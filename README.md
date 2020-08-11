# TIC-TAC-TOE Tutorial Intro to React

I have finished up the oficial React Tutorial from [reactjs.org](https://reactjs.org/tutorial/tutorial.html) and in this repository I will add my solutions to the additional excercises.

## Usage

This repository use yarn use ``` yarn install ``` and then ``` yarn start ``` command to run local view

## Additional Excercises

Here are the additional excercies 

### 1. Display the location for each move in the format (col, row) in the move history list.
#### Code

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

Then we need to modify render function in order to display correctly our new element (col,row). We need to add two elements col and row and then append them to desc in order to display them.

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

### 2. Bold the currently selected item in the move list.
#### Code
For this, first we need to add a class rule to CSS.
```
 .selected {
    font-weight: bold;
  }
```

In render function we add a conditional for ``` className ``` if move is strictly equal to stepNumber then we change the class to selected else we stay the same.

```
render() {
    const history = this.state.history;
    ...
      return (
        <li key={move}>
          <button className = {move === this.state.stepNumber ? 'selected' : ''}
          onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
    ...
    );
  }
```
Then we see that it comes bold the selected item.

### 3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
#### Code
Into the class Board we will change all the code in the render function. 
We initialize with a constant of boardSize with a size of 3 and squares empty. We need to use two for loops, one inside another and with the method array.prototype.push() to render next square until the row is complete.

```
class Board extends React.Component {
  renderSquare(i) {
  ...
  }

  render() {
    const boardSize = 3;
    let squares = [];
    for(let i=0; i<boardSize; ++i) {
      let row = [];
      for(let j=0; j<boardSize; ++j) {
        row.push(this.renderSquare(i * boardSize + j));
      }
      squares.push(<div key={i} className="board-row">{row}</div>);
    }

    return (
      <div>{squares}</div>
    );
  }
}
```

### 4. Add a toggle button that lets you sort the moves in either ascending or descending order.
#### Code
First in class game, in render() method, we need to create an element type button that when is clicked calls a function to sort the list. We add status ascending or descending and add a conditional rule for display the type of sort.

```
render() {
    const history = this.state.history;
...

    return (
      <div className="game">
        ...
        <div className="game-info">
          <div>{status}</div>
          <button onClick={() => this.handleToggle()}>{isAscending ? 'descending' : 'ascending'}</button>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
```

We add the state in game class isAscending set it to true at the first time.

```
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      ...
      isAscending: true,
    };
  }
```
We have to declare our constant isAscending in order to save the state and do the sort with the help of JS method array.prototype.reverse()

```
const isAscending =this.state.isAscending;
    if(!isAscending) {
      moves.reverse();
    }
```

And finally we need to declare our function handleToggle() in order to update the state of ascending or descending in game class.
```
handleToggle() {
    this.setState({
      isAscending: !this.state.isAscending,
    });
  }
```
### 5. When someone wins, highlight the three squares that caused the win.
#### Code
First we need to add a rule for square highlight in the css file
```
.win {
    background: #ddd;
  }
```
Then we need to change the return value of function calculateWinner() because we have wich squares made winner into lines[i].
```
function calculateWinner(squares) {
  const lines = [
    ...
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return {
        winner: squares[a],
        line: lines[i],
      }
    }
  }
  return {
    winner: null,
  }
}
```
We need to change the recived information from calculateWinner at handleClick() function and at Game class in the render method.
First we start with handleClick.
```
handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    ...
    if (calculateWinner(squares).winner || squares[i]) {
      return;
    }
    ...
  }
```
Then we go to const winner in render() method. and change the recieved information. First we need to add a constant that receives the full information from our object and select the information from winner at the declaration. And finally we add our winner line in the return with the win information.
 ```
class Game extends React.Component {
 ...

  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winInfo = calculateWinner(current.squares);
    const winner = winInfo.winner;
    ...

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
            winLine={winInfo.line}
          />
        </div>
       ...
      </div>
    );
  }
}
 ```
We need to update our Board class in order to update the received information, we need to change the renderSquare function and receive the information of winner line.
 ```
class Board extends React.Component {
  renderSquare(i) {
    const winLine = this.props.winLine;
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
        highlight={winLine && winLine.includes(i)}
      />
    );
  }
...
}
 ```
And we need to apply a conditional rule to apply css style if the square is winner.
```
function Square(props) {
  const classAssigned = 'square' + (props.highlight ? ' win' : '');
  return (
    <button className= {classAssigned}
      onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```
### 6. When no one wins, display a message about the result being a draw. 
#### Code
First we need to change claculateWinner() function. We need to add a draw state and a conditional that if there is a draw change the state of boolean to true.
```
function calculateWinner(squares) {
  const lines = [
    ...
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return {
        winner: squares[a],
        line: lines[i],
        isDraw: false,
      }
    }
  }
  let isDraw = true;
  for (let i = 0; i < squares.length; i++) {
    if (squares[i] === null) {
      isDraw = false;
      break;
    }
  }
  return {
    winner: null,
    line: null,
    isDraw: isDraw,
  };
}
```
Then we need to update render function and the part of status.
```
 render() {
    const history = this.state.history;
    ...

    let status;
    if (winner) {
      status = "Winner: " + winner;
    } else {
      if (winInfo.isDraw) {
        status = "Draw";
      } else {
        status = "Next player: " + (this.state.xIsNext ? "X" : "O");
      }
    }
...
  }
```
And it is done.

