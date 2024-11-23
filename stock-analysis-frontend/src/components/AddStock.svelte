<script lang="ts">
    export let stocks: { symbol: string; name: string; price: number }[];
    export let setStocks: (stocks: { symbol: string; name: string; price: number }[]) => void;
  
    let name = '';
    let price = 0;
    let symbol = '';
  
    async function addStock() {
      try {
        const response = await fetch('http://localhost:8085/addstock', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ name, price, symbol })
        });
        if (!response.ok) {
          throw new Error(`Error adding stock: ${response.statusText}`);
        }
        const newStock = await response.json();
        setStocks([...stocks, newStock]);
      } catch (err) {
        console.error('Failed to add stock:', err);
      }
    }
  </script>
  
  <h2>Add Stock</h2>
  <form on:submit|preventDefault={addStock}>
    <label>
      Name:
      <input type="text" bind:value={name} />
    </label>
    <label>
      Price:
      <input type="number" bind:value={price} />
    </label>
    <label>
      Symbol:
      <input type="text" bind:value={symbol} />
    </label>
    <button type="submit">Add</button>
  </form>