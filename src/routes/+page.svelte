<script lang="ts">

  type Counter = {
    id: number;
    name: string;
    count: number;
  };

  
  let counters: Counter[] = [
    { id: 1, name: 'new', count: 0 }
  ];

  const addCounter = ():void => {
    const newId: number = Math.max(...counters.map(c => c.id)) + 1;
    counters = [...counters, { id: newId, name: 'new', count: 0 }];
  };

  const increment = (id: number):void => {
    counters = counters.map(c => c.id === id ? { ...c, count: c.count + 1 } : c);
  };

  const decrement = (id: number):void => {
    counters = counters.map(c => {
      if ( c.id === id && c.count > 0 ) {
        return { ...c, count: c.count - 1 };
      }
      return c;
    });
  };

  const reset = (id: number):void => {
    counters = counters.map(c => c.id === id ? { ...c, count: 0 } : c);
  };

  const removeCounter = (id: number):void => {
    counters = counters.filter(c => c.id !== id);
  };

  const updateName = (id: number, newName: string):void => {
    counters = counters.map(c => c.id === id ? { ...c, name: newName } : c);
  };

  $: totalSum = counters.reduce((sum, counter) => sum + counter.count, 0);
  
</script>

<div>
  <h1>Multiple Counter</h1>
  <div>
    {#each counters as counter (counter.id)}
      <div class="counter">
        <input
          type="text"
          bind:value={counter.name}
          on:input={(e) => updateName(counter.id, e.target.value)}
        />
        <span>{counter.count}</span>
        <button on:click={() => increment(counter.id)} class="increment">+</button>
        <button on:click={() => decrement(counter.id)} class="decrement">-</button>
        <button on:click={() => reset(counter.id)} class="reset">0</button>
        <button on:click={() => removeCounter(counter.id)} class="remove">x</button>
      </div>
    {/each}
        <button on:click={addCounter} class="newCounter">new counter</button>
    <div>
      <strong>title list:</strong> {counters.map(c => c.name).join(', ')}
    </div>
    <div>
      <strong>sum of count:</strong> {totalSum}
    </div>
  </div>
</div>

<style>

  div {
    text-align: center;
  }

  h1 {
    font-size: 4rem;
    font-weight: 500;
    margin: 0;
  }

  .counter {
    background-color: #f7fafc;
    box-shadow: 0 10px 15px -3px rgba(0,0,0,.1), 0 4px 6px -2px rgba(0,0,0,.05);
    max-width: 24rem;
    margin-bottom: 0.5rem;
    margin: 0.5rem auto;
    align-items: center;
    border-radius: 0.25rem;
  }

  input {
    width: 8rem;
    --text-opacity: 1;
    color: #718096;
    border: none;
    font-size: 100%;
    margin: 0 1rem 0 0;
    font-family: inherit;
  }

  span {
    padding-right: 1rem;
    padding-left: 1rem;
    font-size: 1.125rem;
    font-weight: 700;
  }

  .increment {
    background-color: #f56565;
    --text-opacity: 1;
    color: rgba(255,255,255,var(--text-opacity));
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
    padding-left: 0.75rem;
    padding-right: 0.75rem;
    margin-top: 0.25rem;
    margin-bottom: 0.25rem;
    margin-right: -0.35rem;
    font-size: 1.125rem;
    border-top-left-radius: 0.25rem;
    border-bottom-left-radius: 0.25rem;
    cursor: pointer;
    border: none;
  }

  .decrement {
    background-color: #4299e1;
    --text-opacity: 1;
    color: rgba(255,255,255,var(--text-opacity));
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
    padding-left: 0.85rem;
    padding-right: 0.85rem;
    margin-top: 0.25rem;
    margin-bottom: 0.25rem;
    margin-right: -0.35rem;
    font-size: 1.125rem;
    cursor: pointer;
    border: none;
  }

  .reset {
    background-color: #ecc94b;
    --text-opacity: 1;
    color: rgba(255,255,255,var(--text-opacity));
    padding-top: 0.25rem;
    padding-bottom: 0.25rem;
    padding-left: 0.75rem;
    padding-right: 0.75rem;
    margin-top: 0.25rem;
    margin-bottom: 0.25rem;
    font-size: 1.125rem;
    border-top-right-radius: 0.25rem;
    border-bottom-right-radius: 0.25rem;
    cursor: pointer;
    border: none;
  }

  .remove {
    --text-opacity: 1;
    color: #a0aec0;
    border: none;
    cursor: pointer;
    background-color: #f7fafc;
    padding-left: 0.5rem;
    padding-right: 0.5rem;
  }

  .newCounter {
    --text-opacity: 1;
    color: #fff;
    text-align: center;
    --bg-opacity: 1;
    background-color: #68d391;
    width: 24rem;
    border-radius: 0.25rem;
    border: none;
    font-size: 1rem;
  }
  
</style>
