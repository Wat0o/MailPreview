<template>
    <UCard class="mail-client-preview">
        <UTabs v-model="activeTarget" :items="previewTargets" :content="false" />
        <template #header>
            <h2 style="font-weight: bold">
                Inbox
            </h2>
        </template>
        <div class="mail-viewport" :class="[
            { 'mail-viewport--mobile': activeTarget.endsWith('-mobile') },
            { 'mail-viewport--gmail': activeTarget.startsWith('gmail') },
            { 'mail-viewport--apple': activeTarget.startsWith('apple') },
            `mail-viewport--${activeTarget}`,
        ]">
            <div class="mail-viewport__layout" :class="{
                'mail-viewport__layout--apple': activeTarget === 'apple-mail',
                'mail-viewport__layout--gmail': activeTarget === 'gmail-desktop',
            }">
                <aside v-if="activeTarget === 'gmail-desktop'" class="mail-viewport__sidebar">
                    <button type="button" @click="showInbox">Inbox</button>
                </aside>
                <div v-if="activeTarget === 'apple-mail' || !isDraftOpen" class="mail-viewport__list">
                    <InboxList :mails="mails" :show-gmail-actions="activeTarget === 'gmail-desktop'"
                        @open-draft="openDraft" />
                </div>
                <div v-if="activeTarget === 'apple-mail' || isDraftOpen" class="mail-viewport__message">
                    <button type="button" @click="showInbox" v-if="activeTarget.endsWith('-mobile') && isDraftOpen"
                        aria-label="Inbox">←</button>
                    <MailBodyPrev :mail="mails[0]!" :is-apple="activeTarget.startsWith('apple')" />
                </div>
            </div>
        </div>
    </UCard>
</template>

<script setup lang="ts">
type PreviewTarget = {
    label: string;
    value: string;
}
type Mail = {
    id: number;
    sender: string;
    subject: string;
    body: string;
    isRead: boolean;
}
defineProps<{
    previewTargets: PreviewTarget[];
    activeTargetLabel: string | undefined;
    mails: Mail[];
}>()


const activeTarget = defineModel<string>('activeTarget', { required: true })

const isDraftOpen = ref(false)

function openDraft() {
    if (activeTarget.value.startsWith('gmail') || activeTarget.value.endsWith('-mobile')) {
        isDraftOpen.value = true;
    }
}

function showInbox() {
    isDraftOpen.value = false;
}

watch(activeTarget, showInbox)
</script>



<style scoped>
.mail-client-preview {
    background-color: #f9f9f9;
    color: #202124;
}

.mail-viewport {
    background-color: #f9f9f9;
    color: #202124;
    margin-top: 1rem;
}

.mail-viewport--gmail:not(.mail-viewport--mobile) .mail-row {
    grid-template-columns: 9rem max-content minmax(0, 1fr);
    align-items: baseline;
    gap: 0.5rem;
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
    font-family: Roboto, RobotoDraft, Helvetica, Arial, sans-serif;
}

.mail-viewport--apple {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

.mail-viewport--apple :deep(.mail-row--current) {
    background-color: #e8eaed;
}

.mail-viewport__layout--apple {
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(0, 2fr);
    min-height: 28rem;
}

.mail-viewport__layout--gmail {
    display: grid;
    grid-template-columns: 10rem minmax(0, 1fr);
    min-height: 28rem;
    column-gap: 0.5rem;
}

.mail-viewport___sidebar {
    padding: 0.75rem;
}

.mail-viewport__sidebar button {
    display: flex;
    align-items: center;
    width: 100%;
    padding: 0.55rem 1rem;
    border: 0;
    border-radius: 999px;
    background: #d3e3fd;
    color: #041e49;
    font: 600 0.875rem Roboto, Arial, sans-serif;
    text-align: left;
    cursor: pointer;
}

.mail-viewport__sidebar button:hover {
    background: #c2dbff;
}

.mail-viewport__message {
    min-width: 0;
    padding: 1rem;
    border-left: 1px solid #e0e0e0;
}

.mail-viewport--apple .mail-viewport__list :deep(.mail-row__snippet) {
    display: -webkit-box;
    overflow: hidden;
    white-space: normal;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    line-clamp: 3;
}
</style>