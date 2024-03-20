```
cargo new    //creates a new project
cargo build  //builds the executable
cargo run    //lazy compile + runs the executable
cargo check  //checks if the code still compiles
cargo build --release // + optimizations opted when benchmarking

// array fixed in size
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5]; // [3, 3, 3, 3, 3];

println!("{:?}", a); // {:?} is the valid formatter for vec/hashmap

```