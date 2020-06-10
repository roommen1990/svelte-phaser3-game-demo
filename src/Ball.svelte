<script>
  import { Sprite, ArcadePhysics, ArcadeCollider } from 'svelte-phaser'

  export let instance = undefined

  function handlePaddleCollide(event) {
    const { self, other: paddle } = event.detail
    console.log("BALL MADE CONTACT")
    if (self.x < paddle.x) {
      const diff = paddle.x - self.x
      self.body.setVelocityX(-10 * diff)
      console.log("BALL HITS LEFT")
    } else if (self.x > paddle.x) {
      const diff = self.x - paddle.x
      self.body.setVelocityX(10 * diff)
       console.log("BALL HITS RIGHT")
    } else {
      self.body.setVelocityX(2 + Math.random() * 8)
      console.log("BALL HITS MIDDLE")
    }
  }
</script>

<Sprite bind:instance name="ball" texture="assets" frame="ball2">
  <ArcadePhysics collideWorldBounds bounce={1} />
  <ArcadeCollider with="paddle" on:collide={handlePaddleCollide} />
</Sprite>