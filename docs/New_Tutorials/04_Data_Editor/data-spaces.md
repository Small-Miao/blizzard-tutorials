---
title: 数据空间
authors:
    - duckies
---

# 数据空间

一种把相关数据聚合进单个文件的方法，这样就能以更有条理、也更省时的方式把它复制到其他地图中。

本教程默认你这辈子都没见过 `.xml`，一听到任何稍微复杂点的东西就想直接掀桌子。

## 第 1 步：将地图保存为组件

依次点击 `File->Save As->`，选择保存为 `.SC2Components`，而不是常见的 `.SC2map` 文件。

![](./data-spaces/7a7c2e19fe350e07246ebce2a2c5dedc1aed007f.png)

然后找到新建出来的地图文件夹，并进入 `Base.SC2Data` -> `GameData` 文件夹。

![](./data-spaces/d24fac19396186c19b2f8ad4bcc82d76f5dcfcf9.png)

把地图保存为组件后，你就能更清楚地看到一张地图实际上由哪些部分构成。在 `Base.SC2Data` -> `GameData` 文件夹中，你可以看到自己做过的数据修改。

![](./data-spaces/ce9096c22e61b820b7dc61f3c591c5834f791d27.png)

只要把两张地图都保存为组件，你就可以通过复制粘贴这些 xml 文件，轻松把数据从一张地图搬到另一张地图。

## 第 2 步：创建数据空间

在这里，你会看到数据修改已经按类别拆分开了，比如单位、Actor、效果、行为等等。

我们要创建一个单独的 `.xml` 文件，把所有相关数据都放进去，而不是分散在多个不同文件中。

你可以新建一个 xml 文件，也可以直接复制任意一个现有 xml 文件，然后把它重命名为你希望这个数据空间使用的名称，打开它，删除里面的全部内容，再粘贴如下内容：

```xml
<?xml version="1.0" encoding="us-ascii"?>
<Catalog>
</Catalog>
```

![](./data-spaces/0bef7176c983a4cdf417abecd0644374b927c1e8.png)


## 第 3 步：让编辑器识别数据空间

我们需要在 `Base.SC2Data` 文件夹里新建一个名为 `GameData.xml` 的 xml 文件（创建方式和第 2 步中新建前一个 xml 文件时相同）。把下面内容粘贴进去，不过要记得把 catalog path 改成你自己的数据空间文件名。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Includes>
    <Catalog path="GameData/AbilHeroChainStrike.xml"/>
</Includes>
```

![](./data-spaces/b666aef6f73b61cb64b79e542e37e57d5e918321.png)

## 第 4 步：把数据转移进数据空间

现在一切都已经设置好了。重新载入地图后，你会看到数据空间菜单里已经出现了我们刚创建的数据空间。

![](./data-spaces/d1c42025870c4bbd280c23d03083545f8b0b71da.png)

选中你想移动到数据空间中的数据项，右键并选择 `"Move ..."`.

![](./data-spaces/10db63dea886ac1099c752e12ebbc47dfa4c85aa.png)

!!! info

    如果你正在使用数据集合功能，那么移动一个数据集合时，也会一并移动它所包含的所有元素。

![](./data-spaces/cf0678e56d9cb6f5d63c4ffad849a215ac6a1eef.png)

等所有内容都移动完成后，保存地图并检查这个数据空间。你会发现，之前在编辑器中移动进去的所有信息，现在都方便地集中到了这一个文件里，也就更容易分享了。

如果要把它导入到新的地图或模组中，我们需要先把那张地图或模组也保存为组件，再把自己的数据空间文件移动到 `\Base.SC2Data\GameData` 文件夹，并像第 3 步那样在 `GameData.xml` 文件中创建或添加 catalog path。

## 第 5 步：为什么全都变成了 `(Unknown)` 和 `(Unnamed)`

原因是文本是单独存储的。如果我们把数据空间加入另一张地图或模组，就会发现所有描述和数据字段名称都丢了，或者变得乱七八糟。因为游戏客户端除了英文外还有很多语言，所以所有展示给用户看的文本都是单独存储的。在地图的组件文件夹里，你可以找到本地化文件夹；以我的情况为例，它是 `enUS.SC2Data`。这里保存着数据条目对应的文本文件。我们必须手动把相关本地化文件中的数据添加或合并到目标地图里，才能正确导入数据空间。

![](./data-spaces/4465131cbdddc78685fd15dfe02108fe0097d014.png)

## 示例地图

* [BounceTestMap.SC2Map](./data-spaces/BounceTestMap.SC2Map)

**附加链接**

- Mapster 的 Github 教程链接：<https://sc2mapster.github.io/mkdocs/data/>
