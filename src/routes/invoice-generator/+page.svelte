<script>
    import InvoiceForm from "$lib/components/invoice-generator/InvoiceForm.svelte";

</script>
<InvoiceForm />

<div class="max-w-3xl mx-auto mt-5">
    <div class="p-5">
        <div id="about">
            <h2 class="text-lg font-bold text-center">About InvoiceFast</h2>
            <p class="mt-3 leading-relaxed text-gray-700">InvoiceFast is a free and simple invoice generator solution designed to simplify the invoicing process, helping you save time and focus on what matters most.
                With our invoice generator, you can create professional invoices with just a few clicks, eliminating the hassle of manual paperwork. Our app ensures accurate caluclations, reducing errors and saving your valuable time.
            </p>
        </div>
        <div id="faq" class="space-y-4 mt-5">
            <h2 class="text-lg font-bold text-center">FAQ</h2>
            <details class="group [&_summary::-webkit-details-marker]:hidden" open>
                <summary
                  class="flex cursor-pointer items-center justify-between gap-1.5 rounded-lg bg-gray-50 py-4 text-gray-900"
                >
                  <h2 class="font-medium">Is it free to use?</h2>
            
                  <svg
                    class="h-5 w-5 shrink-0 transition duration-300 group-open:-rotate-180"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                  </svg>
                </summary>
            
                <p class="mt-2 leading-relaxed text-gray-700">
                  Yes, it is completely free to use. You can generate invoices as many as you want.
                </p>
            </details>
            <details class="group [&_summary::-webkit-details-marker]:hidden" open>
                <summary
                  class="flex cursor-pointer items-center justify-between gap-1.5 rounded-lg bg-gray-50 py-4 text-gray-900"
                >
                  <h2 class="font-medium">Do you store our data?</h2>
            
                  <svg
                    class="h-5 w-5 shrink-0 transition duration-300 group-open:-rotate-180"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                  </svg>
                </summary>
            
                <p class="mt-2 leading-relaxed text-gray-700">
                  No, once you clicked download button we generate your invoice pdf and send back to you and delete that invoice instantly.
                </p>
            </details>
            <details class="group [&_summary::-webkit-details-marker]:hidden" open>
                <summary
                  class="flex cursor-pointer items-center justify-between gap-1.5 rounded-lg bg-gray-50 py-4 text-gray-900"
                >
                  <h2 class="font-medium">Who can use our invoice generator?</h2>
            
                  <svg
                    class="h-5 w-5 shrink-0 transition duration-300 group-open:-rotate-180"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                  </svg>
                </summary>
            
                <p class="mt-2 leading-relaxed text-gray-700">
                  Anyone who needed. It is suitable for freelancers, small businesses and medium sized businesses.
                </p>
            </details>
        </div>
        <div class="px-5 pt-16 pb-3">
            <h1 class="text-center font-bold text-2xl">InvoiceFast</h1>
            <p class="text-center text-sm mt-2">Copyright © 2024 <a href="/invoice-generator">InvoiceFast</a></p>
        </div>
    </div>
</div>