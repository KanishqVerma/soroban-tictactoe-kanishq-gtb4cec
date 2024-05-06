# 🚀 Stellar Tic-Tac-Toe 🚀
This is a project that enables players to play Tic-tac-toe games on the Stellar network.

It consists of two contracts: the first contract is used to create and keep track of Tic-tac-toe games, and the second contract is used to play the games

To prevent excessively long games, each game has a default duration of 10 minutes, after which it will end automatically.

## Table of Contents
<ol>
<li><a href="#functions">Functions</a></li>
<li><a href="#install">Install and Build</a></li>
<li><a href="#deployment-to-futurenet">Deployment</a></li>
<li><a href="#playing-the-game">Interacting with game</a></li>
</ol>


# Contracts

## Manager contract
This contract is used to initialize a new tic-tac-toe game and define its players. The order matters; the first player starts first.
It stores all the games and their states, which can be accessed later.

<br />

## Game contract
This contract manages the game itself. To make a move, you need to call the `play` function.
It checks for winning conditions or a tie and ends the game when either condition is met.

It also allows the player to make bets. If there is a winner, the minimum bet is paid and the rest is returned.
If there is no winner at the end of the game, the players ask for their bet to be returned.

<br />

---

<br />

# Functions

## Game Functions
### Init
Initialize the players using the `init` function.

⚠️ Only use this function if you are not deploying the game using the manager contract.

Player_a always starts first
```
Arguments:
    player_a: Address,
    player_b: Address,
    expiration: u64       // Expiration as unix timestamp
```

### Play
To play, each player needs to call the play function and pass their own address and the desired position to mark as arguments.

An error will be thrown if the player is not one of the two players, if it is not their turn, or if the player's address doesn't match the caller's address.

```
Arguments:
    player: Address, 
    pos_x: u32,
    pos_y: u32
```

The position must be within the grid range and correspond to the following grid.
| 2-2 | 1-2 | 0-2 |
|-----|-----|-----|
| 2-1 | 1-1 | 0-1 |
| 2-0 | 1-0 | x=0-y=0 |

### Turn
To know whose turn it is, call the `turn` function without any argument.


### Grid
To view the grid you can call the `grid` function without any argument.

### Bet
Make a bet by calling the `bet` function. If a bet has already been made, the function will increase the existing bet by the specified amount. Note that players can only bet on their own victory.
```
Arguments:
    player: Address, 
    token: BytesN<32>,
    amount: i128,
```

### Collect Bet
After the game has ended, players can collect their winnings. In the event that they have bet a higher amount than their opponent, the difference will be returned to them.
For that call the `clct_bet` with your own address  
```
Arguments:
    player: Address, 
```

### Send Message
Players can interact with each other through a chat feature. To send a message, a player must call the `send_msg` function with the following arguments.
```
Arguments:
    player: Address, 
    message: Symbol
```

### Chat
To view all previous messages in the chat, call the `chat` function without any argument.

<br/>

## Manager Functions
### Deploy
⚠️ Before deploying you must deploy the Game contract


Deploy new games using the `deploy` function with the following arguments:
```
Arguments:
    salt: Bytes,
    wasm_hash: BytesN<32>, // the hash of the game contract 
    init_args: Vec<Val> // init_args should contain player_a and player_b addresses
```
It will return the Address of the Game contract

### Get game information
The manager stores all the deployed game and its status,
call `game` with the game address to know the players and if the game is ended
```
Arguments:
    id: Address // Game address
```

<br/>

---

<br />

# Install
## Clone this repository
```
git clone https://github.com/elfacu0/soroban-tictactoe.git
```

## You can run the test with
```
cargo test
```

## Build the Contracts
⚠️ Since the contract manager uses certain functions from the game contract, you need to first build the game contract and place it inside the game folder. (In this repository, a game contract is already provided in the game folder.)

To build the contracts, execute the following command:
```
soroban contract build
```

<br/>

---

<br />
