# 建立太空游戏 Part 4：加入雷射与碰撞侦测

## 课前测验

[课前测验](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/35?loc=zh_tw)

这堂课中，你会学会如何在 JavaScript 上发射雷射光！我们需要在游戏中新增两项东西：

- **雷射光**：这束雷射会从英雄舰艇垂直往上移动
- **碰撞侦测**，除了完成*射击*这项能力，我们还会新增几项游戏规则：
   - **雷射击中敌人**：被雷射击中的敌人会死亡
   - **雷射击中画面最上方**：雷射击中到画面最上方会消失
   - **敌人碰触到英雄**：敌人与英雄在互相碰触时皆会被摧毁
   - **敌人碰触到画面最下方**：敌人碰触到画面最下方时，该敌人与英雄会被摧毁

简单来说，你这位*英雄*需要在敌人到达画面最下方之前，使用雷射击毁它们。

✅ 做点关于第一款电脑游戏的研究。它有哪些功能？

让我们成为英雄吧！

## 碰撞侦测

我们该如何进行碰撞侦测呢？我们需要将游戏物件视为移动中的矩形。你或许会问为什么？这是因为，绘制游戏物件的图片皆为矩形：它有一组 `x` 与 `y`、`width` 与 `height`。

若两个矩形，举例来说：英雄与敌人*相交*了，这就是一次碰撞。至于会发生什么事取决于游戏规则。要制作碰撞侦测，你需要这些步骤：

1. 取得表达游戏物件为矩形的方法，好比是：

   ```javascript
   rectFromGameObject() {
     return {
       top: this.y,
       left: this.x,
       bottom: this.y + this.height,
       right: this.x + this.width
     }
   }
   ```

2. 一个比较函式，这个函式可以如下：

   ```javascript
   function intersectRect(r1, r2) {
     return !(r2.left > r1.right ||
       r2.right < r1.left ||
       r2.top > r1.bottom ||
       r2.bottom < r1.top);
   }
   ```

## 我们该如何摧毁物件

要摧毁游戏物件，你需要让游戏知道，它不再需要在定期的游戏回圈中绘制该物件。一种方法是在情况发生时，我们可以标记游戏物件为*死亡*，例如：

```javascript
// 碰撞发生
enemy.dead = true
```

接著，你在重新绘制画面前，排除掉这些*死亡*的物件，例如：

```javascript
gameObjects = gameObject.filter(go => !go.dead);
```

## 我们该如何发射雷射

发射雷射可以被视作回应一件键盘事件，并建立往特定方向移动的物件。因此我们需要列出下列步骤：

1. **建立雷射物件**：从英雄舰艇的正上方，建立往画面上方移动的物件。
2. **连接该程式到键盘事件**：我们需要在键盘中挑选一个按键，表达玩家发射雷射光。
3. 在按键按压时，**建立看起来像雷射光的游戏物件**。

## 雷射的冷却时间

在每次按压按键时，好比说*空白键*，雷射光都需要被发射出来。为了让游戏不要在短时间内发射太多组雷射光，我们需要修正它。修法为建立*冷却时间* ── 一个计时器确保雷射在一定期间内只能被发射一次。你可以借由下列方式建立：

```javascript
class Cooldown {
  constructor(time) {
    this.cool = false;
    setTimeout(() => {
      this.cool = true;
    }, time)
  }
}

class Weapon {
  constructor {
  }
  fire() {
    if (!this.cooldown || this.cooldown.cool) {
      // 产生雷射光
      this.cooldown = new Cooldown(500);
    } else {
      // 什么事都不做，冷却中。
    }
  }
}
```

✅ 根据太空游戏系列课程的第一章，回想关于*冷却时间*。

## 建立目标

你会利用上一堂课中现成的程式码(你应该有整理并重构过)做延伸。使用来自 Part II 的档案或是使用 [Part III - Starter](../your-work)。

> 要点：你需要使用的雷射光已经在资料夹中，并已汇入到程式码中。

- **加入碰撞侦测**，建立下列规则给各个雷射碰触到东西的情况：
   1. **雷射击中敌人**：被雷射击中的敌人会死亡
   2. **雷射击中画面最上方**：雷射击中到画面最上方会消失
   3. **敌人碰触到英雄**：敌人与英雄在互相碰触时皆会被摧毁
   4. **敌人碰触到画面最下方**：敌人碰触到画面最下方时，该敌人与英雄会被摧毁
   
## 建议步骤

在你的 `your-work` 子资料夹中，确认档案是否建立完成。它应该包括：

```bash
-| assets
  -| enemyShip.png
  -| player.png
  -| laserRed.png
-| index.html
-| app.js
-| package.json
```

开始 `your_work` 资料夹中的专案，输入：

```bash
cd your-work
npm start
```

这会启动 HTTP 伺服器并发布网址 `http://localhost:5000`。开启浏览器并输入该网址，现在它能呈现英雄以及所有的敌人，但它们还没办法移动！

