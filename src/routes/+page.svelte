<script>
  import Book from '$lib/components/Book.svelte';
  import Purchase from '$lib/components/Purchase.svelte';
  import { writable } from 'svelte/store';

  let title = '';
  let price = 0;
  let description = '';
  
  const books = writable([]);
  let purchasedBooks = [];

  function addBook() {
    books.update(currentBooks => [
      ...currentBooks,
      { title, price, description }
    ]);
    
    // Reset form fields
    title = '';
    price = 0;
    description = '';
  }

  function purchaseBook(event) {
    const newBook = event.detail;
    purchasedBooks = [...purchasedBooks, newBook];
  }
</script>

<style>
  h1 {
    color: purple;
    text-align: center;
  }
  form {
    margin: auto;
    width: 30rem;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  label, input, textarea {
    width: 100%;
    margin-bottom: 0.5rem;
  }
  button {
    width: 100%;
    padding: 0.5rem;
    background-color: purple;
    color: white;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  }
  .books {
    margin-top: 2rem;
  }

  .no-books-message {
    grid-column: 1 / -1;
    text-align: center;
    color: #d00c0c;
    font-style: italic;
    padding: 2rem;
    background-color: #f8f8f8;
    border-radius: 8px;
  }
</style>

<h1>SvelteKit Bookstore</h1>

<form on:submit|preventDefault={addBook}>
  <div>
    <label for="title">Title</label>
    <input type="text" id="title" bind:value={title} required/>
  </div>
  <div>
    <label for="price">Price</label>
    <input type="number" id="price" bind:value={price} min="0" step="0.01" required/>
  </div>
  <div>
    <label for="description">Description</label>
    <textarea rows="3" id="description" bind:value={description} required></textarea>
  </div>
  <button type="submit">Add Book</button>
</form>

<div class="books">
  {#if $books.length === 0} 
    <p class="no-books-message"> 
        No books in stock. 
    </p> 
{:else} 
    {#each $books as book}
        <Book bookTitle={book.title} bookPrice={book.price} bookDescription={book.description} on:addtocart={purchaseBook}/>
    {/each}
{/if}
</div>
<Purchase {purchasedBooks} />
