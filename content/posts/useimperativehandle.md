+++
date = '2025-02-25T16:34:37-05:00'
draft = false
author = 'Ben Prothe'
title = 'Ref Shenanigans Kicked my Butt'
tags = ['React', 'Reflections']
+++

![shenanigans](/img/useImperativeHandleDemo/shenanigans.png)

We all know you're not actually supposed to try to use refs in a React app. You can freely use the `useRef()` hook to create a persisting value that doesn't cause re-renders as it changes... but I'm talking about the "funny business" like attaching a ref to a DOM node or using it to lift up the API of a component. This kind of thing feels weird if you're thinking in the "React Way". It almost feels naughty. Hence "ref shenanigans".

When I was learning our codebase at my first developer job I ran into these shenanigans quite a bit and I had a hard time wrapping my head around them. I'm going to do my best to illustrate what you gain from using one of these shenanigans, `useImperativeHandle()`, verses a more traditional React approach.

To demonstrate, here's what we're building:
![Selections](/img/useImperativeHandleDemo/app-w-selections.png)
We have two buttons, "add" and "toggle" and any number of elements (there happen to be four in the screenshot). Each element has a "select" input with four options: "none", "green", "red", and "blue".

Here's what happens when I click toggle:
![Selections](/img/useImperativeHandleDemo/app-selections-toggled.png)

Pretty simple behavior. You would can accomplish this much with a couple of state variables, props, and handlers.

That might look something like this:

```react
function Parent() {
  const [toggled, setToggled] = useState(false);
  const [thingsToRender, setThingsToRender] = useState([])

  // pretend I filled in the rest...

  return (
    <>
      <button onClick={handleAddThing}>Add Thing</button>
      <button onClick={() => setToggled(!toggled)}>Toggle</button>
      {
        thingsToRender.map((thing) => {
          <ColorChangingComponent key={thing.id} toggled={toggled}/>
        })
      }
    </>
  )
}

const defaultColor = 'gray'

function ColorChangingComponent({toggled}) {
  const [color, setColor] = useState(defaultColor)
  const [selection, setSelection] = useState(defaultColor)

  useEffect(() => {
    if(toggled) {
      setColor(selection)
    }
  }, [toggled])

  return (
    <div className={${color}}>
      <select onChange={(e) => setSelection(e.target.value)}>
        <option value={defaultColor}>None</option>
        <option value='green'>Green</option>
        <option value='red'>Red</option>
        <option value='blue'>Blue</option>
      </select>
    </div>
  )
}
```

This solution folows React's "declarative" intent. The parent says, "I just became toggled and since you have a prop inquiring, I'm letting you know so you can respond accordingly." The data flows downhill to the leaf nodes that care about the data. If you need the data to live globally, you can access it from a store. So hows that? Easy to read? Easy to debug? I know right!

**I NEED TO ACTUALLY TEST THE ABOVE SOLUTION AND SEE IF IT WORKS. IF IT DOES, I HAVE NO IDEA WHY WE USED USEIMPERATIVEHANDLE**

**AFTER THAT, NEXT STEP TO DO IS ADD ANOTHER TYPE OF ELEMENT THAT DOES SOMETHING ELSE WHEN ITS TOGGLED**
