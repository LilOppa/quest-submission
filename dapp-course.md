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
