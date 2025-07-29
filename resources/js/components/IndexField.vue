<template>
    <div class="flex items-center">
        <template v-if="hasValue">
            <div v-if="field.asHtml" class="flex-grow" @click.stop v-html="value"></div>
            <span v-else class="whitespace-nowrap text-90 flex-grow">{{ value }}</span>

            <svg
                v-if="field.showCopyToClipboard"
                @click.stop="copyToClipboard"
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6 cursor-pointer ml-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
            >
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1
                       M8 5a2 2 0 002 2h2a2 2 0 002-2
                       M8 5a2 2 0 012-2h2a2 2 0 012 2
                       m0 0h2a2 2 0 012 2v3
                       m2 4H10m0 0l3-3m-3 3l3 3"
                />
            </svg>

            <svg
                v-if="masked"
                @mousedown.stop="toggle"
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6 cursor-pointer ml-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
            >
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M13.875 18.825A10.05 10.05 0 0112 19
                       c-4.478 0-8.268-2.943-9.543-7
                       a9.97 9.97 0 011.563-3.029
                       m5.858.908a3 3 0 114.243 4.243
                       M9.878 9.878l4.242 4.242
                       M9.88 9.88l-3.29-3.29
                       m7.532 7.532l3.29 3.29
                       M3 3l3.59 3.59
                       m0 0A9.953 9.953 0 0112 5
                       c4.478 0 8.268 2.943 9.543 7
                       a10.025 10.025 0 01-4.132 5.411
                       m0 0L21 21"
                />
            </svg>

            <svg
                v-else
                @mouseup.stop="toggle"
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6 cursor-pointer ml-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
            >
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                />
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M2.458 12C3.732 7.943 7.523 5 12 5
                       c4.478 0 8.268 2.943 9.542 7
                       -1.274 4.057-5.064 7-9.542 7
                       -4.477 0-8.268-2.943-9.542-7z"
                />
            </svg>
        </template>
        <p v-else>&mdash;</p>
    </div>
</template>

<script>
export default {
    props: ['resourceName', 'field'],

    data() {
        return {
            masked: true,
        }
    },

    methods: {
        toggle() {
            this.masked = !this.masked;
        },
        async copyToClipboard() {
            try {
                await navigator.clipboard.writeText(this.field.value);
                Nova.$toasted.show(this.field.copiedMsg, { type: 'success' });
            } catch {
                Nova.$toasted.show(this.field.failedMsg, { type: 'error' });
            }
        },
    },

    computed: {
        value() {
            return this.masked
                ? String(this.field.value).replace(/./g, '•')
                : String(this.field.value);
        },
        hasValue() {
            return this.field.value !== null && this.field.value !== '';
        },
    },
}
</script>
