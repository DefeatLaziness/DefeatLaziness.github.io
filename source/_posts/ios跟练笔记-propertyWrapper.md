---
title: ios跟练笔记-@propertyWrapper
date: 2022-07-19 15:50:17
summary: ios跟练笔记-@propertyWrapper
tags: [ios]
categories: [ios]
---

# 属性包装器 @propertyWrapper

```swift
@propertyWrapper
struct Validation {
    private var text: String
    private var defaultValue: String
    var isValid: Bool {
        !text.isEmpty
    }
    
    var wrappedValue: String {
        get {
            text.isEmpty ? defaultValue : text
        }
        set {
            text = newValue
        }
    }
    
// 只有暴露projectedValue才能在外面使用$name  
    var projectedValue: Self { self }
    
    init(wrappedValue: String, defaultValue: String) {
        text = wrappedValue
        self.defaultValue = defaultValue
    }
}

struct User {
    
    @Validation(defaultValue: "nobody") var name: String = ""
    
    func coding() {
        if ($name.isValid) {
            print("\(name)正在coding")
        } else {
            print("请告诉我谁在coding")
        }
    }
}

var user = User()
user.coding()
user.name = "kim"
user.coding()

// Result
// 请告诉我谁在coding
// kim正在coding
```

