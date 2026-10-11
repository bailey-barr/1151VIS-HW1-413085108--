<template>
    <div class="sankey-container">
        <h1>台灣區域人口遷移到四大新興城市</h1>
        <div class="subtitle">HW1:</div>
        <!-- 放置 SVG 圖表的容器 -->
        <div ref="chartContainer" id="chart"></div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from 'vue'
    import * as d3 from 'd3'
    // 【關鍵修正 1】明確從 d3-sankey 引入 sankeyRight
    import { sankey as d3Sankey, sankeyLinkHorizontal, sankeyRight } from 'd3-sankey'

    // 綁定 DOM 容器
    const chartContainer = ref(null)

    onMounted(() => {
    // 清空容器避免重複渲染
    d3.select(chartContainer.value).selectAll("*").remove()

    // 設定畫布尺寸
    const width = 900
    const height = 550
    const margin = { top: 20, right: 20, bottom: 20, left: 20 }

    // 建立 SVG
    const svg = d3.select(chartContainer.value)
    .append("svg")
    .attr("width", width)
    .attr("height", height)

    // 定義原始資料
    const data = {
    nodes: [
    { name: "台北市 (淨遷出)" },    // 0
    { name: "高雄市 (傳統移出)" },// 1
    { name: "其他縣市 (流出端)" },      // 2
    { name: "新北市 (接收衛星城)" },    // 3
    { name: "桃園市 (淨遷入榜首)" },    // 4
    { name: "台中市 (中部磁吸核心)" },  // 5
    { name: "新竹縣市 (竹科紅利)" }     // 6
    ],
    links: [
    { source: 0, target: 3, value: 28000 },
    { source: 0, target: 4, value: 15000 },
    { source: 1, target: 4, value: 12000 },
    { source: 1, target: 5, value: 14000 },
    { source: 2, target: 4, value: 8000 },
    { source: 2, target: 5, value: 9000 },
    { source: 2, target: 6, value: 11000 },
    { source: 1, target: 3, value: 10000 }
    ]
    }

    // 設定 Sankey 排版產生器
    const sankey = d3Sankey()
    .nodeId((d, i) => i) // 明確指定使用數字索引
    .nodeWidth(15)
    .nodePadding(20)
    .extent([[margin.left, margin.top], [width - margin.right, height - margin.bottom]])
    .nodeAlign(sankeyRight) // 【關鍵修正 2】移除 d3. 前綴，直接使用 sankeyRight

    // 【關鍵修正 3】深拷貝資料，防止 d3-sankey 竄改原始物件導致 Vue 渲染報錯
    const graphData = {
    nodes: data.nodes.map(d => ({ ...d })),
    links: data.links.map(d => ({ ...d }))
    }

    // 計算節點與連結位置 (改傳入拷貝後的 graphData)
    const { nodes, links } = sankey(graphData)

    // 色彩比例尺
    const color = d3.scaleOrdinal(d3.schemeCategory10)

    // 繪製連結 (Links)
    const link = svg.append("g")
    .selectAll(".link")
    .data(links)
    .join("path")
    .attr("class", "link")
    .attr("d", sankeyLinkHorizontal())
    .attr("stroke", d => color(d.source.index))
    .attr("stroke-width", d => Math.max(1, d.width))

    // 連結滑鼠提示
    link.append("title")
    .text(d => `${d.source.name} → ${d.target.name}\n遷移人數: ${d.value.toLocaleString()} 人`)

    // 繪製節點群組 (Nodes)
    const node = svg.append("g")
    .selectAll(".node")
    .data(nodes)
    .join("g")
    .attr("class", "node")

    // 節點矩形
    node.append("rect")
    .attr("x", d => d.x0)
    .attr("y", d => d.y0)
    .attr("height", d => Math.max(1, d.y1 - d.y0)) // 加入 Math.max 防止高度為負數或 0
    .attr("width", d => d.x1 - d.x0)
    .attr("fill", d => color(d.index))
    .attr("stroke", "#000")

    // 節點文字標籤
    node.append("text")
    .attr("x", d => d.x0 < width / 2 ? d.x1 + 6 : d.x0 - 6)
    .attr("y", d => (d.y1 + d.y0) / 2)
    .attr("dy", "0.35em")
    .attr("text-anchor", d => d.x0 < width / 2 ? "start" : "end")
    .text(d => d.name)

    // 節點滑鼠提示
    node.append("title")
    .text(d => `${d.name}\n總流量: ${d.value.toLocaleString()} 人`)
    })
</script>

<style scoped>
    .sankey-container {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        background-color: #f8f9fa;
        color: #333;
        padding: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    h1 {
        font-size: 24px;
        margin-bottom: 5px;
    }

    .subtitle {
        font-size: 14px;
        color: #666;
        margin-bottom: 20px;
    }

    #chart {
        background: #fff;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    }

    .node rect {
        cursor: move;
        fill-opacity: .9;
        shape-rendering: crispEdges;
        stroke: #333;
        stroke-width: 0.5px;
    }

    .node text {
        pointer-events: none;
        font-size: 12px;
        text-shadow: 0 1px 0 #fff;
    }

    .link {
        fill: none;
        stroke-opacity: .35;
    }

        .link:hover {
            stroke-opacity: .6;
        }
</style>