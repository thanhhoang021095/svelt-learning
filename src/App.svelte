<script>
  import Eliza from "elizabot";
  import { beforeUpdate, afterUpdate } from "svelte";
  export let name;

  let div;
  let autoScroll;

  beforeUpdate(() => {
    autoScroll =
      div && div.offsetHeight + div.scrollTop > div.scrollHeight - 20;
  });

  afterUpdate(() => {
    if (autoScroll) div.scrollTo(0, div.scrollHeight);
  });

  const eliza = new Eliza();
  let comments = [{ author: "eliza", text: eliza.getInitial() }];
  function handleKey(e) {
    if (e.key === "Enter") {
      const text = e.target.value;

      if (!text) return;

      comments = comments.concat({ author: "user", text });
      e.target.value = "";
      const reply = eliza.transform(text);
      setTimeout(() => {
        comments = comments.concat({
          author: "eliza",
          text: "...",
          placeholder: true
        });
        setTimeout(() => {
          comments = comments.concat({
            author: "eliza",
            text: reply
          });
        }, 500 + Math.random() * 500);
      }, 200 + Math.random() * 200);
    }
  }

  let todos = [
    { done: false, text: "finish Svelte tutorial" },
    { done: false, text: "build an app" },
    { done: false, text: "world domination" }
  ];

  function add() {
    todos = todos.concat({ done: false, text: "" });
  }

  function clear() {
    todos = todos.filter(t => !t.done);
  }

  $: remaining = todos.filter(t => !t.done).length;
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  .chat {
    display: flex;
    flex-direction: column;
    height: 100%;
    max-width: 320px;
  }

  .scrollable {
    flex: 1 1 auto;
    border-top: 1px solid #eee;
    margin: 0 0 0.5em 0;
    overflow-y: auto;
  }

  article {
    margin: 0.5em 0;
  }

  .user {
    text-align: right;
  }

  span {
    padding: 0.5em 1em;
    display: inline-block;
  }

  .eliza span {
    background-color: #eee;
    border-radius: 1em 1em 1em 0;
  }

  .user span {
    background-color: #0074d9;
    color: white;
    border-radius: 1em 1em 0 1em;
    word-break: break-all;
  }
  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
  .done {
    opacity: 0.4;
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <p>
    Visit the
    <a href="https://svelte.dev/tutorial">Svelte tutorial</a>
    to learn how to build Svelte apps.
  </p>
  <div class="chat">
    <h1>Eliza</h1>

    <div class="scrollable" bind:this={div}>
      {#each comments as comment}
        <article class={comment.author}>
          <span>{comment.text}</span>
        </article>
      {/each}
    </div>

    <input on:keydown={handleKey} />
    <h1>Todos</h1>

    {#each todos as todo}
      <div class:done={todo.done}>
        <input type="checkbox" bind:checked={todo.done} />

        <input placeholder="What needs to be done?" bind:value={todo.text} />
      </div>
    {/each}

    <p>{remaining} remaining</p>

    <button on:click={add}>Add new</button>

    <button on:click={clear}>Clear completed</button>
  </div>
</main>
