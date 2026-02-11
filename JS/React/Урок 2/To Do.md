Дописать обновление обработчиков





```JSX
import { useState } from 'react'

import reactLogo from './assets/react.svg'

import viteLogo from '/vite.svg'

import './App.css'

import React from 'react'

  

function App() {

  return (

    <div>

      <h1>

        First Lesson

      </h1>

    <TextAreaCounter />

    </div>

  )

}

  

export default App;

  

class TextAreaCounter extends React.Component {

  render() {

    const text = this.props.text;

    return (

      <div>

        <h3>{text.length}</h3>

        <textarea defaultValue={text}></textarea>

      </div>

    )

  }

}

  

TextAreaCounter.defaultProps = {text: 'Brave new world'}
```