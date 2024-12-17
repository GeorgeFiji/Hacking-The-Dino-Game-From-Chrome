# Hack Google Chrome Dinosaur Game and Make Your Dinosaur Immortal

The game can be hacked pretty easily, making your dinosaur not even flinch at the sight of a cactus.

## Getting Started
To hack the game, first go to the error message page where your dinosaur is hanging out.

1. Press the **space bar** to start the game.
2. Once the game starts, right-click and select **Inspect** to open Chrome DevTools, then select the **Console** tab.
3. Alternatively, you can use the shortcut **Ctrl+Shift+I** (Windows/Linux) or **Cmd+Option+I** (Mac) to open the Console tab directly.

---

## Tweaks and Hacks

### 1. **Tweaking Speed**
Set the speed of the game by typing the following command in the Console and pressing Enter. Replace `1000` with any desired speed:
```javascript
Runner.instance_.setSpeed(1000);
```

### 2. **Immortality**
Make your dinosaur immortal by overriding the `gameOver` function:

#### Store the Original Game Over Function:
```javascript
var original = Runner.prototype.gameOver;
```

#### Make the Game Over Function Empty:
```javascript
Runner.prototype.gameOver = function(){};
```

#### Revert Back to Original:
When you want to stop the game, revert to the original function:
```javascript
Runner.prototype.gameOver = original;
```

### 3. **Setting the Current Score**
Set a custom score (e.g., `12345`). Replace `12345` with your desired score (maximum is `99999`):
```javascript
Runner.instance_.distanceRan = 12345 / Runner.instance_.distanceMeter.config.COEFFICIENT;
```

### 4. **Adjusting Jump Height**
Control how high the dinosaur jumps. Adjust the value (default is `10`) as necessary:
```javascript
Runner.instance_.tRex.setJumpVelocity(10);
```

---

## Automating Gameplay with a Bot
The following script automates the game, allowing the dinosaur to avoid obstacles automatically:

```javascript
function TrexRunnerBot() {
  const makeKeyArgs = (keyCode) => {
    const preventDefault = () => void 0;
    return {keyCode, preventDefault};
  };
  const upKeyArgs = makeKeyArgs(38);
  const downKeyArgs = makeKeyArgs(40);
  const startArgs = makeKeyArgs(32);
  if (!Runner().playing) {
    Runner().onKeyDown(startArgs);
    setTimeout(() => {
      Runner().onKeyUp(startArgs);
    }, 500);
  }
  function conquerTheGame() {
    if (!Runner || !Runner().horizon.obstacles[0]) return;
    const obstacle = Runner().horizon.obstacles[0];
    if (obstacle.typeConfig && obstacle.typeConfig.type === 'SNACK') return;
    if (needsToTackle(obstacle) && closeEnoughToTackle(obstacle)) tackle(obstacle);
  }
  function needsToTackle(obstacle) {
    return obstacle.yPos !== 50;
  }
  function closeEnoughToTackle(obstacle) {
    return obstacle.xPos <= Runner().currentSpeed * 18;
  }
  function tackle(obstacle) {
    if (isDuckable(obstacle)) {
      duck();
    } else {
      jumpOver(obstacle);
    }
  }
  function isDuckable(obstacle) {
    return obstacle.yPos === 50;
  }
  function duck() {
    Runner().onKeyDown(downKeyArgs);
    setTimeout(() => {
      Runner().onKeyUp(downKeyArgs);
    }, 500);
  }
  function jumpOver(obstacle) {
    if (isNextObstacleCloseTo(obstacle))
      jumpFast();
    else
      Runner().onKeyDown(upKeyArgs);
  }
  function isNextObstacleCloseTo(currentObstacle) {
    const nextObstacle = Runner().horizon.obstacles[1];
    return nextObstacle && nextObstacle.xPos - currentObstacle.xPos <= Runner().currentSpeed * 42;
  }
  function jumpFast() {
    Runner().onKeyDown(upKeyArgs);
    Runner().onKeyUp(upKeyArgs);
  }
  return {conquerTheGame: conquerTheGame};
}
let bot = TrexRunnerBot();
let botInterval = setInterval(bot.conquerTheGame, 2);
```

---

## Custom Score Manipulation
To manipulate the score dynamically:
```javascript
let hackScore = 0;

Object.defineProperty(Runner.instance_, 'distanceRan', {
  get: () => hackScore,
  set: (value) => hackScore = value + Math.floor(Math.random() * 1000),
  configurable: true,
  enumerable: true,
});
```

---

## Notes
- These hacks are for educational purposes and temporary use. The modifications reset when the page is refreshed.
- Enjoy exploring the tweaks but avoid using them maliciously!

Happy Hacking! 🎉

