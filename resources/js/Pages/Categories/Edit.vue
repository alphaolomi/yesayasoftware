<template>
    <app-layout>
        <template #header>
            <div class="lg:flex lg:items-center lg:justify-between">
                <div class="flex-1 min-w-0">
                    <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                        Category
                    </h2>

                    <div class="mt-1 flex flex-col sm:mt-0 sm:flex-row sm:flex-wrap">
                        <inertia-link 
                            :href="`/categories`"
                            class="mt-2 flex items-center text-sm leading-5 text-blue-500 sm:mr-6">
                            <svg class="flex-shrink-0 mr-1.5 h-5 w-5 text-gray-400" fill="currentColor" viewBox="0 0 24 24">
                                <path d="M10 19L3 12M3 12L10 5M3 12L21 12" stroke="#4A5568" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                            </svg>

                            Go back
                        </inertia-link>

                        <div class="mt-2 flex items-center text-sm leading-5 text-gray-500 sm:mr-6">
                            <svg class="flex-shrink-0 mr-1.5 h-5 w-5 text-gray-400" fill="currentColor" viewBox="0 0 24 24">
                                <path d="M13 16v5a1 1 0 01-1 1H9l-3-6a2 2 0 01-2-2 2 2 0 01-2-2v-2c0-1.1.9-2 2-2 0-1.1.9-2 2-2h7.59l4-4H20a2 2 0 012 2v14a2 2 0 01-2 2h-2.41l-4-4H13zm0-2h1.41l4 4H20V4h-1.59l-4 4H13v6zm-2 0V8H6v2H4v2h2v2h5zm0 2H8.24l2 4H11v-4z"/>
                            </svg>
                            
                            {{ category.posts.length }}
                        </div>
                    </div>
                </div>
                
                <div class="mt-5 flex lg:mt-0 lg:ml-4">
                    <span class="hidden sm:block shadow-sm rounded-md">
                        <inertia-link 
                            :href="`/categories/${category.slug}`" 
                            class="inline-flex items-center px-4 py-2 border border-gray-300 text-sm leading-5 font-medium rounded-md text-gray-700 bg-white hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 active:text-gray-800 active:bg-gray-50 transition duration-150 ease-in-out">
                            <svg class="-ml-1 mr-2 h-5 w-5 text-gray-500" fill="currentColor" viewBox="0 0 20 20">
                                <path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"/>
                            </svg>
                            
                            View
                        </inertia-link>
                    </span>

                    <span v-if="$page.user" class="sm:ml-3 shadow-sm rounded-md">
                        <form id="subscribe-form" action="" method="POST">
                            <button type="submit" class="inline-flex items-center px-4 py-2 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-blue-600 hover:bg-blue-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-700 active:bg-blue-700 transition duration-150 ease-in-out">
                                <svg class="-ml-1 mr-2 h-5 w-5" fill="currentColor" viewBox="0 0 20 20">
                                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
                                </svg>
                                
                                Subscribe
                            </button>
                        </form>
                    </span>

                    <span v-if="$page.user" x-data="{ open: false }" class="ml-3 relative shadow-sm rounded-md sm:hidden">
                        <button @click="open = !open" type="button" class="inline-flex items-center px-4 py-2 border border-gray-300 text-sm leading-5 font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:shadow-outline focus:border-blue-300 transition duration-150 ease-in-out">
                            More
                            
                            <svg class="-mr-1 ml-2 h-5 w-5 text-gray-500" fill="currentColor" viewBox="0 0 20 20">
                                <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd"/>
                            </svg>
                        </button>

                        <div x-show="open" x-transition:enter="transition ease-out duration-200" x-transition:enter-start="transform opacity-0 scale-95" x-transition:enter-end="transform opacity-100 scale-100" x-transition:leave="transition ease-in duration-75" x-transition:leave-start="transform opacity-100 scale-100" x-transition:leave-end="transform opacity-0 scale-95" class="origin-top-right absolute right-0 mt-2 -mr-1 w-48 rounded-md shadow-lg">
                            <div class="py-1 rounded-md bg-white shadow-xs">
                                <inertia-link 
                                    :href="`/categories/${category.slug}/edit`" 
                                    class="block px-4 py-2 text-sm leading-5 text-gray-700 hover:bg-gray-100 focus:outline-none focus:bg-gray-100 transition duration-150 ease-in-out">
                                    Edit
                                </inertia-link>
                            </div>
                        </div>
                    </span>
                </div>
            </div>
        </template>

        <div>
            <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
                <div class="pt-12 sm:pt-16">
                    <div class="flex flex-col">
                        <form class="bg-white pt-12 sm:pt-16 px-4 sm:px-6 md:px-8" 
                            method="POST"
                            @submit.prevent="update">

                            <div class="grid grid-cols-1 row-gap-6 col-gap-4 sm:grid-cols-6">
                                <div class="sm:col-span-6">
                                    <label for="name" class="block text-sm font-medium leading-5 text-gray-700">
                                        Name
                                    </label>
                                    
                                    <div class="mt-1 rounded-md shadow-sm">
                                        <input v-model="form.name"
                                            id="name" 
                                            name="name" 
                                            class="form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5" />
                                    </div>
                                </div>

                                <div class="sm:col-span-6">
                                    <label for="description" class="block text-sm font-medium leading-5 text-gray-700">
                                        Description
                                    </label>

                                    <div class="mt-1 rounded-md shadow-sm">
                                        <VueTrix 
                                            v-model="form.description"
                                            id="descrtiption" 
                                            name="descrtiption" 
                                            placeholder="Enter content"/>
                                    </div>

                                    <p class="mt-2 text-sm text-gray-500">
                                        Write a few sentences to description this Tender.
                                    </p>
                                </div>

                                <div class="sm:col-span-6">
                                    <!-- Post Thumbnail -->
                                    <div class="col-span-6 sm:col-span-4">
                                        <!-- Post Thumbnail File Input -->
                                        <input type="file" class="hidden"
                                                    ref="thumbnail"
                                                    @change="updateThumbnailPreview">

                                        <label for="thumbnail" class="block text-sm font-medium leading-5 text-gray-700">
                                            Thumbnail
                                        </label>

                                        <!-- Current Post Thumbnail -->
                                        <div class="mt-2 mb-4" v-show="! thumbnailPreview">
                                            <img :src="`/${form.thumbnail}`" alt="Current Post Thumbnail" class="w-full object-cover">
                                        </div>

                                        <!-- New Post Thumbnail Preview -->
                                        <div class="mt-2 mb-4" v-show="thumbnailPreview">
                                            <span class="block w-full h-72"
                                                :style="'background-size: cover; background-repeat: no-repeat; background-position: center center; background-image: url(\'' + thumbnailPreview + '\');'">
                                            </span>
                                        </div>

                                        <button type="button" 
                                            @click.prevent="selectNewThumbnail" 
                                            class="py-2 px-4 border border-gray-300 rounded-md text-sm leading-5 font-medium text-gray-700 hover:text-gray-500 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-50 active:text-gray-800 transition duration-150 ease-in-out">
                                            Select A New Thumbnail
                                        </button>

                                        <button type="button" 
                                            @click.prevent="deleteThumbnail" 
                                            v-if="`/${form.thumbnail}`"
                                            class="py-2 px-4 border border-gray-300 rounded-md text-sm leading-5 font-medium text-gray-700 hover:text-gray-500 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-50 active:text-gray-800 transition duration-150 ease-in-out">
                                            Remove Thumbnail
                                        </button>

                                        <p class="mt-2 text-sm text-red-500">
                                            {{ form.error('thumbnail') }}
                                        </p>
                                    </div>
                                </div>
                            </div>

                            <div class="mt-8 mb-8 border-t border-gray-200 pt-5">
                                <div class="flex justify-end">
                                    <span class="inline-flex rounded-md shadow-sm">
                                        <button type="button" class="py-2 px-4 border border-gray-300 rounded-md text-sm leading-5 font-medium text-gray-700 hover:text-gray-500 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-50 active:text-gray-800 transition duration-150 ease-in-out">
                                            Cancel
                                        </button>
                                    </span>
                                
                                    <span class="ml-3 inline-flex rounded-md shadow-sm">
                                        <button type="submit" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-blue-600 hover:bg-blue-500 focus:outline-none focus:border-blue-700 focus:shadow-outline-blue active:bg-blue-700 transition duration-150 ease-in-out"
                                            :disabled="loading">
                                            <half-circle-spinner
                                                v-if="loading"
                                                :animation-duration="1000"
                                                :size="20"
                                                color="#fff"
                                                class="mr-4"
                                                />
                                            Save
                                        </button>
                                    </span>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import AppLayout from './../../Layouts/AppLayout'
    import VueTrix from "vue-trix";
    import { HalfCircleSpinner } from 'epic-spinners'

    export default {
        props: ['sessions', 'category'],

        components: {
            AppLayout,
            VueTrix,
            HalfCircleSpinner
        },

        data() {
            return {
                loading: false,

                form: this.$inertia.form({
                    '_method': 'PUT',
                    name: this.category.name,
                    description: this.category.description,
                    thumbnail: this.category.thumbnail,
                }, {
                    bag: 'store',
                    resetOnSuccess: false,
                }),

                thumbnailPreview: null,
            }
        },

        methods: {
            update() {
                if (this.$refs.thumbnail) {
                    this.form.thumbnail = this.$refs.thumbnail.files[0]
                }

                this.form.post('/categories/' + this.category.slug, {
                    preserveScroll: true
                });
            },

            selectNewThumbnail() {
                this.$refs.thumbnail.click();
            },

            updateThumbnailPreview() {
                const reader = new FileReader();

                reader.onload = (e) => {
                    this.thumbnailPreview = e.target.result;
                };

                reader.readAsDataURL(this.$refs.thumbnail.files[0]);
            },

            deleteThumbnail() {
                this.$inertia.delete('/categories/thumbnail/' + this.category.slug, {
                    preserveScroll: true,
                }).then(() => {
                    this.thumbnailPreview = null
                });
            },
        }
    }
</script>