<script lang="ts">
  import { onMount } from 'svelte';

  import { fade } from 'svelte/transition';
  import { A } from '../data/a';
  import { B } from '../data/b';

  let laws: string[] = [];
  let interval = 50;
  let url = '';
  let dialog = false;

  function pick<T>(array: Array<T>) {
    return array[Math.floor(Math.random() * array.length)];
  }

  function generate() {
    const i = laws.length;
    const newLaw = pick(A) + 'は、' + pick(B) + '。';
    laws.push('');

    for (let j = 1; j <= newLaw.length; ++j) {
      setTimeout(() => {
        laws[i] = newLaw.slice(0, j);
        laws = laws;
      }, j * 100);
    }

    setTimeout(generate, interval);
    if (interval < 3000) {
      interval *= 1.2;
    }
  }

  onMount(() => {
    generate();
    url = location.href;
  });
</script>

<style lang="sass">
  article
    font-family: serif

    @media (min-width: 992px)
      writing-mode: vertical-rl

  .law
    margin: 1em

    user-select: all

    b
      span
        text-combine-upright: all

    i
      font-size: 0

  .btn-about
    position: fixed
    bottom: 1em
    left: 1em
    border: 0
    background: #ffffffe5
    color: #4444ed
    cursor: pointer

  nav
    position: fixed
    top: 50vh
    left: 50vw
    padding: 2em
    min-width: 320px
    background: #fcfdfddd
    box-shadow: 0 0 3px #00000044
    transform: translate(-50%, -50%)

    .right
      float: right
      font-size: 0.5em

    img
      width: 4em

</style>

<svelte:head>
  <title>#ない法律</title>
</svelte:head>

<article>
  {#each laws as law, number}
    <div class="law" transition:fade><b>第<span>{number + 1}</span>条　</b>{law}<i>　#ない法律 {url}</i></div>
  {/each}
</article>

<button class="btn-about" on:click={() => (dialog = !dialog)}>これは何</button>

{#if dialog}
  <nav transition:fade>
    <p>日本の法令を解析して、「Aは、B。」という形の条文を適当に集め、適当につなぎかえました。</p>
    <p class="right"><a href="https://twitter.com/p_km">私が作りました <img src="me.svg" alt="私" /></a></p>
    <p><a href="https://github.com/kissge/legaltech">source</a></p>
    <button on:click={() => (dialog = !dialog)}>とじる</button>
  </nav>
{/if}
