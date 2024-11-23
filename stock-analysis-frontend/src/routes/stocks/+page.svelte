<script lang="ts">
  import { onMount } from 'svelte';
  import StockList from '../../components/StockList.svelte';
  import AddStock from '../../components/AddStock.svelte';

  let stocks: { symbol: string; name: string; price: number }[] = [];
  let error: string | null = null;

  onMount(async () => {
    try {
      const res = await fetch('http://localhost:8085/stocks');
      if (!res.ok) {
        throw new Error(`Error fetching stocks: ${res.statusText}`);
      }
      stocks = await res.json();
    } catch (err) {
      error = (err as Error).message;
      console.error('Failed to fetch stocks:', err);
    }
  });

  function setStocks(newStocks: { symbol: string; name: string; price: number }[]) {
    console.log('Updating stocks:', newStocks);
    stocks = [...newStocks];
  }
</script>

{#if error}
  <p>Error: {error}</p>
{/if}
<StockList {stocks} />
<AddStock fetchStocks={() => fetch('http://localhost:8085/stocks').then(res => res.json()).then(setStocks)} />