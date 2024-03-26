<script setup>
import DialogModal from "@/Components/DialogModal.vue";
import FormSection from "@/Components/FormSection.vue";
import {nextTick} from "vue";
import {useForm} from "@inertiajs/vue3";
import InputLabel from "@/Components/InputLabel.vue";
import TextInput from "@/Components/TextInput.vue";
import InputError from "@/Components/InputError.vue";
import ActionMessage from "@/Components/ActionMessage.vue";
import DangerButton from "@/Components/DangerButton.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";

const props = defineProps({
    post: Object,
    enableModal: Boolean,
})
const emit = defineEmits(['updated']);
const form = useForm({
    _method: '',
    name: props.post?.name || null,
    description: props.post?.description || null,
});
const closeModal = () => {
    form.clearErrors();
    form.reset();
    nextTick().then(() => emit('updated'));
};

const createPost = () => {
    form.post(route('posts.store'), {
        errorBag: 'posts',
        preserveScroll: true,
        onSuccess: () => closeModal(),
    });
};
const updatePost = (id) => {
    form._method = 'PUT';
    form.post(route('posts.update', id), {
        errorBag: 'posts',
        preserveScroll: true,
        onSuccess: () => closeModal(),
    });
};
</script>

<template>
    <DialogModal :show="enableModal" @close="closeModal">
        <template #title>
            <h1 class="font-bold text-xl uppercase">
                {{ post ? 'Edit post' : 'Create post' }}
            </h1>
        </template>
        <template #content>
            <form-section @submitted="post ? updatePost(post.id) : createPost()">
                <template #form>
                    <div class="flex-col gap-4 w-full">
                        <div class="w-full">
                            <input-label class="my-2 text-lg"
                                         for="name"
                                         :value="'Name'"
                            />
                            <text-input class="block w-full"
                                        id="name"
                                        name="name"
                                        type="text"
                                        required
                                        v-model="form.name"
                            />
                            <input-error :message="form.errors.name"/>
                        </div>
                        <div class="w-full">
                            <input-label class="my-2 text-lg"
                                         for="description"
                                         :value="'Description'"
                            />
                            <text-input class="block w-full"
                                        id="description"
                                        name="description"
                                        type="text"
                                        v-model="form.description"
                            />
                            <input-error :message="form.errors.description"/>
                        </div>
                    </div>
                </template>
                <template #actions>
                    <ActionMessage :on="form.recentlySuccessful" class="mr-3">
                        Saved.
                    </ActionMessage>
                    <DangerButton @click="closeModal">
                        Cancel
                    </DangerButton>
                    <PrimaryButton class="ml-3" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                        Save
                    </PrimaryButton>
                </template>
            </form-section>
        </template>
    </DialogModal>
</template>
