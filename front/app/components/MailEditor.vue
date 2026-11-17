<template>
    <UCard class="editor-section">
        <div class="editor-fields">
            <UFormField label="Expéditeur">
                <UInput v-model="expeditor" class="w-full" placeholder="Expéditeur " />
            </UFormField>

            <UFormField label="Objet">
                <UInput v-model="subject" class="w-full" placeholder="Objet" />
            </UFormField>

            <UFormField label="Contenu">
                <UTextarea v-model="body" class="w-full" :rows="25" placeholder="Contenu de l'email..." />
            </UFormField>
        </div>
    </UCard>
</template>

<script setup lang="ts">
const expeditor = defineModel<string>('expeditor', { required: true })
const subject = defineModel<string>('subject', { required: true })
const body = defineModel<string>('body', { required: true })
</script>

<style scoped>
.editor-section {
    background-color: #f9f9f9;
    color: #000000;
}

.editor-fields {
    display: grid;
    gap: 1rem;
}

.editor-fields :deep(input),
.editor-fields :deep(textarea) {
    background-color: #e5e7eb;
    color: #111827;
}

.editor-fields :deep(input::placeholder),
.editor-fields :deep(textarea::placeholder) {
    color: #6b7280;
}

.editor-fields :deep(label) {
    color: #000000;
}

.input {
    background-color: #ffffff;
    color: #000000;
}
</style>