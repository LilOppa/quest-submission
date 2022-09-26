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

## Chapter 3 Day 1

1. Structs can be copied, owerwrited and created them you want. You can't do that with Resources.
2. If we use Struct to transfer a NFT it may be lost(if the developer made a mistake in the code). That's why we need to use Resource!
3. ```create```
4. No. We are can create Resource only inside the contract
5. ```Jacob```
6. 

```
pub fun createJacob(): @Jacob {
  let myJacob <- create Jacob()
  return <- myJacob
}
```

## Chapter 3 Day 2

```

pub contract BasketballFantasy {

    pub var arrayOfLineUp: @[LineUp]
    pub var dictionaryOfLineUp: @{UInt: LineUp}

    pub resource LineUp {
        pub let number: UInt
        init() {
            self.number = 23
        }
    }

    pub fun addLineUpArray(lineup: @LineUp) {
        self.arrayOfLineUp.append(<- lineup)
    }

    pub fun addLineUpDictionary(lineup: @LineUp) {
        let key = lineup.number
        
        let oldLineUp <- self.dictionaryOfLineUp[key] <- lineup
        destroy oldLineUp
    }

    pub fun removeLineUpArray(index: Int): @LineUp {
        return <- self.arrayOfLineUp.remove(at: index)
    }

    pub fun removeLineUpDictionary(key: UInt): @LineUp {
        let lineup <- self.dictionaryOfLineUp.remove(key: key)!
        return <- lineup
    }

    init() {
        self.arrayOfLineUp <- []
        self.dictionaryOfLineUp <- {}
    }

}

```

## Chapter 3 Day 3

1.


```

pub contract BasketballFantasy {

    pub var dictionaryOfLineup: @{UInt: Lineup}

    pub resource Lineup {
        pub let name: String
        pub let team: String
        init(_name: String, _team: String) {
            self.name = _name
            self.team = _team
        }
    }

    pub fun getReference(key: UInt): &Lineup {
        return (&self.dictionaryOfLineup[key] as &Lineup?)!
    }

    init() {
        self.dictionaryOfLineup <- {
            23: <- create Lineup(_name: "LeBron James", _team: "Los Angeles Lakers"), 
            7: <- create Lineup(_name: "Kevin Durant", _team: "Brooklyn Nets")
        }
    }
}

```

2.

```

import BasketballFantasy from 0x01

pub fun main(): String {
  let ref = BasketballFantasy.getReference(key: 23)
  return ref.name // returns "LeBron James"
}

```

3. This is very fast and easy when you want to access a resource to view or update its fields.

## Chapter 3 Day 4

1. 
- to expose data from resources and structures to another resources/structs
- access restrictions: allows you to only expose certain data to certain users

2. 

```

pub contract BasketballFantasy {

    pub resource interface ILineup {
      pub let player: String
    }

    pub resource Lineup: ILineup {
        pub let player: String
        pub let team: String
        init() {
            self.player = "Kevin Durant"
            self.team = "Brooklyn Nets"
        }
    }

    pub fun noInterface() {
      let lineup: @Lineup <- create Lineup()
      log(lineup.team) // Brooklyn Nets

      destroy lineup
    }

    pub fun yesInterface() {
      let lineup: @Lineup{ILineup} <- create Lineup()
      log(lineup.team) // ERROR: `member of restricted type is not accessible: team`

      destroy lineup
    }
}

```

3.

```

pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      pub fun changeGreeting(newGreeting: String): String // fixed
    }

    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String // fixed

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting 
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "apple" // fixed
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") 
      log(newGreeting)
    }
}

```

## Chapter 3 Day 5

```pub(set) var a: String```

read scope: AREA 1-4

write scope: AREA 1-4


```pub var b: String```

read scope: AREA 1-4

write scope: AREA 1


```access(contract) var c: String```

read scope: AREA 1, 2, 3

write scope: AREA 1


```access(self) var d: String```

read scope: AREA 1

write scope: AREA 1


```pub fun publicFunc() {}```

AREA 1-4


```access(contract) fun contractFunc() {}```

AREA 1, 2, 3


```access(self) fun privateFunc() {}```

AREA 1

## Chapter 4 Day 1

1. Almost all data is stored inside the account:
- contract code (one or more)
- account storage 

2. 

```/storage/``` - access only for account owner, all data is stored here
```/public/``` - access for everybody
```/private/``` - access for account owner and the people that the owner gives access to

3.

``` .save()``` - saves data in account storage
``` .load()``` - takes data out of our account storage
``` .borrow()``` - get a reference to the resource in our account storage

4. We cannot save something to our account storage inside of a script because scripts use only to read state!

6. You cannot save something to my account cuz only I have access tp path /storage/

A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it:

