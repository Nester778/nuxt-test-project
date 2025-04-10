<template>
    <div @click="toggleMode" class="flex items-center space-x-2 change-mode-btn">
        <div class=" text-mg">Change to {{nextMode}}</div>
        <button>{{ nextModeIcon }}</button>
    </div>
</template>

<script setup>
    const colorMode = useColorMode()

    const modes = [
        'light',
        'dark',
    ]

    const nextModeIcons = {
        light: '🌕',
        dark: '🌑'
    }

    const nextMode = computed(() => {
        const currentModeIndex = modes.indexOf(colorMode.preference)
        let nextModeIndex = null
        if (currentModeIndex === 0) {
            nextModeIndex = 1
        } else {
            nextModeIndex = 0
        }
        return modes[nextModeIndex]
    })

    const nextModeIcon = computed(() => nextModeIcons[nextMode.value])

    const toggleMode = () => colorMode.preference = nextMode.value
    
</script>

<style scoped> 
.change-mode-btn{
    cursor: pointer;
    user-select: none;
}
</style>