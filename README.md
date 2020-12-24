# react-useStateref
useRef and UseState together!

Inspiration:

Many StackOverflow questions that people struggling to get the current state

- https://stackoverflow.com/questions/54069253/usestate-set-method-not-reflecting-change-immediately
- https://stackoverflow.com/questions/60438537/usestate-shows-previous-value-always
- https://stackoverflow.com/questions/57847594/react-hooks-accessing-up-to-date-state-from-within-a-callback
- https://stackoverflow.com/questions/55874789/using-react-hooks-why-are-my-event-handlers-firing-with-the-incorrect-state

In React when functions accessing the state they receive the state from the moment
the function defined - not the current state.

So if the state changed, your functions and effects my use older state.

Using `useRef`, can solve it because it have always one value. But when you update the Ref it's not re-render.

See the example code:

```js
function Hello(){
	var [counter,setCounter]=useState(0)
	function increment(){
			setCount(counter+1)
			alert(counter) // will show 0 since the state not updated yet.			
	}
	useEffect(()=>{
		alert(counter) // Whatever is the current state. It always alert 0
		return ()=>{
				alert(counter) // On unmount it still alert 0, even if you called increment many times.
		}
	},[])
	return <div>
					Current number: {counter}
					<button onClick={increment}>
						Increment
					</button>
				</div>
}
```

To solve it I created a new hook `useStateRef`

See it in action:

```js
function Hello(){
	var [counter,setCounter,counterRef]=useStateRef(0)
	function increment(){
			setCount(counter+1)
			alert(counterRef.current) // will show 1
	}
	useEffect(()=>{
		alert(counterRef.current) // Always show the last value
		return ()=>{
				alert(counterRef.current) // // Always show the last value
		}
	},[])
	return <div>
					Current number: {counter}
					<button onClick={increment}>
						Increment
					</button>
				</div>
}
```

It's fully support the `useState` API, so you can change your `useState` to `useStateRef` and it will not break your app.

# Contribution

I'm open to your contribution. Better documentation or whatever your like. Just open a PR