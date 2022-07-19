---
title: ios跟练笔记-Enum
date: 2022-07-19 15:51:58
summary: ios跟练笔记-Enum
tags: [ios]
categories: [ios]
---

# 枚举 Enum

```swift
// 基本使用
enum Color: String {
    case pink
    case red
}

print("我选择的颜色是\(Color.red.rawValue)") // 我选择的颜色是red

// CaseIterable 是为了能使用allCases属性
// CustomStringConvertible 为了使输出的allCases更可读，且赋予值
// 以下CustomStringConvertible遵守与否的输出对比
// [spring, summer, 秋天, winter]				
// [__lldb_expr_19.Season.spring,__lldb_expr_19.Season.summer,__lldb_expr_19.Season.fall, __lldb_expr_19.Season.winter]
enum Season: String, CaseIterable, CustomStringConvertible {
    var description: String {
        rawValue
    }

    case spring, summer, fall = "秋天", winter
}

let season = Season.summer
print(season)																	// summer
print(Season.allCases)												// [spring, summer, 秋天, winter]

print(String(describing: season)) 						// summer
print(String(describing: Season.allCases)) 		// [spring, summer, 秋天, winter]

// 两个例子
enum MembershipLevel : String, CaseIterable, CustomStringConvertible {

    case ordinary, silver, gold

    enum permission: String, CaseIterable {
        case watchOldMovies, skipAds, downloadMovies, watchNewMovies
    }

    var expense: Int {
        switch self {
        case .ordinary:
            return 0
        case .silver:
            return 250
        case .gold:
            return 400
        }
    }

    var description: String {
        rawValue
    }

    func canDo(_ permission: permission) -> Bool {
        switch self {
        case .ordinary:
            return permission == .watchOldMovies
        case .silver:
            return permission != .watchNewMovies
        case .gold:
            return true
        }
    }

    static let allLevel = MembershipLevel.allCases.map(\.rawValue).joined(separator: "·")

}

print("你好，请选择你想要加入的会员类型：\(MembershipLevel.allLevel)")

let myMemberShip = MembershipLevel.allCases.randomElement()!
print("欢迎加入\(myMemberShip)")
MembershipLevel.permission.allCases.forEach {
    let isAllowed = myMemberShip.canDo($0)
    print(isAllowed ? "您可以\($0.rawValue)" : "\(myMemberShip) 无法 \($0.rawValue)")
}

enum Gender: CaseIterable, CustomStringConvertible, RawRepresentable {
    case male, female, other(desc: String = "其他")

    init(rawValue: String) {
        switch rawValue {
        case "male":
            self = .male
        case "female":
            self = .female
        default:
            self = .other(desc: rawValue)
        }
    }

    var rawValue: String {
        switch self {
        case .male:
            return "男"
        case .female:
            return "女"
        case .other(let desc):
            return "其他:" + desc
        }
    }

    var description: String {
        rawValue
    }

    static var allCases: [Gender] = [.male, .female, .other()]


}

print(Gender.female)
print(Gender.other(desc: "酷儿"))
print(Gender.other())
print(Gender.allCases)
```

