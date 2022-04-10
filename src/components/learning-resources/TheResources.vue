<template>
    <base-card>
        <base-button 
            @click='setSelectedTab("stored-resources")'
            :mode='storedResButtonMode'
        >Stored Reosurces</base-button>
        <base-button 
            @click='setSelectedTab("add-resource")'
            :mode='addResButtonMode'
        >Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>

import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
    components: {
        StoredResources,
        AddResource
    },
    data(){
        return {
            selectedTab: 'stored-resources',
            storedResources: [
                {
                    id: 'official-vue', 
                    title: 'Vue.js', 
                    description: 'Vue.js docs', 
                    link: 'https://vuejs.org/'
                },
                {
                    id: 'google', 
                    title: 'Google', 
                    description: 'Google', 
                    link: 'https://www.google.com/'
                }
            ]
        }
    },
    provide(){
        return {
            resources: this.storedResources,
            addResource: this.addResource,
            removeResource: this.removeResource
        }
    },
    computed: {
        storedResButtonMode(){
            return this.selectedTab === "stored-resources" ? "" : "flat"
        },
        addResButtonMode(){
            return this.selectedTab === "add-resource" ? "" : "flat"
        }
    },
    methods: {
        setSelectedTab(tab){
            this.selectedTab = tab
        },
        addResource(title, description, link){
            const newResources = {
                id: new Date().toISOString(),
                title,
                description,
                link
            }

            this.storedResources.unshift(newResources)
            this.selectedTab = "stored-resources"
        },
        removeResource(resId){
            const resIndex = this.storedResources.findIndex(item => item.id === resId)
            this.storedResources.splice(resIndex, 1)
        }
    }
}
</script>