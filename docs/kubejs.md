KubeJS 备忘清单
===

[![Github repo](https://badgen.net/badge/icon/Github?icon=github&label)](https://github.com/KubeJS-Mods/KubeJS)
[![WIKI](https://badgen.net/badge/icon/wiki?icon=wiki&label)](https://mods.latvian.dev/books/kubejs)

这个模组可以让你用 JavaScript 语言创建脚本来管理你的服务器、添加新的方块和物品、更改配方、为任务模组添加自定义处理程序等等！
<!--rehype:style=padding-top: 12px;-->

入门
---

### 介绍

KubeJS 是一个多模加载器 Minecraft 模组，它允许您使用 JavaScript 编程语言创建脚本，以使用事件、更改配方、添加并编辑（即将推出！）战利品表，自定义您的世界生成，添加新的方块和物品，或使用与FTB Quests等其他模组的自定义集成以获得更高级的功能！

- [KubeJS 官方文档](https://kubejs.com/)
- [KubeJS Wiki](https://mods.latvian.dev/books/kubejs)
- [KubeJS Wiki 2 (WIP)](https://kubejs.com/wiki/)
<!--rehype:className=style-round-->

注意：KubeJS 5 与 KubeJS 6 API有所不同

### 重新加载脚本
<!--rehype:wrap-class=col-span-2-->

重新加载 server_scripts 下的脚本文件

```bash
$ /reload
```

重新加载 client_scripts 下的脚本文件，快捷键 `F3 + T`

重新加载 startup_scripts 下的脚本文件

```bash
$ /kubejs reload_startup_scripts
```
<!--rehype:className=wrap-text -->

自定义物品
---

### 基本
<!--rehype:wrap-class=wrap-text-->
这是一个 startup_scripts/ 事件
纹理必须放在 `kubejs/assets/kubejs/textures/item/test_item.png`

可以使用 Blockbench 制作模型放在 `kubejs/assets/kubejs/models/item/test_item.json`

```ts
// 监听物品注册事件
onEvent('item.registry', event => {
  // 创建一个物品
  event.create('物品ID', '物品类型')
    .tier('物品等级')
    .attackDamageBaseline(基本伤害)
})
```

### 物品类型
<!--rehype:wrap-class=wrap-text-->

:- | :-
:- | :-
`basic` | 基本
`sword` | 剑（`工具`）
`pickaxe` | 镐（`工具`）
`axe` | 斧（`工具`）
`shovel` | 铲（`工具`）
`hoe` | 锄（`工具`）
`helmet` | 头盔（<pur>护甲</pur>）
`chestplate` | 胸甲（<pur>护甲</pur>）
`leggings` | 护腿（<pur>护甲</pur>）
`boots` | 靴子（<pur>护甲</pur>）

### 基本方法
<!--rehype:wrap-class=wrap-text-->

:- | :-
:- | :-
`maxStackSize(size)` | 最大堆叠数量
`displayName(name)` | 物品名称
`unstackable()` | 不可堆叠
`maxDamage(damage)` | 最大耐久
`burnTime(ticks)` | 燃烧时间
`containerItem(item_id)` | 容器物品
`rarity('rarity')` | 稀有度
`tool(type, level)` | 工具类型
`glow(true/false)` | 是否发光
`tooltip(text...)` | 物品提示
`group('group_id')` | 物品分组
`color(index, colorHex)` | 物品颜色
`texture(customTexturePath)` | 自定义纹理
`parentModel(customParentModel)` | 自定义父模型
`food(foodBuilder => ...)` | 食物

### 物品等级
<!--rehype:wrap-class=wrap-text-->

#### 工具类-等级

:- | :-
:- | :-
`wood` | 木质
`stone` | 石质
`iron` | 铁质
`gold` | 金质
`diamond` | 钻石
`netherite` | 下界合金

#### 护甲类-等级

:- | :-
:- | :-
`leather` | 皮革
`chainmail` | 链甲
`iron` | 铁质
`gold` | 金质
`diamond` | 钻石
`turtle` | 龟壳
`netherite` | 下界合金

### 特定类型方法
<!--rehype:wrap-class=wrap-text-->

#### 工具类-方法

:- | :-
:- | :-
`tier('toolTier')` | 工具等级
`modifyTier(tier => ...)` | [详情](#创建自定义工具和护甲等级)
`attackDamageBaseline(damage)` | 攻击伤害基础值
`attackDamageBonus(damage)` | 攻击伤害加成
`speedBaseline(speed)` | 攻击速度基础值
`speed(speed)` | 攻击速度加成

#### 护甲类-方法

:- | :-
:- | :-
`tier('armorTier')` | 护甲等级
`modifyTier(tier => ...)` | [详情](#创建自定义工具和护甲等级)

### 组/标签

:- | :-
:- | :-
`search` | 搜索
`buildingBlocks` | 建筑
`decorations` | 装饰
`redstone` | 红石
`transportation` |  运输
`misc` | 杂项
`food` | 食物
`tools` | 工具
`combat` | 战斗
`brewing` | 酿造

### 创建自定义工具和护甲等级
<!--rehype:wrap-class=col-span-3-->
所有值都是可选的，默认情况下基于铁质工具等级。

#### 自定义工具等级

```js
onEvent('item.registry.tool_tiers', event => {
  event.add('tier_id', tier => {
    tier.uses = 250
    tier.speed = 6.0
    tier.attackDamageBonus = 2.0
    tier.level = 2
    tier.enchantmentValue = 14
    tier.repairIngredient = '#forge:ingots/iron'
  })
})
```

#### 自定义护甲等级

```js
onEvent('item.registry.armor_tiers', event => {
  // 插槽索引是 [脚, 腿, 身体, 头]
  event.add('tier_id', tier => {
    tier.durabilityMultiplier = 15 // 耐久乘数 皮革 5，链甲 15，铁质 15，金质 7，钻石 33，下界合金 37
    tier.slotProtections = [2, 5, 6, 2] // 每个插槽的防御值
    tier.enchantmentValue = 9 // 附魔值
    tier.equipSound = 'minecraft:item.armor.equip_iron' // 声音
    tier.repairIngredient = '#forge:ingots/iron' // 修复材料
    tier.toughness = 0.0 // 韧性 钻石有 2.0，下界合金 3.0 
    tier.knockbackResistance = 0.0 // 击退抗性
  })
})
```

### 创建自定义食品
<!--rehype:wrap-class=col-span-3-->
这些方法都是可选的，您可以根据需要添加任意数量的方法。

```js
onEvent('item.registry', event => {
  event.create('magic_steak').food(food => {
    food
        .hunger(6)
        .saturation(6)//This value does not directly translate to saturation points gained
          //The real value can be assumed to be:
          //min(hunger * saturation * 2 + saturation, foodAmountAfterEating)
          .effect('speed', 600, 0, 1)
          .removeEffect('poison')
          .alwaysEdible()//Like golden apples
          .fastToEat()//Like dried kelp
          .meat()//Dogs are willing to eat it
          .eaten(ctx => {//runs code upon consumption
            ctx.player.tell('Yummy Yummy!')
              //If you want to modify this code then you need to restart the game.
              //However, if you make this code call a global startup function
              //and place the function *outside* of an 'onEvent'
              //then you may use the command:
              //  /kubejs reload startup_scripts
              //to reload the function instantly.
          })
  })
})
```

另见
---

- [Vue 3.x 官方文档](https://cn.vuejs.org/)
- [Vue Router 4.x 官方文档](https://router.vuejs.org/zh/)
