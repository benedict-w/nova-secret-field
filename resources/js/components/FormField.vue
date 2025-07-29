<template>
    <DefaultField
        :field="currentField"
        :errors="errors"
        :show-help-text="showHelpText"
    >
        <template #field>
            <div class="flex items-center">
                <input
                    v-bind="extraAttributes"
                    class="w-full form-control form-input form-input-bordered mr-2"
                    @input="handleChange"
                    :value="value"
                    :id="currentField.uniqueKey"
                    :dusk="field.attribute"
                    :disabled="currentlyIsReadonly"
                    :list="`${field.attribute}-list`"
                    :type="type"
                />

                <!-- Toggle visibility icons -->
                <svg
                    v-if="masked"
                    @click="toggle"
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
                        d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7
                           a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243
                           M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29
                           m7.532 7.532l3.29 3.29M3 3l3.59 3.59
                           m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943
                           9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21"
                    />
                </svg>
                <svg
                    v-else
                    @click="toggle"
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6 cursor-pointer ml-2"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    stroke-width="2"
                >
                    <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" d="M2.458 12C3.732 7.943 7.523 5 12 5
                        c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7
                        -4.477 0-8.268-2.943-9.542-7z" />
                </svg>
            </div>

            <!-- Suggestions dropdown -->
            <datalist
                v-if="currentField.suggestions && currentField.suggestions.length > 0"
                :id="`${field.attribute}-list`"
            >
                <option
                    v-for="suggestion in currentField.suggestions"
                    :key="suggestion"
                    :value="suggestion"
                />
            </datalist>
        </template>
    </DefaultField>
</template>

<script>
import { DependentFormField, HandlesValidationErrors } from 'laravel-nova'

export default {
    mixins: [HandlesValidationErrors, DependentFormField],

    data() {
        return {
            masked: true,
        }
    },

    methods: {
        toggle() {
            this.masked = !this.masked;
        },
    },

    computed: {
        type() {
            return this.masked ? 'password' : 'text';
        },

        defaultAttributes() {
            return {
                min: this.currentField.min,
                max: this.currentField.max,
                step: this.currentField.step,
                pattern: this.currentField.pattern,
                placeholder: this.currentField.placeholder || this.field.name,
                class: this.errorClasses,
            }
        },

        extraAttributes() {
            return {
                ...this.defaultAttributes,
                ...this.currentField.extraAttributes,
            }
        },
    },
}
</script>
