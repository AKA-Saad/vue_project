<template>
    <div class="mt-8 flow-root">
        <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
            <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">
                <div class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 sm:rounded-lg">
                    <table class="min-w-full divide-y divide-gray-300">
                        <thead class="bg-gray-50">
                            <tr>
                                <th scope="col"
                                    class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6">Name</th>

                                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Email</th>
                                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Total
                                    Empoyees</th>

                            </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-200 bg-white">
                            <tr v-for="employee in employees" :key="employee.email">
                                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ employee.name }}</td>
                                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ employee.email }}</td>
                                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{
                                    getTotalSales(employee.sales) }} </td>

                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import { reactive, onMounted } from 'vue';

export default {
    name: 'EmployeeList',
    setup() {
        const employees = reactive([]);

    
        onMounted(() => {

            const yourFetchedData = [
                {
                    name: 'John',
                    email: 'john3@example.com',
                    sales: [
                        { customer: 'The Blue Rabbit Company', order_total: 7444 },
                        { customer: 'Black Melon', order_total: 1445 },
                        { customer: 'Foggy Toaster', order_total: 700 }
                    ]
                },
                {
                    name: 'Jane',
                    email: 'jane8@example.com',
                    sales: [
                        { customer: 'The Grey Apple Company', order_total: 203 },
                        { customer: 'Yellow Cake', order_total: 8730 },
                        { customer: 'The Piping Bull Company', order_total: 3337 },
                        { customer: 'The Cloudy Dog Company', order_total: 5310 }
                    ]
                },
                {
                    name: 'Dave',
                    email: 'dave1@example.com',
                    sales: [
                        { customer: 'The Acute Toaster Company', order_total: 1091 },
                        { customer: 'Green Mobile', order_total: 2370 }
                    ]
                }
            ];



            employees.push(...yourFetchedData);
        });

        const getTotalSales = (sales) => {
            // Calculate the total sales for an employee here
            return sales.reduce((total, sale) => total + sale.order_total, 0);
        };

        return {
            employees,
            getTotalSales
        };
    }
};
</script>
