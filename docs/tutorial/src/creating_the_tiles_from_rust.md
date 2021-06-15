# Creating The Tiles From Rust

The tiles in the game should have a random placement. We'll need to add the <`rand` dependency to
`Cargo.toml` for the randomization.

```toml
[dependencies]
sixtyfps = "0.0.6"
rand = "0.8" # Added
```

What we'll do is take the list of tiles declared in the .60 language, duplicate it, and shuffle it.
We'll do so by accessing the `memory_tiles` property through the Rust code. For each top-level property,
a getter and a setter function is generated - in our case `get_memory_tiles` and `set_memory_tiles`.
Since `memory_tiles` is an array in the `.60` language, it is represented as a [`Rc<dyn sixtyfps::Model>`](https://sixtyfps.io/docs/rust/sixtyfps/trait.model).
We can't modify the model generated by the .60, but we can extract the tiles from it, and put it
in a [`VecModel`](https://sixtyfps.io/docs/rust/sixtyfps/struct.vecmodel) which implements the `Model` trait.
`VecModel` allows us to make modifications and we can use it to replace the static generated model.

We modify the main function like so:

```rust
{{#include main_tiles_from_rust.rs}}
```

Note that we clone the `tiles_model` because we'll use it later to update the game logic.

Running this gives us a window on the screen that now shows a 4 by 4 grid of rectangles, which can show or obscure
the icons when clicking. There's only one last aspect missing now, the rules for the game.

<video autoplay loop muted playsinline src="https://sixtyfps.io/blog/memory-game-tutorial/creating-the-tiles-from-rust.mp4"></video>
