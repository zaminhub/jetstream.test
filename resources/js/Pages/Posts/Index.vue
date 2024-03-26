<script setup>
import AppLayout from "@/Layouts/AppLayout.vue";
import ActionDeleteButton from "@/Components/ActionDeleteButton.vue";
import ActionLink from "@/Components/ActionLink.vue";
import {router} from "@inertiajs/vue3";
import {ref} from "vue";
import PostModal from "@/Pages/Posts/PostModal.vue";

defineProps({
    posts: Object,
});

const enableModal = ref(false);
const editedPost = ref(null);

const editPost = (post) => {
    editedPost.value = post;
    enableModal.value = true;
}
const deletePost = (post_id) => {
    router.delete(route('posts.destroy', post_id), {
        errorBag: 'posts',
        preserveScroll: true,
    })
}
const closeModal = () => {
    enableModal.value = false;
    editedPost.value = null;
}
</script>

<template>
    <AppLayout title="Posts">
        <template #header>
            <div class="flex justify-between items-center">
                <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">
                    Posts
                </h2>
                <button @click.prevent="enableModal = true"
                        class="p-1.5 -m-1.5 border border-gray-200 dark:border-gray-700 rounded-md bg-white hover:bg-gray-100 dark:bg-gray-800 dark:hover:bg-gray-700 transition ease-in-out hover:border-blue-500 focus:outline-none focus:border-blue-300 focus:ring focus:ring-blue-200 shadow dark:shadow-gray-700 text-blue-600 dark:text-blue-300">
                    <svg xmlns="http://www.w3.org/2000/svg"
                         width="18px"
                         height="18px"
                         viewBox="0 0 490 490"
                         fill="currentColor">
                        <polygon points="222.031,490 267.969,490 267.969,267.969 490,267.969 490,222.031 267.969,222.031 267.969,0 222.031,0
            222.031,222.031 0,222.031 0,267.969 222.031,267.969 "/>
                    </svg>
                </button>
            </div>

        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white dark:bg-gray-800 overflow-hidden shadow-xl sm:rounded-lg">
                    <table class="w-full table-auto">
                        <thead>
                        <tr class="bg-gray-300 dark:bg-slate-500 text-gray-600 dark:text-gray-200 uppercase text-sm leading-normal">
                            <th class="py-3 px-6 text-left">Post name</th>
                            <th class="py-3 px-6 text-left">Description</th>
                            <th class="py-3 px-6 text-center w-5">Actions</th>
                        </tr>
                        </thead>
                        <tbody class="dark:bg-gray-700 text-gray-600 dark:text-gray-200 text-sm font-light">
                        <tr class="border-b border-gray-200 dark:border-gray-500 hover:bg-gray-100 dark:hover:bg-gray-600 font-medium"
                            v-for="post in posts">
                            <td class="py-3 px-6 text-left">
                                {{ post.name }}
                            </td>
                            <td class="py-3 px-6 text-center">
                                <div class="flex item-center justify-center gap-2">
                                    <action-link as="a" @click.prevent="editPost(post)">
                                        <svg xmlns="http://www.w3.org/2000/svg"
                                             fill="none"
                                             viewBox="0 0 24 24"
                                             stroke="currentColor"
                                        >
                                            <path stroke-linecap="round"
                                                  stroke-linejoin="round"
                                                  stroke-width="2"
                                                  d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"
                                            />
                                        </svg>
                                    </action-link>
                                    <action-delete-button @submitted="deletePost(post.id)"/>
                                </div>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <PostModal :post="editedPost" v-if="enableModal" :enable-modal="enableModal" @updated="closeModal"/>
    </AppLayout>
</template>
