<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Inertia } from "@inertiajs/inertia";
import { Head } from "@inertiajs/vue3";
</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                List of To Do
            </h2>
        </template>
        <div class="py-10">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-32">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div>
                        <form
                            v-if="!isUpdating"
                            class="md:px-32 md:py-5 sm:p-8"
                            @submit.prevent="add()"
                        >
                            <label
                                for="addInput"
                                class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white"
                                >Search</label
                            >
                            <div class="relative">
                                <div
                                    class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
                                ></div>
                                <input
                                    type="search"
                                    id="addInput"
                                    name="activity"
                                    v-model="form.activity"
                                    class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500"
                                    placeholder="Add Activities"
                                    required
                                />
                                <input
                                    type="number"
                                    id="user_id"
                                    name="user_id"
                                    v-model="form.user_id"
                                    hidden
                                />
                                <button
                                    type="submit"
                                    :disabled="form.processing"
                                    class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                                >
                                    Add
                                </button>
                            </div>
                        </form>
                        <form
                            v-if="isUpdating"
                            class="md:px-32 md:py-5 sm:p-1"
                            @submit.prevent="updateList(updateData.id)"
                        >
                            <label
                                for="update"
                                class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white"
                                >Search</label
                            >
                            <div class="relative">
                                <div
                                    class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
                                ></div>
                                <input
                                    type="search"
                                    id="update"
                                    name="activity"
                                    v-model="updateData.activity"
                                    class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500"
                                    placeholder="Add Activities"
                                    required
                                />
                                <button
                                    type="submit"
                                    :disabled="form.processing"
                                    class="mr-20 text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                                >
                                    Update
                                </button>
                                <button
                                    @click="isUpdating = false"
                                    class="text-white absolute right-2.5 bottom-2.5 bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800"
                                >
                                    Cancel
                                </button>
                            </div>
                        </form>
                    </div>
                    <div
                        class="border-black md:px-32 md:py-5 max-md:px-3 max-sm:py-3 max-sm:px-50"
                    >
                        <div>
                            <ul>
                                <li
                                    v-for="(todo, index) in toDoList"
                                    :key="index"
                                    class="mb-4"
                                >
                                    <div class="flex">
                                        <span class="mr-2 text-lg">
                                            {{ index + 1 }}
                                        </span>
                                        <span class="text-lg">
                                            {{ todo.activity }}
                                        </span>
                                        <button
                                            @click="showAlert(todo.id)"
                                            class="ml-auto text-white right-2.5 bottom-2.5 bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-1 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800"
                                        >
                                            <svg
                                                xmlns="http://www.w3.org/2000/svg"
                                                x="0px"
                                                y="0px"
                                                width="24"
                                                height="24"
                                                viewBox="0,0,256,256"
                                                style="fill: #000000"
                                            >
                                                <g
                                                    fill="#ffffff"
                                                    fill-rule="nonzero"
                                                    stroke="none"
                                                    stroke-width="1"
                                                    stroke-linecap="butt"
                                                    stroke-linejoin="miter"
                                                    stroke-miterlimit="10"
                                                    stroke-dasharray=""
                                                    stroke-dashoffset="0"
                                                    font-family="none"
                                                    font-weight="none"
                                                    font-size="none"
                                                    text-anchor="none"
                                                    style="
                                                        mix-blend-mode: normal;
                                                    "
                                                >
                                                    <g
                                                        transform="scale(10.66667,10.66667)"
                                                    >
                                                        <path
                                                            d="M10,2l-1,1h-4c-0.6,0 -1,0.4 -1,1c0,0.6 0.4,1 1,1h2h10h2c0.6,0 1,-0.4 1,-1c0,-0.6 -0.4,-1 -1,-1h-4l-1,-1zM5,7v13c0,1.1 0.9,2 2,2h10c1.1,0 2,-0.9 2,-2v-13zM9,9c0.6,0 1,0.4 1,1v9c0,0.6 -0.4,1 -1,1c-0.6,0 -1,-0.4 -1,-1v-9c0,-0.6 0.4,-1 1,-1zM15,9c0.6,0 1,0.4 1,1v9c0,0.6 -0.4,1 -1,1c-0.6,0 -1,-0.4 -1,-1v-9c0,-0.6 0.4,-1 1,-1z"
                                                        ></path>
                                                    </g>
                                                </g>
                                            </svg>
                                        </button>
                                        <button
                                            @click="
                                                getDataUpdate(
                                                    todo.id,
                                                    todo.activity
                                                )
                                            "
                                            class="ml-4 text-white right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-1 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                                        >
                                            <svg
                                                xmlns="http://www.w3.org/2000/svg"
                                                x="0px"
                                                y="0px"
                                                width="24"
                                                height="24"
                                                viewBox="0,0,256,256"
                                                style="fill: #000000"
                                            >
                                                <g
                                                    fill="#ffffff"
                                                    fill-rule="nonzero"
                                                    stroke="none"
                                                    stroke-width="1"
                                                    stroke-linecap="butt"
                                                    stroke-linejoin="miter"
                                                    stroke-miterlimit="10"
                                                    stroke-dasharray=""
                                                    stroke-dashoffset="0"
                                                    font-family="none"
                                                    font-weight="none"
                                                    font-size="none"
                                                    text-anchor="none"
                                                    style="
                                                        mix-blend-mode: normal;
                                                    "
                                                >
                                                    <g
                                                        transform="scale(10.66667,10.66667)"
                                                    >
                                                        <path
                                                            d="M19.17188,2c-0.72375,0 -1.4475,0.27562 -2,0.82813l-1.17187,1.17188l4,4l1.17188,-1.17187c1.104,-1.104 1.104,-2.895 0,-4c-0.5525,-0.5525 -1.27625,-0.82812 -2,-0.82812zM14.5,5.5l-11.5,11.5v4h4l11.5,-11.5z"
                                                        ></path>
                                                    </g>
                                                </g>
                                            </svg>
                                        </button>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>

