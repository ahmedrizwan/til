# Greedy Algorithm

```kotlin

fun main(args: Array<String>) {
    val stations = mutableMapOf<String, Set<String>>()
    stations["kone"] = setOf("id", "nv", "ut")
    stations["ktwo"] = setOf("wa", "id", "mt")
    stations["kthree"] = setOf("or", "nv", "ca")
    stations["kfour"] = setOf("nv", "ut")
    stations["kfive"] = setOf("ca", "az")

    val finalStations = mutableSetOf<String>()
    var bestStation: String?

    var statesNeeded = setOf("mt", "wa", "or", "id", "nv", "ut", "ca", "az")

    while (statesNeeded.isNotEmpty()) {
        bestStation = null
        var statesCovered = setOf<String>()
        stations.forEach { station, states ->
            val covered = statesNeeded.intersect(states)
            if (covered.size > statesCovered.size) {
                bestStation = station
                statesCovered = covered
            }
        }
        statesNeeded -= statesCovered
        finalStations.add(bestStation!!)
    }
    println(finalStations)
}

```