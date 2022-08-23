<template>
    <header>
        <h1>{{ title }}</h1>
        <Button @toggle-add-task="$emit('toggle-add-task')"
         :text="showAddTask ? 'close' : 'add Task'" :colour="showAddTask ? 'red' : 'green'" />
         
    </header>
    
</template>

<script>
import { booleanLiteral } from '@babel/types';
import Button from './Button'


export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: booleanLiteral,
    },
    components: {
        Button,
    }
}
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>
