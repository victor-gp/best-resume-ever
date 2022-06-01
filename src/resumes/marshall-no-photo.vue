<template>
    <marshall no-photo="yes" ></marshall>
</template>

<script>
import Vue from 'vue';
import { getVueOptions } from './options';

const name = 'marshall-no-photo';
export default Vue.component(name, getVueOptions(name));
</script>
