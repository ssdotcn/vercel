<script setup>
import { useLayout } from '@/layout/composables/layout';
import { computed } from 'vue';

const { layoutConfig } = useLayout();

const logoUrl = computed(() => {
    return `layout/images/${layoutConfig.darkTheme.value ? 'logo-white' : 'logo-dark'}.svg`;
    // return `../../../dist/layout/images/unach/${layoutConfig.darkTheme.value ? 'logo_unach_white' : 'logo_unach_black'}.png`;
});
</script>

<template>
    <div class="layout-footer">
        <img :src="logoUrl" alt="Logo" height="30" class="mr-2" />
        by
        <span class="font-medium ml-2">UNACH</span>
    </div>
</template>
<style lang="scss" scoped></style>
