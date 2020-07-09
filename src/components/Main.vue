<template>
    <div id="graph" ref="graph" class="graph">

    </div>
</template>

<script>
    import Test from "./test";
    import Vue from 'vue'

    const mxgraph = require("mxgraph")({
        mxImageBasePath: "./src/images",
        mxBasePath: "./src"
    });
    const {mxGraph, mxClient, mxCodec, mxUtils, mxConstants, mxPerimeter} = mxgraph;

    export default {
        name: "Main",
        components:{
            Test
        },
        data() {
            return {}
        },
        computed: {},
        mounted() {
            let graph = new mxGraph(this.$refs.graph);
            window.graph = graph;
            graph.recursiveResize = true;

            graph.isCellResizable = function (cell) {
                return cell.parent.geometry === undefined
            };

            graph.isCellEditable = graph.isCellResizable;
            graph.isCellSelectable = graph.isCellResizable;

            graph.convertValueToString = function (cell) {
                if (cell.insertComponent) {
                    const div = document.createElement('div');
                    div.setAttribute("class", "value");
                    div.setAttribute("style", "text-align: left");
                    const span = document.createElement("span");
                    span.innerHTML = "input: ";
                    div.appendChild(span);

                    const input = document.createElement('input');
                    input.setAttribute('type', 'text');
                    input.setAttribute('style', 'width:100px');
                    div.appendChild(input);

                    const testVue = document.createElement("div");
                    testVue.setAttribute("id", cell.id);
                    div.appendChild(testVue);

                    return div;
                }
                return cell.value;
            };


            let parent = graph.getDefaultParent();
            graph.getModel().beginUpdate();
            try {
                const node1 = graph.insertVertex(parent, "node1", '', 400, 300, 200, 100, "rounded=0;whiteSpace=wrap;html=1;fillColor=none;strokeColor=none;horizontal=1;shadow=1;glass=1;comic=0;");
                const top1 = graph.insertVertex(node1, "top1", 'Title', 0, 0, 200, 30, "constituent=1;fontSize=14;fontColor=#ffffff;rounded=0;whiteSpace=wrap;html=1;fillColor=#3399FF;");
                const bottom1 = graph.insertVertex(node1, "bottom1", 'Content', 0, 30, 200, 70, "constituent=1;rounded=0;whiteSpace=wrap;html=1;fillColor=#FFFFFF;fontSize=14;fontColor=#000000;");
                top1.setConnectable(false);
                bottom1.setConnectable(false);
                bottom1.insertComponent = true;

                const node2 = graph.insertVertex(parent, "node2", '', 400, 600, 200, 100, "rounded=0;whiteSpace=wrap;html=1;fillColor=none;");
                const top2 = graph.insertVertex(node2, "top2", 'Title', 0, 0, 200, 30, "constituent=1;fontSize=14;fontColor=#ffffff;rounded=0;whiteSpace=wrap;html=1;fillColor=purple;");
                const bottom2 = graph.insertVertex(node2, "bottom2", 'Content', 0, 30, 200, 70, "constituent=1;fontSize=14;fontColor=#00000;rounded=0;whiteSpace=wrap;html=1;fillColor=#FFFFFF;");
                top2.setConnectable(false);
                bottom2.setConnectable(false);
                bottom2.insertComponent = true;

                const edge = graph.insertEdge(parent, null, null, node1, node2)
            } finally {
                graph.getModel().endUpdate();
            }
            for (const key in graph.getModel().cells) {
                const cell = graph.getModel().cells[key];
                if (cell.insertComponent) {
                    const component = new Vue({
                        render: h => h(Test, {
                            props: {
                                cell
                            }
                        }),
                    }).$mount(`#${cell.id}`);
                }
            }
        },
        methods: {}
    }
</script>

<style scoped>
    .graph {
        width: 100%;
        height: 100vh;
    }
    .graph >>> foreignObject {
        justify-content: left;
    }
</style>