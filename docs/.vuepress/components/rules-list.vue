<template>
    <div>
        <div class="tools">
            <label>Filter by Config: </label>
            <select v-model="configName">
                <option value="all">ALL</option>
                <option value="json">plugin:jsonc/recommended-with-json</option>
                <option value="jsonc">
                    plugin:jsonc/recommended-with-jsonc
                </option>
                <option value="json5">
                    plugin:jsonc/recommended-with-json5
                </option>
            </select>
        </div>
        <slot />
    </div>
</template>

<script>
export default {
    name: "RulesList",
    data() {
        return {
            configName: "all",
        }
    },
    watch: {
        configName: {
            async handler() {
                await this.$nextTick()
                this.filterTable()
            },
            immediate: true,
        },
    },
    methods: {
        filterTable() {
            if (this.configName === "all") {
                for (const tr of this.$el.querySelectorAll("tbody > tr")) {
                    tr.style.display = ""
                }
            } else {
                for (const tr of this.$el.querySelectorAll("tbody > tr")) {
                    const cells = tr.querySelectorAll("td")
                    const cell =
                        this.configName === "json"
                            ? cells[3]
                            : this.configName === "jsonc"
                            ? cells[4]
                            : cells[5]
                    if (cell.textContent.trim()) {
                        tr.style.display = ""
                    } else {
                        tr.style.display = "none"
                    }
                }
            }
        },
    },
}
</script>

<style scoped>
.tools {
    display: flex;
    justify-content: flex-end;
}
</style>