### 建立程式码

1. **设定表达游戏物件为矩形的方法，以处理碰撞状况** 下列的程式表达 `GameObject` 的矩形呈现方式。编辑你的 GameObject class：

    ```javascript
    rectFromGameObject() {
        return {
          top: this.y,
          left: this.x,
          bottom: this.y + this.height,
          right: this.x + this.width,
        };
      }
    ```

2. **加入程式码来检查碰撞** 这会是新函式来测试两矩形是否相交：

    ```javascript
    function intersectRect(r1, r2) {
      return !(
        r2.left > r1.right ||
        r2.right < r1.left ||
        r2.top > r1.bottom ||
        r2.bottom < r1.top
      );
    }
    ```

3. **加入雷射发射功能**
   1. **加入键盘事件讯息**。 *空白键*要能在英雄舰艇上方建立雷射光。加入三个常数到 Messages 物件中：

       ```javascript
        KEY_EVENT_SPACE: "KEY_EVENT_SPACE",
        COLLISION_ENEMY_LASER: "COLLISION_ENEMY_LASER",
        COLLISION_ENEMY_HERO: "COLLISION_ENEMY_HERO",
       ```

   1. **处理空白键**。 编辑 `window.addEventListener` 的 keyup 函式来处理空白键：

      ```javascript
        } else if(evt.keyCode === 32) {
          eventEmitter.emit(Messages.KEY_EVENT_SPACE);
        }
      ```

    1. **加入监听者**。 编辑函式 `initGame()` 来确保英雄可以在空白键按压时，发射雷射光：

       ```javascript
       eventEmitter.on(Messages.KEY_EVENT_SPACE, () => {
        if (hero.canFire()) {
          hero.fire();
        }
       ```

       建立新的函式 `eventEmitter.on()` 确保敌人碰触到雷射光时，能更新死亡状态：

          ```javascript
          eventEmitter.on(Messages.COLLISION_ENEMY_LASER, (_, { first, second }) => {
            first.dead = true;
            second.dead = true;
          })
          ```

   1. **移动物件**。 确保雷射逐步地向画面上方移动。建立新的 class Laser 延伸自 `GameObject`，你应该有做过：
   
      ```javascript
        class Laser extends GameObject {
        constructor(x, y) {
          super(x,y);
          (this.width = 9), (this.height = 33);
          this.type = 'Laser';
          this.img = laserImg;
          let id = setInterval(() => {
            if (this.y > 0) {
              this.y -= 15;
            } else {
              this.dead = true;
              clearInterval(id);
            }
          }, 100)
        }
      }
      ```

   1. **处理碰撞**。 建立雷射的碰撞规则。加入函式 `updateGameObjects()` 来确认被碰撞的物件。

      ```javascript
      function updateGameObjects() {
        const enemies = gameObjects.filter(go => go.type === 'Enemy');
        const lasers = gameObjects.filter((go) => go.type === "Laser");
      // 雷射击中某物
        lasers.forEach((l) => {
          enemies.forEach((m) => {
            if (intersectRect(l.rectFromGameObject(), m.rectFromGameObject())) {
            eventEmitter.emit(Messages.COLLISION_ENEMY_LASER, {
              first: l,
              second: m,
            });
          }
         });
      });

        gameObjects = gameObjects.filter(go => !go.dead);
      }  
      ```

      记得在 `window.onload` 里的游戏回圈中加入 `updateGameObjects()`。

   4. **设定雷射的冷却时间**，它只能在定期内发射一次。

      最后，编辑 Hero class 来允许冷却：

       ```javascript
      class Hero extends GameObject {
        constructor(x, y) {
          super(x, y);
          (this.width = 99), (this.height = 75);
          this.type = "Hero";
          this.speed = { x: 0, y: 0 };
          this.cooldown = 0;
        }
        fire() {
          gameObjects.push(new Laser(this.x + 45, this.y - 10));
          this.cooldown = 500;
    
          let id = setInterval(() => {
            if (this.cooldown > 0) {
              this.cooldown -= 100;
            } else {
              clearInterval(id);
            }
          }, 200);
        }
        canFire() {
          return this.cooldown === 0;
        }
      }
      ```

到这里，你的游戏有了些功能！你可以测试方向键，使用空白键发射雷射。当你击中敌人时它们会消失。干得漂亮！

---

## 🚀 挑战

加入爆炸特效！ 看看 [Space Art Repo](../solution/spaceArt/readme.txt) 中的档案，试著在雷射击中外星人时，加入爆炸画面。

## 课后测验

[课后测验](https://ashy-river-0debb7803.1.azurestaticapps.net/quiz/36?loc=zh_tw)

## 复习与自学

对游戏中的回圈定时做点实验。当你改变数值时，发生了什么事？阅读更多关于 [JavaScript 计时事件](https://www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/)。

## 作业

[探索碰撞](assignment.zh-cn.md)
