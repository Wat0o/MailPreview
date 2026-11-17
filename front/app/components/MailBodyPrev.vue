<template>
    <div class="mail-body-prev" :class="{ 'mail-body-prev--apple': isApple }">
        <h3 class="mail-body-prev__subject">{{ mail.subject }}</h3>

        <div class="mail-body-prev__sender">
            <span class="mail-body-prev__avatar" :class="{ 'mail-body-prev__avatar--apple': isApple }"
                aria-hidden="true">
                {{ senderInitial }}
            </span>
            <strong>{{ mail.sender }}</strong>
        </div>

        <p class="mail-body-prev__body">{{ mail.body }}</p>
    </div>
</template>

<script setup lang="ts">
const props = defineProps<{
    mail: {
        id: number
        sender: string
        subject: string
        body: string
    }
    isApple: boolean
}>()

const senderInitial = computed(
    () => props.mail.sender.trim().charAt(0).toUpperCase() || '?'
)
</script>

<style scoped>
.mail-body-prev {
    padding: 1.5rem;
    background: #fff;
    color: #202124;
    display: flex;
    flex-direction: column;
}

.mail-body-prev__subject {
    margin: 0 0 1.5rem;
    font-size: 1.35rem;
    font-weight: 400;
    order: 1;
}

.mail-body-prev__sender {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-bottom: 1.5rem;
    order: 2;
}

.mail-body-prev__avatar {
    display: grid;
    width: 2.5rem;
    height: 2.5rem;
    flex: none;
    place-items: center;
    border-radius: 50%;
    background: #1a73e8;
    color: #fff;
    font-weight: 600;
}

.mail-body-prev--apple .mail-body-prev__sender {
    order: 1;
    padding-bottom: 1rem;
    border-bottom: 1px solid #dadce0;
}

.mail-body-prev--apple .mail-body-prev__subject {
    order: 2;
    font-weight: 600;
}

.mail-body-prev__avatar--apple {
    background: #9aa0a6;
}

.mail-body-prev__body {
    margin: 0;
    line-height: 1.6;
    white-space: pre-wrap;
    order: 4;
}
</style>