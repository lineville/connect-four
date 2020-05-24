<script>
  import { onMount, onDestroy } from 'svelte'
  import Navbar from './components/Navbar.svelte'
  import ConnectFour from './components/ConnectFour.svelte'

  onMount(() => {
    console.log('App mounted')
  })
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <Navbar />
  <h1>Connect Four</h1>
  <ConnectFour />
</main>
