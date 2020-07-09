<template>
    <div style="padding-top: 12px">
        <div style="color: limegreen">
            Test Vue Component
        </div>
        <div>
            Cell Id:
            <span style="font-weight: bold; color: red">{{cell.id}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Test",
        props: {
            cell: {
                type: Object
            }
        },
        created() {
            console.log(this.cell)
        }
    }
</script>

<style scoped>

</style>