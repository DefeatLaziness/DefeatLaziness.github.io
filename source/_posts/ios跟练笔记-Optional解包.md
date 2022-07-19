---
title: ios跟练笔记-Optional解包
date: 2022-07-19 15:53:23
summary: ios跟练笔记-Optional解包
tags: [ios]
categories: [ios]
---

# Optional 解包

```swift
let name: String? = nil

// !解包    这种方式要必须确定name有值的时候才可使用，不然会引起报错闪退
print(name!)

// ??解包    提供默认值，解包成功用原值，否则用默认值
print(name ?? "kim") // kim

// if let 解包
if let name = name {
    // 进入到此视为有值
    print("我的名字是\(name)")
}
// 此外不确定name能否解包成功
// if let 多条件同时满足才能解包成功
let lastName: String? = "Lee"
if let lastName = lastName, let name = name, (lastName + name).count > 5 {
    print("解包成功")
}



// guard let 解包 必须在闭包内使用
func introduceMyself(_ name: String?) {
    guard let name = name else {
//        此处可以处理解包失败的情况
        return
    }
//    解包成功可以往下继续进行
    print(name)
}

introduceMyself(name)   //无名
introduceMyself("kim")  //kim


// Optional Chaining : ?.
struct Person {
    let name: String
    let tel: String?
}

let profile: [Person?] = [
    Person(name: "k", tel: nil),
    Person(name: "i", tel: "123456"),
    Person(name: "m", tel: nil)
]

let phone1 = profile.map(\.?.tel)
print(phone1) //    [nil, Optional("123456"), nil]
let phone2 = profile.compactMap(\.?.tel)
print(phone2) //    ["123456"]
```

