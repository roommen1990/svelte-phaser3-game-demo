<script>
  import Phaser from 'phaser'
  import { Rectangle, Container } from 'svelte-phaser'
  import { tweened } from 'svelte/motion'
  import { bounceInOut } from 'svelte/easing'

  const barWidth = 400

  export let x
  export let y
  export let progress

  const tweenedProgress = tweened(progress, {
    duration: 100,
    easing: bounceInOut,
  })

  $: $tweenedProgress = progress
</script>

<Container {x} {y} width={barWidth} height={50}>
  <!-- outer bar -->
  <Rectangle
    width={barWidth}
    height={50}
    fillColor={0x777777}
    {...$$restProps} />
  <!-- inner bar -->
  <Rectangle
    x={-barWidth / 2}
    originX={0}
    originY={0.5}
    width={Phaser.Math.Clamp(barWidth * $tweenedProgress, 10, barWidth)}
    height={100}
    fillColor={0xbbbbbb} />
</Container>