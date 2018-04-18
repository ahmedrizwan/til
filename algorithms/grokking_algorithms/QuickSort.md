# QuickSort Algorithm

```kotlin
fun main(args: Array<String>) {
    val array = listOf(3, 2, 5, 1, 3, 5, 4, 6, 2, 3, 4, 6, 3, 3)
    println(quickSort(array))
}

private fun quickSort(array: List<Int>): List<Int> {
    if (array.isEmpty()) return listOf()

    val pivot = array[0]
    val lessThanPivot = mutableListOf<Int>()
    val greaterThanPivot = mutableListOf<Int>()

    (1 until array.size)
            .map { array[it] }
            .forEach {
                if (it < pivot) {
                    lessThanPivot.add(it)
                } else if (it >= pivot) {
                    greaterThanPivot.add(it)
                }
            }

    return quickSort(lessThanPivot) + pivot + quickSort(greaterThanPivot)
}
```