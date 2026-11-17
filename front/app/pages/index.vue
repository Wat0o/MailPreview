<template>
    <main class="page">
        <header class="page-header">
            <h1>Email Preview</h1>
        </header>
        <div class="workspace">
            <MailEditor v-model:expeditor="expeditor" v-model:subject="subject" v-model:body="body" />
            <MailClientPreview :mails="mails" :previewTargets="previewTargets" v-model:activeTarget="activeTarget"
                :activeTargetLabel="activeTargetLabel" />
        </div>
    </main>
</template>

<script setup lang="ts">
const expeditor = ref("")
const subject = ref("")
const body = ref("")

const sampleMails = ref([
    {
        id: 1, sender: "Camille Bernard", subject: "Récapitulatif de notre réunion de jeudi et prochaines étapes du projet", body: `Bonjour,
Merci pour vos retours lors de notre réunion. Vous trouverez ci-joint le récapitulatif des décisions prises et la liste des prochaines étapes.
N'hésitez pas à me signaler s'il manque un point avant notre prochain échange.
Bonne journée,
Camille`, isRead: true
    },
    {
        id: 2, sender: "Thomas Lefèvre", subject: "Votre réservation est confirmée", body: `Bonjour,
Nous vous confirmons que votre réservation est bien enregistrée. Vous trouverez les horaires d’arrivée, l’adresse et les informations d’accès dans votre espace client.
Notre équipe reste disponible si vous avez la moindre question.
À bientôt,
Thomas`, isRead: false
    },
    {
        id: 3, sender: "Nora Benali", subject: "Les nouvelles maquettes sont prêtes pour relecture avant la présentation de lundi", body: `Bonjour à tous,
Les dernières maquettes sont maintenant disponibles. J’ai intégré les commentaires reçus cette semaine et ajusté les versions mobile et ordinateur.
Pourriez-vous les relire d’ici vendredi après-midi afin que nous puissions préparer la présentation de lundi ?
Merci,
Nora`, isRead: true
    },
])
const mails = computed(() => [
    {
        id: 0,
        sender: expeditor.value,
        subject: subject.value,
        body: body.value,
        isRead: false
    },
    ...sampleMails.value,
])
const previewTargets = [
    { label: "Gmail desktop", value: "gmail-desktop" },
    { label: 'Apple Mail', value: 'apple-mail' },
    { label: "Gmail mobile", value: "gmail-mobile" },
    { label: "Apple Mail mobile", value: "apple-mail-mobile" }
]

const activeTarget = ref('gmail-desktop')
const activeTargetLabel = computed(() =>
    previewTargets.find(t => t.value === activeTarget.value)?.label
)
</script>

<style scoped>
.page {
    background-color: #f9f9f9;
    font-family: "Segoe UI", Arial, sans-serif;
    color: #202124;
    min-height: 100vh;
}

.page :deep(input),
.page :deep(textarea) {
    font-family: inherit;
}

.page-header {
    margin-bottom: 1.5rem;
    text-align: center;
}


.page-header h1 {
    margin: 0;
    color: #1f2937;
    font-size: clamp(1.75rem, 2.5vw, 2.25rem);
    font-weight: 700;
}


.workspace {
    display: grid;
    grid-template-columns: 1fr 3fr;
    gap: 1.5rem;
}

@media (max-width: 768px) {
    .workspace {
        grid-template-columns: 1fr;
    }
}
</style>
