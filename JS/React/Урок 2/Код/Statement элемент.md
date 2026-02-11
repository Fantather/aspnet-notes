![[Pasted image 20251210191201.png]]


Функциональный
```JSX
function TextAreaCounter({ text = "Count me!" }) {

  const [value, setValue] = useState(text);

  function onTextChanged(event) {

    setValue(event.target.value);

  }

  return (

    <div>

      <h3>{value.length}</h3>

      <textarea value={value} onChange={onTextChanged} />

    </div>

  );

}
```