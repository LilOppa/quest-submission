# EA-dapp-course Quest submissions

## Chapter 2 Day 1

1. Frontend is focused on those elements of the site that you see in the browser and interact with directly. Backend is responsible for the functionality of the site and deals with things you cant see.
When you drive a car you use the steering wheel to move it. But what really drives a car is hidden inside - it's his engine.

2. Global styling will effect everything, module will effect only files they are imported in. 

3. 
![Screenshot_1](https://user-images.githubusercontent.com/72570095/195547729-c8f74cd6-6910-49d6-bab0-68c26645c49e.png)

4. https://github.com/LilOppa/beginner-emerald-dapp

## Chapter 2 Day 2

![image](https://user-images.githubusercontent.com/72570095/195830872-79215921-e44c-4077-a4c4-028978f01eca.png)
![image](https://user-images.githubusercontent.com/72570095/195830668-389fd951-6952-4fab-84c3-bd6e19ce8e32.png)
![image](https://user-images.githubusercontent.com/72570095/195830738-69cf0e4e-adb9-4530-85c5-3740c7fcc22b.png)

## Chapter 2 Day 3

![image](https://user-images.githubusercontent.com/72570095/195976179-dec485d7-5ff8-4c2b-b341-e51a38437c41.png)

## Chapter 2 Day 4

```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from '../components/nav'
import { useState } from 'react'

export default function Home() {
  const [newGreeting, setNewGreeting] = useState('')

  function runTransaction() {
    console.log(newGreeting)
  }

  return (
    <div className={styles.container}>
      <Head>
        <title>Emerald DApp</title>
        <meta name="description" content="Created by Emerald Academy" />
        <link rel="icon" href="https://i.imgur.com/hvNtbgD.png" />
      </Head>

      <Nav />

      <main className={styles.main}>
        <h1 className={styles.title}>
          Welcome to my{' '}
          <a href="https://github.com/LilOppa" target="_blank">
            Github!
          </a>
        </h1>
        <p>Hi there!</p>

        <div className={styles.flex}>
          <button onClick={runTransaction}>Run Transaction</button>

          <input
            onChange={(e) => setNewGreeting(e.target.value)}
            placeholder="Hello, Idiots!"
          />
        </div>
      </main>
    </div>
  )
}
```

![image](https://user-images.githubusercontent.com/72570095/195980327-6a40e425-fae5-4394-b1c4-ca1fbb5ff183.png)

## Chapter 4 Day 1

1. We checked if the user is logged in and if not, we called the log in function. For calling this function we imported ```fcl```.
Also we added an ```onClick``` handler to our ```<button>``` for to call our function when we click.
![image](https://user-images.githubusercontent.com/72570095/196143742-7c1fe0d0-3eb1-40ab-9060-a0fadd597429.png)


2. ```fcl.authenticate()``` is a fcl property to log in the user.
```fcl.unauthenticate()``` to log out.

3. We are created a ```config.js``` cuz we need a configuration for connecting our dapp to network(testnet or mainnet) and wallets. 
It connects our dapp to flow testnet and allows our dapp to connect to Blocto and Lilico wallet.

4. ```useEffect``` is a function that runs after every rendering. In our case its to check a value ```user``` variable after every page refreshing.

5. ```import * as fcl from "@onflow/fcl"```

6. ```fcl.currentUser.subscribe(setUser);``` is making sure the user variable retains its value even if the page is refreshed.

## Chapter 4 Day 2

1. 
![image](https://user-images.githubusercontent.com/72570095/196160913-ebb6bc78-de31-4cc8-885f-d3d4d63966f0.png)

2a. 

```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from '../components/nav'
import { useState, useEffect } from 'react'
import * as fcl from '@onflow/fcl'

export default function Home() {
  const [newGreeting, setNewGreeting] = useState('')
  const [greeting, setGreeting] = useState('')
  const [newNumber, setNewNumber] = useState('')

  function runTransaction() {
    console.log(newGreeting)
  }

  async function executeScript() {
    const response = await fcl.query({
      cadence: `
      import HelloWorld from 0xc0cc376c9b7faec3

      pub fun main(): String {
          return HelloWorld.greeting
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    console.log('Response from our script: ' + response)
    setGreeting(response)
  }

  async function executeScriptNum() {
    const response = await fcl.query({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c

      pub fun main(): Int {
          return SimpleTest.number
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    setNewNumber(response)
  }

  useEffect(() => {
    executeScript()
  }, [])

  return (
    <div className={styles.container}>
      <Head>
        <title>Emerald DApp</title>
        <meta name="description" content="Created by Emerald Academy" />
        <link rel="icon" href="https://i.imgur.com/hvNtbgD.png" />
      </Head>

      <Nav />

      <main className={styles.main}>
        <h1 className={styles.title}>
          Welcome to my{' '}
          <a href="https://github.com/LilOppa" target="_blank">
            Github!
          </a>
        </h1>
        <p>Hi there!</p>

        <div className={styles.flex}>
          <button onClick={runTransaction}>Run Transaction</button>

          <input
            onChange={(e) => setNewGreeting(e.target.value)}
            placeholder="Hello, Idiots!"
          />
        </div>

        <p>{greeting}</p>

        <div>
          <button onClick={executeScriptNum}>Read Number</button>
          <output> {newNumber}</output>
        </div>
      </main>
    </div>
  )
}
```
![image](https://user-images.githubusercontent.com/72570095/196356291-53850cae-4682-4fb8-8d80-99c113fdcc78.png)

2b. 
```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from '../components/nav'
import { useState, useEffect } from 'react'
import * as fcl from '@onflow/fcl'

export default function Home() {
  const [newGreeting, setNewGreeting] = useState('')
  const [greeting, setGreeting] = useState('')
  const [newNumber, setNewNumber] = useState('')

  function runTransaction() {
    console.log(newGreeting)
  }

  async function executeScript() {
    const response = await fcl.query({
      cadence: `
      import HelloWorld from 0xc0cc376c9b7faec3

      pub fun main(): String {
          return HelloWorld.greeting
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    console.log('Response from our script: ' + response)
    setGreeting(response)
  }

  async function executeScriptNum() {
    const response = await fcl.query({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c

      pub fun main(): Int {
          return SimpleTest.number
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    setNewNumber(response)
  }

  useEffect(() => {
    executeScript()
    executeScriptNum()
  }, [])

  return (
    <div className={styles.container}>
      <Head>
        <title>Emerald DApp</title>
        <meta name="description" content="Created by Emerald Academy" />
        <link rel="icon" href="https://i.imgur.com/hvNtbgD.png" />
      </Head>

      <Nav />

      <main className={styles.main}>
        <h1 className={styles.title}>
          Welcome to my{' '}
          <a href="https://github.com/LilOppa" target="_blank">
            Github!
          </a>
        </h1>
        <p>Hi there!</p>

        <div className={styles.flex}>
          <button onClick={runTransaction}>Run Transaction</button>

          <input
            onChange={(e) => setNewGreeting(e.target.value)}
            placeholder="Hello, Idiots!"
          />
        </div>

        <p>{greeting}</p>

        <div>
          <output>{newNumber}</output>
        </div>
      </main>
    </div>
  )
}
```
![image](https://user-images.githubusercontent.com/72570095/196356785-97edafae-5566-4c25-b9a3-d34851548875.png)

## Chapter 4 Day 3

1. 
![image](https://user-images.githubusercontent.com/72570095/196365042-b62a239d-0e81-44ba-98c0-1fb355862dea.png)

## Chapter 4 Day 4

```javascript
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import Nav from '../components/nav'
import { useState, useEffect } from 'react'
import * as fcl from '@onflow/fcl'

export default function Home() {
  const [newGreeting, setNewGreeting] = useState('')
  const [greeting, setGreeting] = useState('')
  const [newNumber, setNewNumber] = useState('')
  const [number, setNumber] = useState('')

  async function runTransaction() {
    const transactionId = await fcl.mutate({
      cadence: `
      import HelloWorld from 0xc0cc376c9b7faec3 
  
      transaction(myNewGreeting: String) {
  
        prepare(signer: AuthAccount) {}
  
        execute {
          HelloWorld.changeGreeting(newGreeting: myNewGreeting)
        }
      }
      `,
      args: (arg, t) => [arg(newGreeting, t.String)],
      proposer: fcl.authz,
      payer: fcl.authz,
      authorizations: [fcl.authz],
      limit: 999,
    })

    console.log('Here is the transactionId: ' + transactionId)
    await fcl.tx(transactionId).onceSealed()
    executeScript()
  }

  async function changeNumber() {
    const transactionId = await fcl.mutate({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c 
  
      transaction(myNewNumber: Int) {
  
        prepare(signer: AuthAccount) {}
  
        execute {
          SimpleTest.updateNumber(newNumber: myNewNumber)
        }
      }
      `,
      args: (arg, t) => [arg(newNumber, t.Int)],
      proposer: fcl.authz,
      payer: fcl.authz,
      authorizations: [fcl.authz],
      limit: 999,
    })

    console.log('Here is the transactionId: ' + transactionId)
    await fcl.tx(transactionId).onceSealed()
    executeScriptNum()
  }

  async function executeScript() {
    const response = await fcl.query({
      cadence: `
      import HelloWorld from 0xc0cc376c9b7faec3

      pub fun main(): String {
          return HelloWorld.greeting
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    console.log('Response from our script: ' + response)
    setGreeting(response)
  }

  async function executeScriptNum() {
    const response = await fcl.query({
      cadence: `
      import SimpleTest from 0x6c0d53c676256e8c

      pub fun main(): Int {
          return SimpleTest.number
      }
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [], // ARGUMENTS GO IN HERE
    })

    setNumber(response)
  }

  async function executeScriptArgs() {
    const response = await fcl.query({
      cadence: `
      pub fun main(
        worlds2022: [String]
      ): [String] {
        return worlds2022
      }      
      `, // CADENCE CODE GOES IN THESE ``
      args: (arg, t) => [arg(['T1', 'RNG', 'GenG'], t.Array(t.String))], // ARGUMENTS GO IN HERE
    })

    console.log('worlds2022: ' + response)
  }

  useEffect(() => {
    executeScript()
    executeScriptNum()
    executeScriptArgs()
  }, [])

  return (
    <div className={styles.container}>
      <Head>
        <title>Emerald DApp</title>
        <meta name="description" content="Created by Emerald Academy" />
        <link rel="icon" href="https://i.imgur.com/hvNtbgD.png" />
      </Head>

      <Nav />

      <main className={styles.main}>
        <h1 className={styles.title}>
          Welcome to my{' '}
          <a href="https://github.com/LilOppa" target="_blank">
            Github!
          </a>
        </h1>
        <p>Hi there!</p>

        <div className={styles.flex}>
          <button onClick={runTransaction}>Run Transaction</button>

          <input
            onChange={(e) => setNewGreeting(e.target.value)}
            placeholder="Hello, Idiots!"
          />
        </div>

        <p>{greeting}</p>

        <div>
          <button onClick={changeNumber}>Update Number</button>
          <input
            onChange={(e) => setNewNumber(e.target.value)}
            placeholder=""
          />
        </div>
        <p>{number}</p>
      </main>
    </div>
  )
}
```
![image](https://user-images.githubusercontent.com/72570095/196628175-025f0d0c-ee0c-49dd-b7b4-48232a512cc4.png)

## Chapter 5 Day 1

1. 
```res.status === 0``` - Unknown

```res.status === 1``` - Transaction pending - awaiting finalization

```res.status === 2``` - Transaction finalized - awaiting execution

```res.status === 3``` - Transaction executed - awaiting sealing

```res.status === 4``` - Transaction sealed - transaction complete

```res.status === 5``` - Transaction expired

2. ```setTimeout()``` sets a timer which executes a function once the timer expires.
```setTimeout(() => setTxStatus('Run Transaction'), 5000)```

3. ```fcl.tx(transactionId).subscribe(res => {...}``` function gives us the new status of the transaction every time it changes inside the ```res``` object

4. 
![image](https://user-images.githubusercontent.com/72570095/196890724-6215dc89-bd4a-4b2c-bb36-bcc3ebdcbb70.png)