```

import BasketballFantasy from 0x01
transaction() {
  prepare(signer: AuthAccount) {
    let saveLineupResource <- BasketballFantasy.createLineup()
    signer.save(<- saveLineupResource, to: /storage/MyLineupResource)

    let loadLineupResource <- signer.load<@BasketballFantasy.Lineup>(from: /storage/MyLineupResource) 
                                ?? panic("A `@BasketballFantasy.Lineup` resource does not live here.")
    log(loadLineupResource.player) // "Kevin Durant"

    destroy loadLineupResource
  }

  execute {

  }
}

```

A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource:

```

import BasketballFantasy from 0x01
transaction() {
  prepare(signer: AuthAccount) {
    let saveLineupResource <- BasketballFantasy.createLineup()
    signer.save(<- saveLineupResource, to: /storage/MyLineupResource)

    let borrowLineupResource = signer.borrow<&BasketballFantasy.Lineup>(from: /storage/MyLineupResource)
                          ?? panic("A `@BasketballFantasy.Lineup` resource does not live here.")
    log(borrowLineupResource.number) // 7
  }

  execute {

  }
}

```

## Chapter 4 Day 2

1. The ```.link()``` function links our resource to the ```/public/``` path so other can access it to read

2. When we define a new resource interface, we expose only the fields or methods we need. So we restrict the reference to use that resource when we create ```.link()``` the resource to the ```/public/``` path

3.

Contract

```
pub contract BasketballFantasy {

  pub resource interface ILineup {
    pub var number: UInt64
  }

  pub resource Lineup: ILineup {
    pub let player: String
    pub var number: UInt64

    pub fun changeNumber(newNumber: UInt64) {
      self.number = newNumber
    }

    init() {
      self.player = "Kevin Durant"
      self.number = 7
    }
  }

    pub fun createLineup(): @Lineup {
    return <- create Lineup()
  }

}
```

Transaction - save the resource to storage and link it to the public with the restrictive interface.

```
import BasketballFantasy from 0x05
transaction() {
  prepare(signer: AuthAccount) {
    signer.save(<- BasketballFantasy.createLineup(), to: /storage/MyLineupResource)

    signer.link<&BasketballFantasy.Lineup{BasketballFantasy.ILineup}>(/public/MyLineupResource, target: /storage/MyLineupResource)
  }

  execute {

  }
}
```

Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.

```
import BasketballFantasy from 0x05
pub fun main(address: Address): String {
  let publicCapability: Capability<&BasketballFantasy.Lineup{BasketballFantasy.ILineup}> =
    getAccount(address).getCapability<&BasketballFantasy.Lineup{BasketballFantasy.ILineup}>(/public/MyLineupResource)

  let lineupResource: &BasketballFantasy.Lineup{BasketballFantasy.ILineup} = publicCapability.borrow() 
                          ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  // ERROR!!! member of restricted type is not accessible: player
  return lineupResource.player
}
```

Run the script and access something you CAN read from. Return it from the script.

```
import BasketballFantasy from 0x05
pub fun main(address: Address): UInt64 {
  let publicCapability: Capability<&BasketballFantasy.Lineup{BasketballFantasy.ILineup}> =
    getAccount(address).getCapability<&BasketballFantasy.Lineup{BasketballFantasy.ILineup}>(/public/MyLineupResource)

  let lineupResource: &BasketballFantasy.Lineup{BasketballFantasy.ILineup} = publicCapability.borrow() 
                          ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

    return lineupResource.number
}
```

## Chapter 4 Day 3

1.
- If we wanted to have a lot of NFTs, we would have to remember all the storage paths we gave it - its inefficient
- Nobody can mint us NFT

2. When you have resources inside of resources, you must declare a ```destroy``` function that manually destroys those "nested" resources with the ```destroy``` keyword

3. 
Idea #1: Do we really want everyone to be able to mint an NFT?
- We need to have a way to decide who can mint cuz the collection is limited. To do this, you need to implement the mint through a resource that will carry the rights of the mint. And only those who have this resource will be able to mint.

Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?
- We need to add a new function to our Collection to return references to our NFTs - it is faster and easier

## Chapter 4 Day 4

```javascript
pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid

      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  // This is a resource interface that allows us to only exposes `deposit`, `getIDs` and `borrowNFT` to public
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }
  
  // This is a resource stores a dictionary called `ownedNFTs`,
  // that maps an `id` to the `NFT` with that `id`
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}
    
    // Allows us to deposit an NFT to our Collection
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }
    
    // Allows us to withdraw an NFT from our Collection
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // Returns an array of all the NFT ids in our Collection
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // This is a function for to get the reference to one of our NFTs
    // from the our Collection
    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }
  
  // This is a function that allows us to save a `Collection` to our account storage
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }
  
  // This is a resource that allows anyone who holds it to mint NFTs
  pub resource Minter {

    // This is a function that allows us to mint a new NFT resource contains a name,
    // favouriteFood and luckyNumber
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }
    
    // Allow us to create a new Minter resource
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
