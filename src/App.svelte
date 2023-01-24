<script lang="ts">
	// main.tsより全体タイトル取得
  export let name: string;
  import { onMount } from 'svelte';
  interface Counter {
		title: string;
		count: number;
  }
	// デフォルトで一つのカウンターを定義
  let counters: Counter[] = [{ title: "Counter 1", count: 0 }];
	// 合計値取得用
  let total = 0;

  $:total = counters.reduce((acc, curr) => acc + curr.count, 0);

  onMount(() => {
    counters.forEach(counter => {
      total += counter.count;
		});
  });
	// カウンター追加アクション
  function handleAddCounter() {
    counters.push({ title: "New Counter", count: 0 });
    counters = counters;
  }
	// カウンター削除アクション
  function handleRemoveCounter(index: number) {
    total -= counters[index].count;
    counters.splice(index, 1);
    counters = counters;
  }
	// カウンターの名前が変化した時の挙動
  function handleTitleChange(e: Event, index: number) {
    counters[index].title = (e.target as HTMLInputElement).value;
		counters[index].title = counters[index].title
  }
// toalの数を制御
  function handleCountChange(e: Event, index: number) {
    total -= counters[index].count;
    counters[index].count = Number((e.target as HTMLInputElement).value);
    total += counters[index].count;
  }
</script>

<h1 class="site_name">{name}</h1>
	{#each counters as counter, index}
		<div class="counter">
		<br />
			<div class="counter_area">
				<input class="title" type="text" bind:value={counter.title} on:input={(e) => handleTitleChange(e, index)} placeholder="Enter title" />
				<input type="number" class="num"  bind:value={counter.count} on:input={(e) => handleCountChange(e, index)} />
				<button class="plus" on:click={() => counter.count++}>+</button>
				<button class="minus" on:click={() => counter.count--}>-</button>
				<button class="reset" on:click={() => counter.count = 0}>Reset</button>
				<button class="delete" on:click={() => handleRemoveCounter(index)}>Delete</button>
			</div>
		</div>
	{/each}
		<br />
			<div class="appendix">
				<button class="add_counter" on:click={handleAddCounter}>Add Counter</button>
				<div class="title_holder">
					<p>Your counter lists:</p>
					<ul>
						{#each counters as counter}
							<li>{counter.title}</li>
						{/each}
					</ul>
				</div>
				<p >Total: {total}</p>
			</div>

<style>
	/* 以下css */
	.site_name{
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.counter,.appendix {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin-bottom: 10px;
		padding:10px;
	}
	.counter input[type="text"],
	.counter input[type="number"] {
		padding: 10px;
		margin: 5px 5px;
	}
	.counter button {
		padding: 5px 10px;
		margin: 5px;
	}
	.counter .counter_area {
		padding:10px;
		border: ridge 5px;
		align-items: center;
		width: 800px;
	}
	.counter .title {
		font-weight: bold;
		align-items: center;
		margin-bottom: 10px;
		width: 30%;
	}
	.counter .minus,.plus { width: 5%; }
	.counter .delete,.reset,.num { width: 10%; }

	@media (min-width: 640px) {
		div {
			max-width: none;
		}
  }
</style>