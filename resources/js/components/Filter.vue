<template>
    <div>
        <h3 class="text-sm uppercase tracking-wide text-80 bg-30 p-3">
            {{ filter.name }}
        </h3>

        <div class="p-2">
            <select-multiple
                :dusk="filter.name + '-filter-select'"
                class="block w-full form-control-sm form-select"
                :options="filter.options"
                :value="value"
                @change="handleChange"
            />
        </div>

    </div>
</template>

<script>
    import SelectMultiple from './SelectMultiple'

    export default {
        components: {
            SelectMultiple
        },

        props: {
            filterKey: {
                type: String,
                required: true,
            },
            resourceName: {
                type: String,
                required: true,
            },
        },

        methods: {
            handleChange(value) {
                //No need to update state if value is same as current value
                if(value === this.value) {
                  return
                }
                this.$store.commit(`${this.resourceName}/updateFilterState`, {
                    filterClass: this.filterKey,
                    value: value,
                })

                this.$emit('change')
            },
        },

        computed: {
            filter() {
                return this.$store.getters[`${this.resourceName}/getFilter`](this.filterKey)
            },

            value() {
                return this.filter.currentValue
            },
        },
    }
</script>
