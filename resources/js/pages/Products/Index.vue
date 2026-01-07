<script setup lang="ts">
import {
  Alert,
  AlertDescription,
  AlertTitle,
} from '@/components/ui/alert'
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'
import { Rocket } from 'lucide-vue-next'
import Button from '@/components/ui/button/Button.vue';
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem} from '@/types/index';
import { Head, Link, router } from '@inertiajs/vue3';
import  { route } from 'ziggy-js';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Products',
        href: '/products',
    },
];
interface Product {
    id: number,
    name: string,
    price: number,
    description: string,
}
interface Props  {
    flash: {
        message?: string
    },
    products: Product[]
}

//Get props form my Inertia
 const props = defineProps<Props>();

const handleDelete = (id: number, name: string) => {
    if(confirm(`Do you want to delete a product - ${id}. ${name}`)){
        router.delete(route('products.destroy',{id}))
    }
}

</script>

<template>
    <Head title="Products" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="p-4">
            <div v-if="props.flash?.message" class="mb-4">
                <Alert>
                    <Rocket class="h-4 w-4"/>
                    <AlertTitle>Message</AlertTitle>
                    <AlertDescription>
                        {{ props.flash.message }}
                    </AlertDescription>
                </Alert>
            </div>
                
            <Link :href="route('products.create')"><Button>Create a product</Button></Link>
            <div>
                <Table>
                    <TableCaption>A list of products.</TableCaption>
                    <TableHeader>
                        <TableRow>
                            <TableHead className="w-[100px]">ID</TableHead>
                            <TableHead>Name</TableHead>
                            <TableHead>Price</TableHead>
                            <TableHead>Description</TableHead>
                            <TableHead className="text-center">Action</TableHead>
                        </TableRow>
                    </TableHeader>
                    <TableBody>
                        <TableRow v-for="product in props.products" :key="product.id">
                            <TableCell className="font-medium">{{product.id}}</TableCell>
                            <TableCell>{{product.name}}</TableCell>
                            <TableCell>{{product.price}}</TableCell>
                            <TableCell>{{product.description}}</TableCell>
                            <TableCell className="text-center space-x-2">
                                <Link :href="route('products.edit',product.id)"><Button class="bg-slate-600 hover:bg-slate-700">Edit</Button></Link>
                                <Button  class="bg-red-500 hover:bg-red-700" @click="handleDelete(product.id,product.name)">Delete</Button>
                            </TableCell>
                        </TableRow>
                    </TableBody>
                </Table>
            </div>
        </div>
    </AppLayout>
</template>
