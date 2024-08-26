<!-- src/lib/components/Purchase.svelte -->
<script>
  export let purchasedBooks = [];
</script>

<div class="cart">
  <h2>Shopping Cart</h2>
  {#if purchasedBooks.length === 0}
    <p>Your cart is empty.</p>
  {:else}
    <ul>
      {#each purchasedBooks as book}
        <li>{book.title} - ${book.price}</li>
      {/each}
    </ul>
  {/if}
</div>

<style>
  .cart {
    border: 1px solid #ddd;
    padding: 1rem;
    margin-top: 1rem;
    border-radius: 8px;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    margin-bottom: 0.5rem;
  }
</style>