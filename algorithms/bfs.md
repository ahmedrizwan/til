# Breadth First Search

```kotlin
fun main(args: Array<String>) {
    val graph = mutableMapOf<String, List<String>>()
    graph["a"] = listOf("b")
    graph["b"] = listOf("c", "e", "d")
    graph["c"] = listOf("d")
    graph["d"] = listOf("e")
    println(bfs("e", graph))
}

private fun bfs(name: String, graph: Map<String, List<String>>): Boolean {
    val myQueue = LinkedBlockingDeque<String>()
    val visited = mutableMapOf<String, Boolean>()
    var steps = 0
    val first = graph.entries.elementAt(0)
    myQueue.add(first.key)
    visited[first.key] = true

    while (myQueue.isNotEmpty()) {
        val deque = myQueue.pop()
        steps++
        println("Popping $deque")
        if (deque == name) {
            println("Steps $steps")
            return true
        }

        graph[deque]!!.forEach {
            if (!visited.getOrDefault(it, false)) {
                println("Adding $it")
                myQueue.add(it)
                visited[it] = true
            }
        }
    }

    return false
}
```