# Dijkstra Algorithm

```kotlin

fun main(args: Array<String>) {
    val graph = DijkstraGraph()
    graph.applyDijkstra()
    graph.printShortestPath("A", "B")
}

class DijkstraGraph {
    private val graph = mutableMapOf<String, MutableMap<String, Int>>()
    private val visited = mutableMapOf<String, Boolean>()
    private val parents = mutableMapOf<String, String>()
    private val costsMap = mutableMapOf<String, Int>()

    init {
        fillGraphWithNodes()
        fillCostsAndParentsMaps()
    }

    private fun fillGraphWithNodes() {
        val aNodes = mutableMapOf("B" to 6, "D" to 1)
        graph["A"] = aNodes
        val bNodes = mutableMapOf("A" to 6, "D" to 2, "E" to 2, "C" to 5)
        graph["B"] = bNodes
        val dNodes = mutableMapOf("A" to 1, "B" to 2, "E" to 1)
        graph["D"] = dNodes
        val eNodes = mutableMapOf("D" to 1, "B" to 2, "C" to 5)
        graph["E"] = eNodes
        val cNodes = mutableMapOf("E" to 5, "B" to 5)
        graph["C"] = cNodes
    }

    private fun fillCostsAndParentsMaps() {
        val start = graph.entries.elementAt(0)
        costsMap[start.key] = Int.MAX_VALUE
        visited[start.key] = true
        start.value.forEach {
            costsMap[it.key] = it.value
            parents[it.key] = start.key
        }
        graph.forEach {
            if (start.key != it.key) {
                if (!costsMap.keys.contains(it.key)) {
                    costsMap[it.key] = Int.MAX_VALUE
                }
            }
        }
    }

    fun applyDijkstra() {
        var node = findTheLowestCostNode()
        while (node != null) {
            val cost = node.value
            val neighbors = getNeighbors(node)
            neighbors.forEach {
                val newCost = cost + it.value
                if (newCost < costsMap[it.key]!!) {
                    costsMap[it.key] = newCost
                    parents[it.key] = node!!.key
                }
            }
            visited[node.key] = true
            node = findTheLowestCostNode()
        }
        println(parents)
    }

    private fun findTheLowestCostNode(): MutableMap.MutableEntry<String, Int>? {
        var lowestNode: MutableMap.MutableEntry<String, Int>? = null
        var lowestCost = Int.MAX_VALUE

        costsMap.entries.forEach {
            if (!visited.getOrDefault(it.key, false) && lowestCost > it.value) {
                lowestNode = it
                lowestCost = it.value
            }
        }

        return lowestNode
    }

    private fun getNeighbors(node: MutableMap.MutableEntry<String, Int>): MutableSet<MutableMap.MutableEntry<String, Int>> {
        return graph[node.key]!!.entries
    }

    fun printShortestPath(startingNode: String, keyWeSearchedFor: String) {
        val path = mutableListOf<String>()
        path.add(keyWeSearchedFor)
        var node = parents[keyWeSearchedFor]
        while (node != null) {
            path.add(node)
            if (node == startingNode) {
                break
            }
            node = parents[node]
        }

        path.reverse()
        println(path)
    }

}
```