<script>
  import Phaser from 'phaser'
  import fragment from 'svelte-fragment'
  import { Game, Scene, Text } from 'svelte-phaser'
  import Breakout from './Breakout.svelte'
  import LoadingBar from './LoadingBar.svelte'

  function preload(scene) {
    scene.load.atlas('assets', 'assets/breakout.png', 'assets/breakout.json')
  }

  Phaser.Math.Clamp()
</script>

<style>
  :global(body) {
    margin: 0;
    position: relative;
    width: 100%;
    height: 100%;
    cursor: grab
  }
</style>

<Game
  width={window.innerWidth}
  height={window.innerHeight}
  physics={{ default: 'arcade' }}
  scale={{ mode: Phaser.Scale.FIT, autoCenter: Phaser.Scale.FIT }}>
  <Scene key="main" {preload}>
    <template use:fragment slot="loading" let:progress>
      <LoadingBar x={window.innerWidth/2} y={window.innerHeight/2} {progress} />
    </template>
    <Breakout />
  </Scene>
</Game>