<script>
import { Link } from "@inertiajs/vue3";
import { Inertia } from "@inertiajs/inertia";
export default {
    components: { Link },
    props: ["toDoList", "user_id"],
    data() {
        return {
            form: {
                activity: "",
                user_id: this.user_id,
            },
            isUpdating: false,
            updateData: {
                activity: "",
                id: 0,
            },
        };
    },
    methods: {
        add() {
            try {
                Inertia.post("/todo", this.form);
            } catch (error) {
                console.error("Kesalahan dalam permintaan:", error);
            }
        },

        deleteList(activityId) {
            Inertia.delete(`/todo/${activityId}`);
        },
        getDataUpdate(id, text) {
            this.updateData.id = id;
            this.updateData.activity = text;
            this.isUpdating = true;
        },
        updateList(activityId) {
            Inertia.put(`/todo/${activityId}`, this.updateData);
        },
        showAlert(id) {
            const swalWithBootstrapButtons = this.$swal.mixin({
                customClass: {
                    confirmButton:
                        "ml-4 text-white bg-green-700 hover:bg-green-800 font-medium rounded-lg text-sm px-4 py-4 dark:bg-green-600 dark:hover:bg-green-700",
                    cancelButton:
                        "ml-4 text-white bg-red-700 hover:bg-red-800 font-medium rounded-lg text-sm px-4 py-4 dark:bg-red-600 dark:hover:bg-red-700",
                },
                buttonsStyling: false,
            });
            swalWithBootstrapButtons
                .fire({
                    title: "Are you sure?",
                    text: "You won't be able to revert this!",
                    icon: "warning",
                    showCancelButton: true,
                    confirmButtonText: "Yes, delete it!",
                    cancelButtonText: "No, cancel!",
                    reverseButtons: true,
                })
                .then((result) => {
                    if (result.isConfirmed) {
                        swalWithBootstrapButtons.fire({
                            title: "Deleted!",
                            text: "Your file has been deleted.",
                            icon: "success",
                        });
                        this.deleteList(id)
                    } else if (
                        /* Read more about handling dismissals below */
                        result.dismiss === Swal.DismissReason.cancel
                    ) {
                        swalWithBootstrapButtons.fire({
                            title: "Cancelled",
                            text: "Your imaginary file is safe :)",
                            icon: "error",
                        });
                    }
                });
        },
    },
};
</script>
