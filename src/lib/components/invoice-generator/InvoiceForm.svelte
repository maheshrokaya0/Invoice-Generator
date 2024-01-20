<script lang="ts">
    import Row from './Row.svelte';
    import axios from 'axios';
  
    let invoiceNumber: number = 1;
    let issuedDate: string = "";
    let dueDate: string = "";
    let clientName: string = "";
    let clientAddress: string = "";
    let clientCityStateZip: string = "";
    let yourName: string = "";
    let yourAddress: string = "";
    let yourCityStateZip: string = "";
  
    let discount: number = 0;
    let tax: number = 0;
  
    let note: string = "";
  
    let rows: { id: number; name: string; quantity: number; rate: number; amount: number }[] = [
      {
        id: 1,
        name: "",
        quantity: 0,
        rate: 0.00,
        amount: 0.00,
      },
    ];
  
    function addNewRow() {
      const newRow = { id: Date.now(), name: "", quantity: 0, rate: 0, amount: 0 };
      rows = [...rows, newRow];
    }
  
    function recalculate() {
      subTotal = rows.reduce((sum, r) => sum + r.amount, 0);
      afterDiscount = subTotal - (discount / 100) * subTotal;
      afterTax = afterDiscount + (tax / 100) * afterDiscount;
      total = afterTax;
    }
  
    function handleRowDelete(event: CustomEvent<{ id: number }>) {
      const { id } = event.detail;
      rows = rows.filter((row) => row.id !== id);
    }
  
    let subTotal: number = 0;
    let afterDiscount: number = 0;
    let afterTax: number = 0;
    let total: number = 0;
  
    $: subTotal = rows.reduce((sum, r) => sum + r.amount, 0);
    $: afterDiscount = subTotal - (discount / 100) * subTotal;
    $: afterTax = afterDiscount + (tax / 100) * afterDiscount;
    $: total = afterTax;
  
    async function generateInvoice() {
      const invoiceData = {
        invoiceNumber,
        issuedDate,
        dueDate,
        clientName,
        clientAddress,
        clientCityStateZip,
        yourName,
        yourAddress,
        yourCityStateZip,
        rows,
        subTotal,
        discount,
        tax,
        total,
        note,
      };
  
      try {
        const response = await axios.post('http://localhost:3000/api/generate-invoice', invoiceData, {
          headers: {
            'Content-Type': 'application/json',
          },
          responseType: 'blob',
        });

        const blob = new Blob([response.data], { type: 'application/pdf' });
        const url = URL.createObjectURL(blob);

        const a = document.createElement('a');
        a.href = url;
        a.download = 'invoice.pdf';
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
  
        if (response.status === 200) {
          console.log("Invoice Generated Successfully");
        } else {
          console.error("Failed to generate invoice");
        }
      } catch (error) {
        console.error("Error : ", error);
      }
    }
  </script>
  
  <div class="max-w-3xl mx-auto">
    <div class="p-5">
      <div>
        <label for="invoice-number">Invoice Number : # </label>
        <input type="number" bind:value={invoiceNumber} class="text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" id="invoice-number" min="1">
      </div>
      <div class="mt-3">
        <label for="issued-date">Issued : </label>
        <input type="date" id="issued-date" bind:value={issuedDate} class="text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1">
      </div>
      <div class="mt-5">
        <label for="issued-date">Due : </label>
        <input type="date" id="due-date" bind:value={dueDate} class="text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1">
      </div>

      <div class="w-full sm:w-7/12 mt-5">
        <h3 class="font-semibold">Client Details</h3>
        <input type="text" bind:value={clientName} placeholder="Client's Name" class="w-full text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required>
        <input type="text" bind:value={clientAddress} placeholder="Client's Address" class="w-full mt-2 text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required>
        <input type="text" bind:value={clientCityStateZip} placeholder="City, State Zip" class="w-full mt-2 text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required>
      </div>

      <div class="w-full sm:w-7/12 mt-5">
        <h3 class="font-semibold">Your Details</h3>
        <input type="text" bind:value={yourName} placeholder="Your Name" class="w-full mb-2 text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required> 
        <input type="text" bind:value={yourAddress} placeholder="Your Address" class="w-full mb-2 text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required>     
        <input type="text" bind:value={yourCityStateZip} placeholder="City, State Zip" class="w-full mb-2 text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" required>
      </div>
    </div>
  </div>

  <div class="overflow-x-auto md:max-w-3xl mt-5 mx-auto">
    <div class="p-5 w-[48rem]">
      <table class="min-w-full divide-y-2 divide-gray-200 bg-white text-sm">
        <thead>
          <tr class="bg-black">
            <th class="whitespace-nowrap px-4 py-2 font-medium text-white">Items</th>
            <th class="whitespace-nowrap px-4 py-2 font-medium text-white">Qty</th>
            <th class="whitespace-nowrap px-4 py-2 font-medium text-white">Rate</th>
            <th class="whitespace-nowrap px-4 py-2 font-medium text-white">Amount</th>
            <th class="px-4 py-2"></th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200">
          {#each rows as row}
            <Row {row} on:delete={handleRowDelete} on:update={recalculate} />
          {/each}
        </tbody>
      </table>
    </div>
  </div>

  <div class="max-w-3xl mx-auto mt-2">
    <div class="w-full px-5 flex">
      <button on:click={addNewRow} class="w-36 mx-auto rounded bg-indigo-600 px-4 py-2 text-sm font-semibold font-medium text-white hover:bg-indigo-700">ADD ROW</button>
    </div>
  </div>

  <div class="mt-5 max-w-3xl mx-auto">
    <div class="p-5">  
      <div class="mt-5">
        <p><strong>SubTotal : ${subTotal.toFixed(2)}</strong></p>
        <p class="mt-2"><strong>Discount: </strong><input type="number" bind:value={discount} class="text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" min="0"/>%</p>
        <p class="mt-2"><strong>Tax: </strong><input type="number" bind:value={tax} class="text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1" min="0"/>%</p>
        <p class="mt-2"><strong>Total: ${total.toFixed(2)}</strong></p>
      </div>
      <div class="mt-5">
        <label for="notes-area"><strong>Notes: </strong></label>
        <textarea id="notes-area" bind:value={note} placeholder="write your notes" class="w-full text-gray-900 border rounded border-gray-200 shadow-sm text-sm px-3 py-1"></textarea>
      </div>
      <div class="mt-3">
        <button on:click={generateInvoice} class="mt-5 inline-block rounded bg-indigo-600 px-4 py-2 text-md font-medium text-white hover:bg-indigo-700">Download</button>
      </div>
    </div>
  </div>