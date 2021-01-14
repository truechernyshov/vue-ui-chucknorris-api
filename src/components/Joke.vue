<template>
  <div class="joke">
    <blockquote>
      <p>{{ text }}</p>
    </blockquote>
  </div>
</template>

<script>
export default {
  name: 'Joke',
  props: {
    text: String
  }
}
</script>

<style lang="scss">
.joke {
  blockquote {
    text-align: justify;
    background: #f9f9f9;
    border-left: 10px solid #ccc;
    margin: 1.5em 0;
    padding: .5em 10px;
    quotes: "\201C" "\201D";

    &::before {
      color: #ccc;
      content: open-quote;
      font-size: 4em;
      line-height: .1em;
      margin-right: .25em;
      vertical-align: -.4em;
    }

    p {
      display: inline;
    }
  }
}
</style>
