# トランスファー

<hr>

## ブロック転送

ブロック転送は、座席内転送とも呼ばれ、トリップのセットが次の条件を満たす場合に利用できます：

1. トリップが連続している。
2. 同じ車両が両方のトリップを運行する。
3. トリップは、トランジットフィードの[trips.txt](../../reference/#tripstxt)ファイルで同じ`block_id`値でプロビジョニングされます。

### ブロック転送を有効にするには、`block_id`を使用します。

ブロック転送は、異なるルート、またはルートがループラインの場合は同じルートで、連続したトリップ間で行うことができます。`block_id`フィールドを使用して、どのトリップが 1 つのブロックに含まれ、座席内移動が利用可能なオプションであるかを指定します。

例えば、以下の[trips.txt](../../reference/#tripstxt)と[stop_times.txt](../../reference/#stoptimestxt)の値を考えてみましょう：

[**trips.txt**](../../reference/#tripstxt)

| route_id | trip_id     | block_id |
| -------- |-------------|----------|
| RouteA     | RouteATrip1 | Block1   |
| RouteB     | RouteBTrip1 | Block1   |

[**stop_times.txt**](../../reference/#stoptimestxt)

| trip_id   | arrival_time     | departure_time     | stop_id | stop_sequence |
| --------- | -------- | -------- | ------- | ------------- |
| RouteATrip1 | 12:00:00 | 12:01:00 | A       | 1             |
| RouteATrip1 | 12:05:00 | 12:06:00 | B       | 2             |
| RouteATrip1 | 12:15:00 |          | C       | 3             |
| RouteBTrip1 |          | 12:18:00 | C       | 1             |
| RouteBTrip1 | 12:22:00 | 12:23:00 | D       | 2             |
| RouteBTrip1 | 12:30:00 |          | E       | 3             |

この例では

- 停留所Aから停留所Eまでのルートを検索したユーザーは、ルートAで12:00に停留所Aで乗船し、`RouteATrip1`の終了後に停留所Cに到着しても車両に留まるよう指示されます。これは、同じ車両が`RouteBTrip1`をRouteBに対してサービスするためである。
- `RouteATrip1`のお客様で、`RouteBTrip1`の停留所に進みたいお客様は、この乗り換えのために車両に留まることができます。
- 同じルートで他の車両を利用する場合は、車両が異なるため、このような選択肢はありません。

### ループ線でのブロック移動

ループ線では、トリップの始発駅と終着駅は同じであり、`stop_id`同じである。このとき、同じ`block_id`の車両が連続する場合、ブロック乗換または座席乗換が可能となり、次のループに進む際、最初のループの乗客がそのまま車両に残ることができます。