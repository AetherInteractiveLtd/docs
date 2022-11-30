# Events

Events are a core library that is used all over Tina, incluiding Networking. Events are useful when it comes to binding callbacks to a unique identifier and emitting data to it.

```ts
EventEmitter<EventsInterface>.when(x: keyof EventsInterface); // Returns an Event listener
```

Using the `.when(...)` method is necessary, since it returns an Event listener where the functions are binded and invoked from. Something you might find interesting is creating multiple chains of actions to be performed asynchronously. 

### Conditions

Conditions are available to be done within events, that way you *condition* (pun intended) the next-in-queue callback. This is done using the `.condition()` method. You can see an implementation on Conditions with this method.

The main idea behind the Conditions implementation within Events is that, the data sent to the Event is also used in the condition as well.

### Do

The "do's" are the way to bind functions as actions to be performed. The dos are the ones who are actually ran when the event is emitted.

```ts
interface Events {
    onEvent: (message: string) => string;
}

const Event = new EventEmitter<Events>
    .when("onEvent")
    .do((message: string) => {
        return `${message}, world!`
    })
    .do((concatenated: string) => print(concatenated));

Event.emit("onEvent", "Hello");
```