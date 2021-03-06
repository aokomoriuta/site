# zoned_time
* chrono[meta header]
* std::chrono[meta namespace]
* class[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::chrono {
  template <class Duration, class TimeZonePtr = const time_zone*>
  class zoned_time;

  using zoned_seconds = zoned_time<seconds>;
}
```
* time_zone[link time_zone.md]

## 概要
`zoned_time`は、タイムゾーンを考慮した時間軸上の一点を表す型である。この型は、テンプレートパラメータとして時間間隔をとる。

秒単位の時間間隔を扱う別名として、`zoned_seconds`も定義される。

`zoned_time`は[`time_point`](time_point.md)と[`time_zone`](time_zone.md)の組である。有効なタイムゾーンを常にもち、あいまいなタイムゾーンを参照するようなことにはならないという不変条件をもつ。

このクラスを介することで、UTCタイムゾーンをもつシステム時間を指定したタイムゾーンのローカル時間に変換でき、またその逆の変換もできる。

このクラスに対する出力ストリームの演算子は、タイムゾーンを考慮したローカル時間を出力するため、単にタイムゾーンを考慮した日時を出力したい場合にも使用できる。


## 適格要件
- テンプレートパラメータ`Duration`が[`duration`](duration.md)クラスの特殊化であること


## メンバ関数
### 構築／コピー／破棄

| 名前 | 説明 | 対応バージョン |
|------|------|----------------|
| [`(constructor)`](zoned_time/op_constructor.md) | コンストラクタ | C++20 |
| [`operator=`](zoned_time/op_assign.md) | 代入演算子 | C++20 |


### 変換

| 名前 | 説明 | 対応バージョン |
|------|------|----------------|
| [`operator sys_time`](zoned_time/op_sys_time.md)     | [`sys_time`](sys_time.md)型への変換演算子 | C++20 |
| [`operator local_time`](zoned_time/op_local_time.md) | [`local_time`](local_time.md)型への変換演算子 | C++20 |


### 観測

| 名前 | 説明 | 対応バージョン |
|------|------|----------------|
| [`get_time_zone`](zoned_time/get_time_zone.md)   | タイムゾーンを取得する | C++20 |
| [`get_local_time`](zoned_time/get_local_time.md) | ローカル時間を取得する | C++20 |
| [`get_sys_time`](zoned_time/get_sys_time.md)     | システム時間を取得する | C++20 |
| [`get_info`](zoned_time/get_info.md)             | 設定されたタイムゾーンの情報を取得する | C++20 |


## 非メンバ関数
### 比較演算

| 名前 | 説明 | 対応バージョン |
|------|------|----------------|
| [`operator==`](zoned_time/op_equal.md) | 等値比較を行う | C++20 |
| `template<class Duration1, class Duration2, class TimeZonePtr>`<br/> `bool operator!=(const zoned_time<Duration1, TimeZonePtr>& x, const zoned_time<Duration2, TimeZonePtr>& y);` | 非等値比較を行う (`==`により使用可能) | C++20 |


### 入出力

| 名前 | 説明 | 対応バージョン |
|------|------|----------------|
| [`operator<<`](zoned_time/op_ostream.md) | 出力ストリームに出力する | C++20 |


## 推論補助

| 名前 | 説明 | 対応バージョン |
|---------------------------------------------|------------------------------------|-------|
| [`(deduction_guide)`](zoned_time/op_deduction_guide.md) | クラステンプレートの推論補助 | C++20 |


## 例
```cpp example
#include <iostream>
#include <chrono>

namespace chrono = std::chrono;

int main()
{
  // システム時間はUTCタイムゾーンをもつ
  auto now = chrono::system_clock::now();

  // タイムゾーン情報なしで日時を出力する
  // (ローカルタイムゾーンへの変換はしてくれないので、デフォルトではUTCタイムゾーンで出力される)
  std::cout << now << std::endl;

  // タイムゾーン付きで日時を出力する
  std::cout << chrono::zoned_time{now} << std::endl;               // デフォルトタイムゾーン (UTC)
  std::cout << chrono::zoned_time{"Asia/Tokyo", now} << std::endl; // 日本 (UTC + 9時間)
  std::cout << chrono::zoned_time{"UTC", now} << std::endl;        // UTC

  // コンピュータに設定されているタイムゾーンで、日時を出力する
  std::cout << chrono::zoned_time{chrono::current_zone(), now} << std::endl;

  // UTCタイムゾーンのシステム時間を、日本のローカル時間に変換
  chrono::local_time lt = chrono::zoned_time{"Asia/Tokyo", now}.get_local_time();
  std::cout << lt << std::endl;
}
```
* chrono::zoned_time[color ff0000]
* chrono::system_clock[link system_clock.md]
* now()[link system_clock/now.md]
* chrono::current_zone()[link current_zone.md]
* chrono::local_time[link local_time.md]
* get_local_time[link zoned_time/get_local_time.md]

### 出力例
```
2019-12-20 10:05:05
2019-12-20 10:05:05.330140 UTC
2019-12-20 19:05:05.330140 JST
2019-12-20 10:05:05.330140 UTC
2019-12-20 19:05:05.330140 JST
2019-12-20 19:05:05
```

## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): (9.0時点で実装なし)
- [GCC](/implementation.md#gcc): (9.2時点で実装なし)
- [Visual C++](/implementation.md#visual_cpp): (2019 Update 3時点で実装なし)

