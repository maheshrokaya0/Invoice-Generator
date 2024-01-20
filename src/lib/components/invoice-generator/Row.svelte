<script lang="ts">
    import { createEventDispatcher } from 'svelte';
  
    export let row: {
      id: number;
      name: string;
      quantity: number;
      rate: number;
      amount: number;
    };
  
    const dispatch = createEventDispatcher();
  
    $: row.amount = row.quantity * row.rate;
  
    function handleQuantityChange(event: InputEvent) {
      row.quantity = parseFloat((event.target as HTMLInputElement).value);
      dispatch('update');
    }
  
    function handleRateChange(event: InputEvent) {
      row.rate = parseFloat((event.target as HTMLInputElement).value);
      dispatch('update');
    }
  
    function deleteRow() {
      dispatch('delete', { id: row.id });
    }
  </script>
  
  <tr>
    <td class="whitespace-nowrap px-4 py-2 font-medium text-gray-900">
      <textarea bind:value={row.name} class="w-full text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1"></textarea>
    </td>
    <td class="whitespace-nowrap px-4 py-2 font-medium text-gray-900">
      <input type="number" bind:value={row.quantity} on:input={handleQuantityChange} class="w-full text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" min="0"/>
    </td>
    <td class="whitespace-nowrap px-4 py-2 font-medium text-gray-900">
      <input type="number" bind:value={row.rate} on:input={handleRateChange} class="w-full text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" min="0"/>
    </td>
    <td class="whitespace-nowrap px-4 py-2 font-medium text-gray-900">${row.amount}</td>
    <td class="whitespace-nowrap px-4 py-2 font-medium text-gray-900"><button on:click={deleteRow} class="inline-block rounded bg-red-600 px-4 py-2 text-xs font-medium text-white hover:bg-red-700">Delete</button></td>
  </tr>
  