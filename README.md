Hack Google Chrome and Make your Dinosaur Immortal
The game can be hacked pretty easily, making your dinosaur not even flinch at the sight of a cactus.

To hack the game, first go the the error message page where your dinosaur is hanging out.

Go ahead and press the space bar to start the game. Once the game starts, right-click and select Inspect” to open up Chrome DevTools, then select the Console tab.

You can also do this by using the Ctrl+Shift+I shortcut, which takes you straight to the Console tab of DevTools.

Open Chrome Console
Make sure you are on the No Internet Connection page.
Right click anywhere on the page and select Inspect.
Go to Console tab. This is where we will enter the commands to tweak the game.
Tweaking Speed
Type the following command in Console and press enter. You can use any other speed in place of 1000.

Runner.instance_.setSpeed(1000)
Immortality
After every command press enter. All the commands are case-sensitive.

We store the original game over function in a variable:

var original = Runner.prototype.gameOver
Then, we make the game over function empty:

Runner.prototype.gameOver = function(){}
Stopping the game after using Immortality

When you want to stop the game, Revert back to the original game over function:

Runner.prototype.gameOver = original
Setting the current score
Let’s set the score to 12345. You can set any other score less than 99999. The current score is reset on game over.

Runner.instance_.distanceRan = 12345 / Runner.instance_.distanceMeter.config.COEFFICIENT
Dino jumping too high?
You can control how high the dino jumps by using the below function. Adjust the value as necessary.

Runner.instance_.tRex.setJumpVelocity(10)
Load earlier comments...
@sudoer-Huatao
sudoer-Huatao commented on Sep 3, 2023
Here's code for a bot that plays:

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
@sudoer-Huatao
sudoer-Huatao commented on Sep 3, 2023
image
try beating me i dare yall

It's actually not that hard. Just use this:

let hackScore = 0;
 
Object.defineProperty(Runner.instance_, 'distanceRan', {
  get: () => hackScore,
  set: (value) => hackScore = value + Math.floor(Math.random() * 1000),
  configurable: true,
  enumerable: true,
});
