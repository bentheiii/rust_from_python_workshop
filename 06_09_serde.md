The `serde` library provides an abstraction over serialization and deserialization of arbitrary data.

```rust
#[derive(Serialize, Deserialize, Debug)]
struct Point {
    x: i32,
    y: i32,
    metadata: HashMap<String, String>
}
let point = Point { x: 1, y: 2, metadata: HashMap::from([("name".to_string(), "joe".to_string())]) };

// Convert the Point to a JSON string.
let serialized = serde_json::to_string(&point).unwrap();
println!("{}", serialized); // {"x":1,"y":2,"metadata":{"name":"joe"}}

let serialized = "x=3\ny=4\n[metadata]\nname=\"joey\"";

// Convert the JSON string back to a Point.
let deserialized: Point = toml::from_str(&serialized).unwrap();
println!("{deserialized:?}");  // Point { x: 3, y: 4, metadata: {"name": "joey"} }
```