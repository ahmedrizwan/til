# Selection Sort Algorithm

```kotlin
fun main(args: Array<String>) {
    val array = arrayOf(3, 2, 5, 1, 3, 5, 4, 6, 2, 3, 4, 6, 3, 3)
    selectionSort(array)
}

private fun selectionSort(array: Array<Int>) {
    array.forEachIndexed { i, _ ->
        var maxIndex = i
        (i until array.size)
                .filter { array[maxIndex] > array[it] }
                .forEach { maxIndex = it }
        // swap
        val current = array[i]
        array[i] = array[maxIndex]
        array[maxIndex] = current
    }
    array.forEach { println(it) }
}

```