+++
date = '2025-02-25T16:34:37-05:00'
draft = false
author = 'Ben Prothe'
title = 'Ref Shenanigans Kicked my Butt'
tags = ['React', 'Reflections']
+++

![shenanigans](/img/useImperativeHandleDemo/shenanigans.png)

### Intro

We all know you're not actually supposed to try to use refs in a React app. I'm not talking about when you need to save a mutable value without causing rerenders... I'm referring to the "funny business" like attaching a ref to a DOM node or using it to lift up the API of a component. This kind of thing feels weird if you're thinking in the "React Way". It almost feels naughty-- hence, "shenanigans" :joy:.

When I first ran into these "shenanigans" as a Junior Engineer I had a hard time wrapping my head around what was going on or gained. Let's take a look at `useImperativeHandle`, one of the offending concepts. I'll compare its use with a more traditional React use case.

### The Goal

Here's what we're building:
![Selections](/img/useImperativeHandleDemo/app-w-selections.png)
We have two buttons, "add" and "toggle" and any number of elements (there happen to be four in the screenshot). Each element has a "select" input with four options: "none", "green", "red", and "blue".

Here's what happens when I click "toggle":
![Selections Toggled](/img/useImperativeHandleDemo/app-selections-toggled.png)

Pretty simple behavior, right? You can get the job done with a couple of state variables, props, and handlers. It might look something like this:

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
    } else {
      setColor(defaultColor)
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

There's just one limitation... the toggled states will always be in sync with the parent. There's no way for us to have isolated "toggled" lifecycles for the children. The parent owns the "toggle" state and distributes it downstream to the children. You can imagine this problem ramping up in complexity pretty quickly. Let's say you needed dynamically-created components with the ability to maintain their own lifecycles. It's almost a video-game type of problem.

With `useImperativeHandle` I am able to accomplish the following:
First Toggle | Second Toggle
----|----
![Independent Toggle States](/img/useImperativeHandleDemo/app-unsync-1.png) |![Independent Toggle States](/img/useImperativeHandleDemo/app-unsync-2.png)

I added the "red" component and configured it while the "green" component was toggled on. The "red" component's initial state is toggled off. Next time I click "toggle" it turned off "green" and turned on "red". So the benefit here is that you get encapsulation of behaviors.

### Become one with the shenanigans

I hope now that the benefits gained by the `useImperativeHandle` approach are clear. It's not an approach you reach for every day. Only when you need the this level of encapsulation. Here's how we'll handle this in pseudo-code:

```react
/*
We'll start with the child first this time,
which will recieve a ref from the parent.
*/
function ChildComponent({ref}) {
  /*
  Toggle will now be an internal state variable but
  now we'll recieve a ref from the parent as a prop.
  */
  /*
  With useImperativeHandle() we'll expose a "toggle"
  function that will manipulate the states.
  */
  /*
  Compose the UI as we did last time.
  */
}

function ParentComponent(){
  /*
  Store the refs of all the children components in a
  state variable as they're being added.
  */
  /*
  Create a toggle handler that will loop over the
  refs and call the exposed "toggle" API of each component
  */
  /*
  Map over the data as usual and render the children components
  while simultaneously affixing the appropriate refs.
  (this will be the trickiest part)
  */
}
```

Let's start to look at the implementation with the child. The old way of receiving a ref as a prop required the use of `forwardRef`. So you might still see that in the wild. It's what I was familiar with before doing research for this. It turns out, now you can now pass a ref as a prop without bothering with `forwardRef`.

Here's how that looks, starting with the child this time:

```react
const defaultColor = 'gray';
function ChildComponent({ref}) {
  const [toggled, setToggled] = useState(false);
  const [selection, setSelection] = useState(defaultColor);
  const [colorClass, setColorClass] = useState(defaultColor);

  useEffect(() => {
    if(toggled) {
      setColorClass(selection);
    } else {
      setColorClass(defaultColor);
    }
  }, [toggled])

  useImperativeHandle(ref, () => {
    return {
      /*
      this object will contain any method or
      property you want to make public
      */
      toggle() {
        /*
        You could actually do a lot
        more to create interesting toggle behavior here
        but we're keeping this simple.
        */
        setToggled(!toggled);
      }
    }
  })

  return (
    // the markup isn't important for this demo
  )
}
```

Note: In the codesandbox , I pulled out the state variables and effects related to toggling and moved them to a custom hook. That's a good way to keep your code readable and reusable.

Here's the parent:

```react
function ParentComponent() => {
  const [thingsToRender, setThingsToRender] = useState([]);
  // Storing refs in a map for efficient lookups
  const refs = useRef(new Map());

  const handleAddElement = () => {
    /*
    I used a uuid library to generate a string ID
    for every new component to add to the UI. These
    will also serve as key for the refs map.
    */
    setThingsToRender((prev) => [uuid(), ...prev]);
  }

  // Here's where we loop over our refs:
  const handleToggle = () => {
    for(let child of refs.current.values()) {
      // Call the exposed "toggle" method of each child
      child.toggle();
    }
  }

  return (
    <>
      // Markup for toggle and add buttons

      {thingsToRender.map((key) => {
        <ChildComponent
          key={key}
          ref={(node) => {
            refs.current.set(key, node)
            return () => refs.current.delete(key)
            }
          }
        />
      })}
    </>
  )
}
```

That last bit is a tad funky looking. It's the "ref callback" pattern. It's kind of its own entity to wrap your head around so I'll just leave this link **[here](https://react.dev/learn/manipulating-the-dom-with-refs#how-to-manage-a-list-of-refs-using-a-ref-callback)** if you want to dig deeper into how it works.

### Conclusions

So that's it. I wouldn't call it the most elegant of solutions. The React team might come up with something cleaner some day. Here's a **[link](https://codesandbox.io/p/devbox/gcrg2d)** to the codesandbox so you can look at the fleshed out code and test the UI. Like I said earlier, I used a custom hook to manage the toggle states. Additionally, I used TypeScript and Tailwinds for some added fun and flare.

Note - There is a TS flag in `CanvasRefs.tsx` that I'm annoyed I haven't been able to figure out so hit me up if you have any ideas. Copilot is way more confused by it than I am and only makes things worse with its suggestions :joy:. Also bear with me on file names and file structure. I kept trying to refactor and the sandbox kept crashing or duplicating files.

Peace, friends!

Ben
