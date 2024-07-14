## Scan Event URL
- https://partners-dev.bodidata.com/licensee/bodidata/operator/scan-event/20I4xcefNfL8OLWXX7ZKl/check-in

# Using redis local without low priority write
- chunk 1/147: 2.924s (chunk of 10 employees, 5 chunks are processed at the same time)
- start getting employees' data: 1:26.960 (m:ss.mmm) (1470 employees)
### Code
```ts
function cacheStorageAsyncFunction<T extends AsyncFunction>(
  redis: RedisClient,
  targetFn: T,
  keyFn: (...args: Parameters<T>) => string,
) {
  return async (...args: Parameters<T>): Promise<Awaited<ReturnType<T>>> => {
    const key = keyFn(...args);
    const cached = await redis.get(key);

    if (cached) {
      return JSON.parse(cached);
    }

    const result = await targetFn(...args);
    await redis.set(key, JSON.stringify(result));

    return result;
  };
}
```
# Using mongodb local with low priority write
- chunk 1/147: 2.913s (chunk of 10 employees, 5 chunks are processed at the same time)
- start getting employees' data: 1:27.801 (m:ss.mmm)
### Code
```ts
function cacheStorageAsyncFunction<T extends AsyncFunction>(
  loader: DataLoader<string, Document>,
  item: Item<Document>,
  fn: T,
  mapKey: (...args: Parameters<T>) => string,
) {
  return async function (this: any, ...args: Parameters<T>) {
    const key = mapKey(...args);

    try {
      const value = await loader.load(key);
      if (value) {
        // console.log("cache hit", key);
        return value;
      }
    } catch {
      // console.log("cache miss", key);

      const result = (await fn.apply(this, args)) as ReturnType<T>;
      item.lowerPrioritySet(key, result);
      return result;
    }
  };
}```
# Using mongodb local with priority write
- chunk 1/147: 3.284s (chunk of 10 employees, 5 chunks are processed at the same time)
- start getting employees' data: 1:18.570 (m:ss.mmm)
### Code
```ts
function cacheStorageAsyncFunction<T extends AsyncFunction>(
  loader: DataLoader<string, Document>,
  item: Item<Document>,
  fn: T,
  mapKey: (...args: Parameters<T>) => string,
) {
  return async function (this: any, ...args: Parameters<T>) {
    const key = mapKey(...args);

    try {
      const value = await loader.load(key);
      if (value) {
        // console.log("cache hit", key);
        return value;
      }
    } catch {
      // console.log("cache miss", key);

      const result = (await fn.apply(this, args)) as ReturnType<T>;
      await item.set(key, result);
      return result;
    }
  };
}```