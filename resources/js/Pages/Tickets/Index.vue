<script setup>
import Pagination from '@/Components/Pagination.vue'
import {watchEffect, ref} from "vue";
import { Link } from "@inertiajs/vue3";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import DateFormatter from '@/Utils/DateFormatter';
import Breadcrumb from '@/Components/Breadcrumb.vue';
import { router } from '@inertiajs/vue3';

const props = defineProps({
    tickets: Object,
})

const searchQuery = ref('');
const filters = ref({
    startDate: null,
    endDate: null,
    priority: '',
    status: '',
});

const searchTickets = () => {
    router.replace(route('tickets.index', { search: searchQuery.value.toLowerCase(), ...filters.value })); // Convert search query to lowercase
};


// Function to apply filters
const applyFilters = () => {
    searchTickets();
    console.log("🚀 ~ applyFilters ~ applyFilters:", "you my search")
};


// Function to format the date
const formatDate = (dateString) => {
    return DateFormatter.formatDateTime(dateString); // or use formatDate for only date
}

const breadcrumbItems = [
        { text: 'Dashboard', link: '/' },
        { text: 'Tickets'},
      ];
</script>

<template>
    <AuthenticatedLayout>
        
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <Breadcrumb :items="breadcrumbItems" />
            <div class="flex justify-between items-center py-6">
                <h1 class="text-2xl font-semibold text-gray-100">Tickets</h1>
                <Link :href="route('tickets.create')"
                   class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                    Create Ticket
                </Link>
            </div>
            <!-- Add advanced search form -->
            <div class="p-4 bg-gray-800 rounded-lg mb-4 flex flex-col space-y-4">
                <!-- Single input field for all filters except date range, priority, and status -->
                <input v-model="searchQuery" type="text" placeholder="Search by title, content, user name, email" class="p-2 border border-gray-700 rounded-md">

                <!-- Date range filter -->
                <div class="flex flex-col space-y-2">
                    <input v-model="filters.startDate" type="date" placeholder="Start Date" class="p-2 border border-gray-700 rounded-md">
                    <input v-model="filters.endDate" type="date" placeholder="End Date" class="p-2 border border-gray-700 rounded-md">
                </div>

                <!-- Priority filter -->
                <select v-model="filters.priority" class="p-2 border border-gray-700 rounded-md">
                    <option value="">Select Priority</option>
                    <option value="low">Low</option>
                    <option value="medium">Medium</option>
                    <option value="high">High</option>
                </select>

                <!-- Status filter -->
                <select v-model="filters.status" class="p-2 border border-gray-700 rounded-md">
                    <option value="">Select Status</option>
                    <option value="open">Open</option>
                    <option value="closed">Closed</option>
                    <option value="in_progress">In Progress</option>
                </select>

                <!-- Apply filter button -->
                <button @click="applyFilters" class="bg-indigo-600 text-white px-4 py-2 rounded-md hover:bg-indigo-700">Apply Filters</button>
            </div>

            <div class="overflow-x-auto shadow  sm:rounded-lg">
                <table class="min-w-full divide-y divide-gray-700">
                    <thead class="">
                    <tr>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            ID
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            Submitted
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            Title
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            User
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            Priority
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            Status
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                            &nbsp;
                        </th>

                    </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-700">
                    <tr v-for="ticket in tickets.data" :key="ticket.id">
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">{{ ticket.id }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">{{ formatDate(ticket.created_at) }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-100">{{ ticket.title }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">{{ ticket.user.name }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">{{ ticket.priority }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">{{ ticket.status }}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-400">
                            <Link :href="route('tickets.show', [ticket.id])">
                                View
                            </Link>
                        </td>
                    </tr>
                    </tbody>
                </table>
                <Pagination
                    :total-items="tickets.total"
                    :current-page="tickets.current_page"
                    :per-page="tickets.per_page"
                    @update:current-page="$inertia.visit(route('tickets.index', {page: $event}))"
                />
            </div>
        </div>
    </AuthenticatedLayout>

</template>

