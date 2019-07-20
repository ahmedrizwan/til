# Binary Search Algorithm 

```kotlin
fun main(args: Array<String>) {
    val array = listOf(3, 2, 5, 1, 4, 6)

    val sortedArray = array.sorted()

    println(binarySearch(1, sortedArray))
    println(binarySearch(2, sortedArray))
    println(binarySearch(3, sortedArray))
    println(binarySearch(4, sortedArray))
    println(binarySearch(5, sortedArray))
    println(binarySearch(6, sortedArray))
    println(binarySearch(0, sortedArray))
}

private fun binarySearch(value: Int, array: List<Int>): Int {
    var startIndex = 0
    var endIndex = array.size - 1
    while (startIndex <= endIndex) {
        val middleIndex = startIndex + (endIndex - startIndex) / 2

        if (array[middleIndex] == value)
            return middleIndex
        if (array[middleIndex] < value)
            startIndex = middleIndex + 1
        else
            endIndex = middleIndex - 1
    }

    return -1
}
```