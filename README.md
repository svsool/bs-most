# bs-most

BuckleScript bindings for [Most.js](https://github.com/cujojs/most)

```reason
let add x y => x + y;
let isEven x => x mod 2 === 0;

/**
 * Logs:
 * 2
 * 4
 * 6
 * 8
 * 10
 * Stream completed
 **/
Most.(
  fromList [0,1,2,3,4,5,6,7,8]
  |> map (add 2)
  |> filter isEven
  |> observe (fun x => Js.log x)
  |> Js.Promise.then_ (fun _ => {
    Js.log "Stream completed";
    Js.Promise.resolve ()
  })
);
```

## How do I use it?

The Most.js [API docs](https://github.com/cujojs/most/blob/master/docs/api.md) are probably the best source of how to use it. This library attempts to provide very low-level bindings, except for what's noted in the **What's different** section.

I try to provide comments next to each binding and am working on flushing out tests, so you can refer to those if you're confused.

You can also shoot me an e-mail or DM on the Reason discord (username: @lilactown). 

## What's missing

1. `generate` - Reason doesn't have generator support yet
2. `transduce` - Awkward to use in Reason/OCaml without a solid transducer impl (maybe coming soon?? 😉)

## What's different

1. Predicates that normally return "truthy" values, now only accept returning booleans
2. `sample` is variadic so we offer `sample2`, `sample3`... `sample6`
3. `fromEvent` - DOM events are supported, but generic `EventEmitter`'s are not yet
4. `fromList` is a function that creates a stream from a Reason `List` type, e.g. `fromList([1, 2, 3])`
4. `unfold` is modeled a bit differently - instead of taking in a JS object `{ done, value, seed }`, it takes an `option (value, seed)`.
Example:
```reason
/* emits 2,4,6 then completes */
Most.unfold
  (
    fun seed =>
      if (seed < 4) {
        let nextSeed = seed + 1;
        Some (seed * 2, nextSeed)
      } else {
        None
      }
  )
  1;
```

# Build
```
npm run build
```

# Build + Watch

```
npm run watch
```

# Testing

```
npm run test
```
