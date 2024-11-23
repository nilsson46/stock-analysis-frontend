<script lang="ts">
    export let fetchStocks: () => Promise<void>;
    let name = '';
    let price = 0;
    let symbol = '';
    let error: string | null = null;
  
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
          const errorData = await response.json();
          throw new Error(errorData.error || `Error adding stock: ${response.statusText}`);
        }
        await fetchStocks(); // Fetch the updated list of stocks
        error = null; // Clear any previous error
        // Clear input fields after successful addition
        name = '';
        price = 0;
        symbol = '';
      } catch (err) {
        error = (err as Error).message;
        console.error('Failed to add stock:', err);
      }
    }
  </script>
  
  <h2>Add Stock</h2>
  <form on:submit|preventDefault={addStock}>
    {#if error}
      <p style="color: red;">{error}</p>
    {/if}
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