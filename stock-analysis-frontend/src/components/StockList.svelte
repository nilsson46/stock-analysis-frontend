<script lang="ts">
    export let stocks: { symbol: string; name: string; price: number }[];
  
    $: console.log(stocks);
  </script> 
  
  <h1>Stocks</h1>
  <ul>
    {#each stocks as stock}
      <li>
        {stock.symbol} - {stock.name}: {stock.price}
      </li>
    {/each}
  </ul>