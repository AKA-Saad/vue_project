<template>
    <div class="mt-8 flow-root">
        <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
            <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">

                {{ getEmployeeWithHighestSales }}
                
                <div class="float-right my-4">
                    <Menu as="div" class="relative inline-block text-left">
                        <div>
                            <MenuButton
                                class="inline-flex w-full justify-center gap-x-1.5 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
                                Sort
                                <svg xmlns="http://www.w3.org/2000/svg"
                                    class="icon inline icon-tabler icon-tabler-arrows-sort ml-4" width="16" height="16"
                                    viewBox="0 0 24 24" stroke-width="1.5" stroke="#9e9e9e" fill="none"
                                    stroke-linecap="round" stroke-linejoin="round">
                                    <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                                    <path d="M3 9l4 -4l4 4m-4 -4v14" />
                                    <path d="M21 15l-4 4l-4 -4m4 4v-14" />
                                </svg>
                            </MenuButton>
                        </div>

                        <transition enter-active-class="transition ease-out duration-100"
                            enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
                            leave-active-class="transition ease-in duration-75"
                            leave-from-class="transform opacity-100 scale-100"
                            leave-to-class="transform opacity-0 scale-95">
                            <MenuItems
                                class="absolute right-0 z-10 mt-2 w-36 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
                                <div class="py-1">
                                    <MenuItem v-slot="{ active }">
                                    <button
                                        :class="[active ? 'bg-gray-100 text-gray-900' : 'text-gray-700', 'block px-4 w-full py-2 text-sm']"
                                        @click="sortEmployees('asc')">Ascending</button>
                                    </MenuItem>
                                    <MenuItem v-slot="{ active }">
                                    <button
                                        :class="[active ? 'bg-gray-100 text-gray-900' : 'text-gray-700', 'block px-4 w-full py-2 text-sm']"
                                        @click="sortEmployees('desc')">Descending</button>

                                    </MenuItem>


                                </div>
                            </MenuItems>
                        </transition>
                    </Menu>
                </div>

                <div class="overflow-hidden w-full shadow ring-1 ring-black ring-opacity-5 sm:rounded-lg">
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
import { reactive, onMounted, computed } from 'vue';
import { Menu, MenuButton, MenuItem, MenuItems } from '@headlessui/vue'

export default {
    name: 'EmployeeList',

    components: {
        Menu,
        MenuButton,
        MenuItem,
        MenuItems,
    },

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


        const getEmployeeWithHighestSales = computed(() => {
            let highestSales = 0;
            let employeeWithHighestSales = null;

            employees.forEach((employee) => {
                const totalSales = getTotalSales(employee.sales);
                if (totalSales > highestSales) {
                    highestSales = totalSales;
                    employeeWithHighestSales = {
                        name: employee.name,
                        email: employee.email
                    };
                }
            });

            return reactive(employeeWithHighestSales);
        });

        const sortEmployees = (order) => {
            employees.sort((a, b) => {
                const salesA = getTotalSales(a.sales);
                const salesB = getTotalSales(b.sales);

                if (order === 'asc') {
                    return salesA - salesB;
                } else {
                    return salesB - salesA;
                }
            });
        };


        const getTotalSales = (sales) => {
            // Calculate the total sales for an employee here
            return sales.reduce((total, sale) => total + sale.order_total, 0);
        };

        return {
            employees,
            getTotalSales,
            getEmployeeWithHighestSales,
            sortEmployees
        };
    }
};
</script>
