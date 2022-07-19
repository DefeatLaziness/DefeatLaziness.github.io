---
title: ios跟练笔记-protocol
date: 2022-07-19 15:54:28
summary: ios跟练笔记-protocol
tags: [ios]
categories: [ios]
---

# Protocol



## 基本用法 (example 1)

```swift
protocol Combatable {

    var name: String { get }
    var hp: Int { get set }
    var maxHp: Int { get set }
    var attackPower: Int { get set }
    var level: Int { get set }

    init()

}

extension Combatable {
    mutating func relax() {
        hp = maxHp
        print("\(name) 睡饱了，HP补满！")
    }

    mutating func upLevel() {
        maxHp = Int(Double(maxHp) * 1.1)
        hp = maxHp
        attackPower = Int(Double(attackPower) * 1.2)
        level += 1
        print("\(name)升至\(level)级了，现在hp为\(hp),攻击力为\(attackPower)。")
    }

    func attack<T: Combatable>( on target: inout T) {
        target.hp -= self.attackPower
        print("\(name)对\(target.name)造成\(attackPower)点伤害，\(target.name)剩下\(target.hp)点HP。")
    }
}

struct Pokemon: Combatable {
    var name: String = "某个宝可梦"
    var hp: Int = 50
    var maxHp: Int = 50
    var attackPower: Int = 5
    var level: Int = 1
}
struct EvilAlien: Combatable {
    var name: String = "某个邪恶外星人"
    var hp: Int = 60
    var maxHp: Int = 60
    var attackPower: Int = 7
    var level: Int = 1
}

var pikachu = Pokemon(name: "皮卡丘")
var duck = Pokemon(name: "可达鸭")
var alien = EvilAlien()

pikachu.upLevel()
alien.upLevel()
pikachu.attack(on: &alien)
duck.attack(on: &pikachu)
duck.attack(on: &alien)
```



## associatedtype

```swift
    protocol Combatable {
    associatedtype Level

    var name: String { get }
    var hp: Int { get set }
    var maxHp: Int { get set }
    var attackPower: Int { get set }
    var level: Level { get set }

    init()

    mutating func setLevel()
}

extension Combatable {
    mutating func relax() {
        hp = maxHp
        print("\(name) 睡饱了，HP补满！")
    }

    mutating func upLevel() {
        maxHp = Int(Double(maxHp) * 1.1)
        hp = maxHp
        attackPower = Int(Double(attackPower) * 1.2)
//        level += 1
        setLevel()
        print("\(name)升至\(level)级了，现在hp为\(hp),攻击力为\(attackPower)。")
    }

    func attack<T: Combatable>( on target: inout T) {
        target.hp -= self.attackPower
        print("\(name)对\(target.name)造成\(attackPower)点伤害，\(target.name)剩下\(target.hp)点HP。")
    }
}

struct Pokemon: Combatable {
    var name: String = "某个宝可梦"
    var hp: Int = 50
    var maxHp: Int = 50
    var attackPower: Int = 5
    var level: Int = 1
}

extension Combatable where Level == Int {
    mutating func setLevel() {
        level += 1
    }
}
struct Jedi: Combatable {
    mutating func setLevel() {
        let level = level.rawValue + 1
        self.level = Level(rawValue: level) ?? .grandmaster
    }

    enum Level: Int {
        case apprentice0, apprentice1, jedi, master, grandmaster
    }

    var name: String = "某个绝地武士"
    var hp: Int = 100
    var maxHp: Int = 60
    var attackPower: Int = 5
    var level: Level = .apprentice0
}


var warrior = Jedi()
warrior.upLevel()
var pikachu = Pokemon(name: "皮卡丘")
warrior.attack(on: &pikachu)
```



## associatedtype 使用 strideable

```swift
protocol Combatable {
    associatedtype Level: Strideable where Level.Stride == Int

    var name: String { get }
    var hp: Int { get set }
    var maxHp: Int { get set }
    var attackPower: Int { get set }
    var level: Level { get set }

    init()
}

extension Combatable {
    mutating func relax() {
        hp = maxHp
        print("\(name) 睡饱了，HP补满！")
    }

    mutating func upLevel() {
        maxHp = Int(Double(maxHp) * 1.1)
        hp = maxHp
        attackPower = Int(Double(attackPower) * 1.2)
        level = level.advanced(by: 1)
        print("\(name)升至\(level)级了，现在hp为\(hp),攻击力为\(attackPower)。")
    }

    func attack<T: Combatable>( on target: inout T) {
        target.hp -= self.attackPower
        print("\(name)对\(target.name)造成\(attackPower)点伤害，\(target.name)剩下\(target.hp)点HP。")
    }
}

struct Pokemon: Combatable {
    var name: String = "某个宝可梦"
    var hp: Int = 50
    var maxHp: Int = 50
    var attackPower: Int = 5
    var level: Int = 1
}

extension Combatable where Level == Int {
    mutating func setLevel() {
        level += 1
    }
}
struct Jedi: Combatable {

    enum Level: Int, Strideable, CustomStringConvertible {
        typealias Stride = Int

        var description: String {
            switch self {

            case .apprentice0:
                return "幼徒"
            case .apprentice1:
                return "学徒"
            case .jedi:
                return "绝地武士"
            case .master:
                return "大师"
            case .grandmaster:
                return "宗师"
            }
        }

        func advanced(by n: Int) -> Jedi.Level {
            let level = rawValue + n
            return Level.init(rawValue: level) ?? .grandmaster
        }

        func distance(to other: Jedi.Level) -> Int {
            other.rawValue - self.rawValue
        }

        case apprentice0, apprentice1, jedi, master, grandmaster
    }

    var name: String = "某个绝地武士"
    var hp: Int = 100
    var maxHp: Int = 60
    var attackPower: Int = 5
    var level: Level = .apprentice0
}


var warrior = Jedi()
warrior.upLevel()
warrior.upLevel()
warrior.upLevel()
warrior.upLevel()
var pikachu = Pokemon(name: "皮卡丘")
warrior.attack(on: &pikachu)
```



## protocol 搭配其他 protocol

在定义protocol的时候可以为此协议声明遵守其他协议，但不可在此协议的extension声明遵循



## 没有associatedtype 的 protocol 可以直接当做类型使用。

```swift
// 放在属性、参数、回传值的类型中
// 用 & 来结合

protocol HasName {
    var name: String { get }
}

protocol HasAddress {
    var address: String { get }
}

let array: [HasName & HasAddress] = []	
```

