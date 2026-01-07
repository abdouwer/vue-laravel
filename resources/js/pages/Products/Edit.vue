<script setup lang="ts">
import Button from '@/components/ui/button/Button.vue';
import Input from '@/components/ui/input/Input.vue';
import Label from '@/components/ui/label/Label.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { Head, useForm } from '@inertiajs/vue3';


interface Product {
    id: number,
    name: string,
    price: number,
    description: string,
};

const props = defineProps<{product: Product}>();

const form = useForm({
    name: props.product.name,
    price: props.product.price,
    description: props.product.description,
});

const handleSubmit = () => {
    form.put(route('products.update',{product: props.product}))
}

</script>

<template>
    <Head title="Edit the product" />

    <AppLayout :breadcrumbs="[{ title: 'Create a product', href: `/products/${props.product.id}/edit` }]">
        <div class="p-4">
            <form @submit.prevent="handleSubmit" className='w-8/12 space-y-4'>
            
                <div class='space-y-4'>
                    <Label for="product name">Name</Label>
                    <Input placeholder="Product Name" v-model="form.name" type="text"/>
                    <div class="text-sm text-red-600" v-if="form.errors.name">{{ form.errors.name }}</div>
                </div>
                <div class='space-y-4'>
                    <Label for="product price">Price</Label>
                    <Input placeholder="Price" v-model="form.price" type="number"/>
                    <div class="text-sm text-red-600" v-if="form.errors.price">{{ form.errors.price }}</div>
                </div>
                <div class='space-y-4'>
                    <Label for="product description">Description</Label>
                    <Input placeholder="Description" v-model="form.description" type="text" />
                    <div class="text-sm text-red-600" v-if="form.errors.description">{{ form.errors.description }}</div>
                </div>
                <Button type="submit" :disabled="form.processing">Edit the product</Button>
            </form>
        </div>
    </AppLayout>
</template>
