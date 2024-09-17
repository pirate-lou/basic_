- `ILookup<K, T> ToLookup(this IEnumerable<T> items, Func<T, K> keySelector)`
- `ILookup<K, V> ToLookup(this IEnumerable<T> items, Func<T, K> keySelector, Func<T, V> valueSelector)`

Lookup по неизвестному ключу возвращает пустую коллекцию. Часто это удобнее, чем поведение Dictionary, который в такой ситуации бросает исключение.