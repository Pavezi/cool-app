<template>
    <div>
        <input type="file" @change="handleFileChange" accept="image/*" />
        <button @click="togglePaletteVisibility">Pick Color</button>
        <div v-if="showPalette && colors.length">
            <div v-for="(color, index) in colors" :key="index"
                :style="{ backgroundColor: `rgb(${color.join(',')})`, width: '100px', height: '100px', margin: '10px' }">
                RGB: {{ color.join(', ') }}
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { ref } from 'vue';
import ColorThief from 'colorthief';

export default {
    setup() {
        const colors = ref<number[][]>([]);
        const showPalette = ref(false);

        const handleFileChange = async (event: Event) => {
            const file = (event.target as HTMLInputElement).files?.[0];
            if (!file) return;

            const img = new Image();
            img.src = URL.createObjectURL(file);
            img.onload = () => {
                const colorThief = new ColorThief();
                const dominantColors = colorThief.getPalette(img, 5);
                colors.value = dominantColors;
            };
        };
        const togglePaletteVisibility = () => {
            showPalette.value = !showPalette.value;
        };
        return { colors, showPalette, handleFileChange, togglePaletteVisibility };
    }
};
</script>
