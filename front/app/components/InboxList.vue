<template>
    <article v-for="mail in mails" :key="mail.id" class="mail-row" :class="{
        'mail-row--current': mail.id === 0, 'mail-row--clickable': mail.id === 0, 'mail-row--gmail': showGmailActions,
        'mail-row--read': showGmailActions && mail.isRead
    }" @click="mail.id === 0 && emit('open-draft')">
        <div v-if="showGmailActions" class="mail-row__actions" aria-hidden="true">
            <span class="mail-row__checkbox"></span>
            <UIcon name="i-lucide-star" />
            <UIcon name="i-lucide-chevron-right" />
        </div>
        <strong class="mail-row__sender">{{ mail.sender }}</strong>
        <p class="mail-row__subject">{{ mail.subject }}</p>
        <p class="mail-row__snippet">{{ mail.body }}</p>
    </article>
</template>

<script setup lang="ts">
type Mail = {
    id: number;
    sender: string;
    subject: string;
    body: string;
    isRead: boolean;
}

defineProps<{
    mails: Mail[];
    showGmailActions: boolean;
}>();

const emit = defineEmits<{
    (event: 'open-draft'): void
}>()

</script>

<style scoped>
.mail-row {
    display: grid;
    gap: 2px;
    padding: 1rem;
    border-bottom: 1px solid #e0e0e0;
}

.mail-row--gmail {
    grid-template-columns: 4.5rem 9rem max-content minmax(0, 1fr);
    align-items: center;
    gap: 0.5rem;
}

.mail-row__actions {
    display: flex;
    align-items: center;
    gap: 0.65rem;
    color: #9aa0a6;
}

.mail-row__checkbox {
    width: 1rem;
    height: 1rem;
    flex: none;
    border: 1px solid currentColor;
    border-radius: 2px;
}

.mail-row--clickable {
    cursor: pointer;
}

.mail-row__sender {
    font-size: 0.875rem;
    font-weight: 700;
}

.mail-row--read {
    background-color: #f0f5ff;
}

.mail-row--gmail:not(.mail-row--read) .mail-row__sender {
    font-weight: 700;
}

.mail-row--gmail:not(.mail-row--read) .mail-row__subject {
    font-weight: 600;
}

.mail-row--read .mail-row__sender,
.mail-row--read .mail-row__subject,
.mail-row--read .mail-row__snippet {
    font-weight: 400;
}

.mail-row__subject,
.mail-row__snippet {
    overflow: hidden;
    margin: 0;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.mail-row__subject {
    font-size: 0.875rem;
    font-weight: 600;
}

.mail-row__snippet {
    color: #667085;
    font-size: 0.8rem;
}



.mail-viewport--gmail:not(.mail-viewport--mobile) .mail-row__sender {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.mail-viewport--gmail:not(.mail-viewport--mobile) .mail-row__subject {
    overflow: visible;
    text-overflow: clip;
}

.mail-viewport--mobile {
    max-width: 380px;
    margin-inline: auto;
}

.mail-viewport--gmail {
    font-family: Roboto, Arial, sans-serif;
}

.mail-viewport--apple {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}
</style>