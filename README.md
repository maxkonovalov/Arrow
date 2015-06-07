# Arrow <--
Dead simple Swift JSON Parsing

## Features

- [x] Leaves your models clean
- [x] Implicitely casts json values to the right types in your model
- [x] Does not crash if json key is not there, nor returns nil, it simple doesn't do anything
- [x] Simple
- [x] Extensible
- [x] Easy to use

## Why Another Swift JSON Parsing Library?


Well the answer is prettry simple. All the others are simply not good enough.
Or at at least none of them meet our requirements for what makes a truely good Json Library :

- Most of them force us to subclass our models. The fact that we get data as JSON representation is a DETAIL and should not leak in our app architecture (right uncle bob?! :p) So there is no reason that using a particular library to parse Json would force us to come back to our pretty models and subclass them with some obscure class. And what if your models aldready subclass something, say NSManagedObject? well you're pretty much screwed :p

- Some use overly complex obscure functional chaining operator overloading voodoo magic

- Most need us to explicitely cast the Json value to the correposnding type on our model. We think this is crazy! the type is already there in our model class, can't we just implicitly cast it?


## Ok I'm sold, Now show me the code

### Spoiler Alert <3
---
```swift
identifier <-- json["id"]
name <-- json["name"]
stats <== json["stats"]
```

### Swift Model
-
```swift
struct Profile {
    var identifier = 0
    var name = ""
    var stats = Stats()
}
```

### JSON File
--
```json
{
    "id": 15678,
    "name": "John Doe",
    "stats": {
        "numberOfFriends": 163,
        "numberOfFans": 10987
    }
}
```
### Usual Swift JSON Parsing (Chaos)
-
```swift
var profile = Profile()
if let id = json["id"] as? Int {
    profile.identifier = id
}  
if let name = json["name"] as? String {
    profile.name = name
}
if let statsJson = json["stats"] as? AnyObject {
    if let numberOfFans = statsJson["numberOfFans"] as? Int {
        profile.stats.numberOfFans = numberOfFans
    }
    if let numberOfFriends = statsJson["numberOfFriends"] as? Int {
        profile.stats.numberOfFriends = numberOfFriends
    }
}
```
### With Arrow --> Sanity preserved
```swift
extension Profile:ArrowParsable {
    init(json: JSON) {
        identifier <-- json["id"]
        name <-- json["name"]
        stats <== json["stats"]
    }
}
```


## Integration
- Step 1 - copy paste Arrow.swift in your Xcode Project
- Step 2 - Create you model parsing extension like so : "Profile+Arrow.swift"
```swift
// Profile+Arrow.swift
extension Profile:ArrowParsable {
    init(json: JSON) {
        identifier <-- json["id"]
        name <-- json["name"]
        stats <== json["stats"]
    }
}
```
- Step 3 - Use it:
```swift
let profile = Profile(json: json)
```
- Step 4 - Ther is no step 4

 
## How Does that work

<-- Operator is for all

