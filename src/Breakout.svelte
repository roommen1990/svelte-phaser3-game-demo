<script>
  import { onGameEvent, onInputEvent, getScene } from 'svelte-phaser'
  import Paddle from './Paddle.svelte'
  import Ball from './Ball.svelte'
  import Block from './Block.svelte'

  const scene = getScene()
  scene.physics.world.setBoundsCollision(true, true, true, false)

  let isBallLaunched = false
  let paddle
  let ball
  let blocks = []
  let brickLength = 100;

  setup()

  onGameEvent('step', () => {
    if (!isBallLaunched) {
      ball.setPosition(paddle.x, paddle.y - 48)
    }

    if (ball.y > 800) {
      console.log("BALL HITS BOTTOM");
      ball.body.setVelocity(0);
      // reset ball and raise level later
      isBallLaunched = false
    }

    if (blocks.length === 0) {
      console.log("WINNER WINNER CHICKEN DINNER");
      ball.body.setVelocity(0)
      if(brickLength < 100) {
        alert("congrats, increasing bricks now");
        brickLength += 10;
      }
      else {
        alert("you win");
        brickLength = 10
      }
      setup()
    }
  })

  // launch ball on click
  onInputEvent('pointerdown', () => {
    if (!isBallLaunched) {
      console.log("BALL IS BEING LAUNCHED");
      isBallLaunched = true
      ball.body.setVelocity(-75, -600)
    }
  })

  // setup game
  function setup() {
    isBallLaunched = false
    blocks = Array.from({ length: brickLength }).map((_, index) => {
      // possible sprites to use for block
      const blockFrames = [
        'blue2',
        'silver2',
        'red2',
        'particle3',
        'green2',
        'yellow2',
        'purple2',
      ]

      return {
        x: (index % 10) * 64,
        y: 10 * Math.floor(index / 10) * 3.2,
        // each row uses same sprite
        frame: blockFrames[Math.floor(index / 10)],
        key: index,
      }
    })
  }
</script>

<Paddle bind:instance={paddle} x={500} y={800} />
<Ball bind:instance={ball} />
{#each blocks as block (block.key)}
  <Block
    x={block.x + 500}
    y={block.y + 100}
    frame={block.frame}
    onBallHit={() => {
      blocks = blocks.filter(b => b !== block)
    }} />
{/each}