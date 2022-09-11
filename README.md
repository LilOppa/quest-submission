# EA Quest submissions

## Chapter 1 Day 1

1. **Blockchain** - decentralized shared database where data stored in blocks and linked together via cryptography
2. **Smart-contract** - simply program stored on a blockchain that run when user interect with him
3. **Transaction** is changes the data on the Blockchain and need an exucation fee. **Script** is only reads data (doesn't change it) on the Blockchain and free

## Chapter 1 Day 2

1. Safety and Security, Clarity, Approachability, Developer Experience, Resource Oriented Programming
2. 
- **Safety and Security** prevent hacking of smart-contracts and leakage of user funds.
- **Clarity** allows us to make sure that the smart-contract is secure developer has not added his malicious code there.
- **Approachability** The fact that Cadence is very similar to other programming languages makes it easy for many developers to go to Flow and start building.
- **Developer Experience** allows developers to quickly find bugs and fix them.

## Chapter 2 Day 1

1. 

![image](https://user-images.githubusercontent.com/72570095/189487636-b5db39d1-a541-4fcd-a3ea-b3c285927782.png)

2.

![image](https://user-images.githubusercontent.com/72570095/189487650-0c25dca3-ea15-448f-b618-be81471ff823.png)

## Chapter 2 Day 2

1. A script cannot changes data, so we call "changeGreeting" function in a transaction (transaction is changes the data).
2. The "AuthAccount" type is used to access the data in your account.
3. The "prepare" phase is used to access the information/data in your account. The "Execute" phase can't do that, but it can call functions and do stuff to change the data on the blockchain.
4.

Contract

![image](https://user-images.githubusercontent.com/72570095/189492131-06a25a9b-6daf-4507-b140-e643ea296567.png)

Script

![image](https://user-images.githubusercontent.com/72570095/189492241-38d75517-a12b-4933-af49-74a85d834989.png)

Transaction

![image](https://user-images.githubusercontent.com/72570095/189492592-8b4df99a-6a10-4024-b895-68c6a8e7391e.png)

Result

![image](https://user-images.githubusercontent.com/72570095/189492626-6113f299-a6de-45bb-ae89-25135bc12a71.png)

## Chapter 2 Day 3

1.

![image](https://user-images.githubusercontent.com/72570095/189526376-fd609de0-db5e-458d-9fe8-78c3d4147506.png)

2.

![image](https://user-images.githubusercontent.com/72570095/189527003-21bf8dfb-68c3-4209-b8bf-98521de81aa6.png)

3. The force unwrap operator ```!``` is used to "unswraps" an optional type: 
- if value not nil, its fine and we get rid of the optional type
- if value is nil, PANIC!

```
pub fun main() {
  var number1: Int? = 7
  var unwrappedNumber1: Int = number1! // Notice it removes the optional type

  var number2: Int? = nil
  var unwrappedNumber2: Int = number2! // PANICS! The entire program will abort because it found a problem. It tried to unwrap a nil, which isn't allowed 
}
```
4. 

- The error message means the function expects return is ```String``` type, but return is ```String?```
- When you access elements of a dictionary, it returns the value as an optional
- ```String``` => ```String?```

  ```return thing[0x03]``` => ```return thing[0x03]!```
  
 ## Chapter 2 Day 4
 
 1. Contract

``` 
    pub contract Pokedex {

    pub var pokemons: {Address: Pokemon}
    
    pub struct Pokemon {
        pub let name: String
        pub let type: String
        pub let total: Int64
        pub let hp: Int64
        pub let account: Address

        // You have to pass in 5 arguments when creating this Struct.
        init(_name: String, _type: String, _total: Int64, _hp: Int64, _account: Address) {
            self.name = _name
            self.type = _type
            self.total = _total
            self.hp = _hp
            self.account = _account
        }
    }

    pub fun addPokemon(name: String, type: String, total: Int64, hp: Int64, account: Address) {
        let newPokemon = Pokemon(_name: name, _type: type, _total: total, _hp: hp, _account: account)
        self.pokemons[account] = newPokemon
    }

    init() {
        self.pokemons = {}
    }

}
```
2. Transaction
```

import Pokedex from 0x01

transaction(name: String, type: String, total: Int64, hp: Int64, account: Address) {

    prepare(signer: AuthAccount) {}

    execute {
        Pokedex.addPokemon(name: name, type: type, total: total, hp: hp, account: account)
        log("Pokemon caught!")
    }
}
```

3. Script
```

import Pokedex from 0x01

pub fun main(account: Address): Pokedex.Pokemon {
    return Pokedex.pokemons[account]!
}
```
