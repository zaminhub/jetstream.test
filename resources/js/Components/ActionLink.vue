<script setup>
import {Link} from "@inertiajs/vue3";

defineProps({
    href: {
        type: String,
        default: '#'
    },
    as: String,
    target: {
        type: String,
        default: ''
    }
})
</script>
<template>
    <div class="w-4 transform hover:text-blue-600 dark:hover:text-blue-300 hover:scale-110">
        <a :href="href" v-if="as === 'a'" :target="target">
            <slot/>
        </a>
        <Link v-else :href="href" :target="target">
            <slot/>
        </Link>
    </div>
</template>
