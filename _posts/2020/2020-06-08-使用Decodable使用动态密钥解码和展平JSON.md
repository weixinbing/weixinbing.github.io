---
title: 使用 Decodable 使用动态密钥解码和展平 JSON
categories: [Swift]
published: true
topmost: false
---

```swift
// ***
// Add generic parameter clause
struct DecodedArray<T: Decodable>: Decodable {

    // ***
    typealias DecodedArrayType = [T]

    private var array: DecodedArrayType

    // Define DynamicCodingKeys type needed for creating decoding container from JSONDecoder
    private struct DynamicCodingKeys: CodingKey {

        // Use for string-keyed dictionary
        var stringValue: String
        init?(stringValue: String) {
            self.stringValue = stringValue
        }

        // Use for integer-keyed dictionary
        var intValue: Int?
        init?(intValue: Int) {
            // We are not using this, thus just return nil
            return nil
        }
    }

    init(from decoder: Decoder) throws {

        // Create decoding container using DynamicCodingKeys
        // The container will contain all the JSON first level key
        let container = try decoder.container(keyedBy: DynamicCodingKeys.self)

        var tempArray = DecodedArrayType()

        // Loop through each keys in container
        for key in container.allKeys {

            // ***
            // Decode T using key & keep decoded T object in tempArray
            let decodedObject = try container.decode(T.self, forKey: DynamicCodingKeys(stringValue: key.stringValue)!)
            tempArray.append(decodedObject)
        }

        // Finish decoding all T objects. Thus assign tempArray to array.
        array = tempArray
    }
}
```

```json
{
  "Vegetable": [{ "name": "Carrots" }, { "name": "Mushrooms" }],
  "Spice": [{ "name": "Salt" }, { "name": "Paper" }, { "name": "Sugar" }],
  "Fruit": [
    { "name": "Apple" },
    { "name": "Orange" },
    { "name": "Banana" },
    { "name": "Papaya" }
  ]
}
```

```swift
struct Food: Decodable {

    let name: String
    let category: String

    enum CodingKeys: CodingKey {
        case name
    }

    init(from decoder: Decoder) throws {

        let container = try decoder.container(keyedBy: CodingKeys.self)

        // Decode name
        name = try container.decode(String.self, forKey: CodingKeys.name)

        // Extract category from coding path
        category = container.codingPath.first!.stringValue
    }
}
```

```swift
let jsonString = """
{
  "Vegetable": [
    { "name": "Carrots" },
    { "name": "Mushrooms" }
  ],
  "Spice": [
    { "name": "Salt" },
    { "name": "Paper" },
    { "name": "Sugar" }
  ],
  "Fruit": [
    { "name": "Apple" },
    { "name": "Orange" },
    { "name": "Banana" },
    { "name": "Papaya" }
  ]
}
"""

let jsonData = Data(jsonString.utf8)

// Define DecodedArray type using the angle brackets (<>)
let decodedResult = try! JSONDecoder().decode(DecodedArray<[Food]>.self, from: jsonData)

// Perform flatmap on decodedResult to convert [[Food]] to [Food]
let allFood = decodedResult.flatMap{ $0 }

dump(allFood)
```

[原文](https://swiftsenpai.com/swift/decode-dynamic-keys-json/)
