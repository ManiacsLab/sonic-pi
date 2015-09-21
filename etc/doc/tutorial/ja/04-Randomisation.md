4 ランダム化

# ランダム化

音楽にすこし面白さを加えるために、ランダムという素晴らしい方法があります。
Sonic Piは音楽にランダム性を追加するためにいくつかの素晴らしい機能を持っていますが、
Sonic Piのランダムは、真のランダムではありません。これは一体何を意味しているのでしょう？
勉強を開始する前に、この衝撃的な真実を見ていきましょう。

## 再現性

たいへん便利なランダム関数に、二つの数字の間 （最小値と最大値）で
乱数（ランダムな値）が得られる`rrand`があります。（rrandはレンジド・ランダムの略です。） 
ランダムな音階を演奏してみましょう。

```
play rrand(50, 100)
```

おぉー、ランダムな音階を演奏しましたね。これは、音階`77.4407`を演奏しました。

50と100との間で素敵なランダム音階はでしたね...でも、ちょっと待ってください、
上記で、私はあなたが再生したランダムな音階を正確に予測していませんか？何か怪しくないですか？
再度コードを実行してみてください。ランダムのはずが、再び`77.4407`が選ばれましたよね？
実は、ランダムにすることができないのです！
この答えとなる擬似ランダムとは、真のランダムではないということです。SonicPiは、
反復可能な数としてのランダムを与えます。これは、あなたのマシンで作成した音楽が、誰か他の人のマシンで
再生されても同じように聞こえることを確認するのに大変便利な機能です。あなたの曲の中でいくつものランダ
ム性を使用している場合でも。

もちろん、音楽に与えられる側面として、もし「ランダム」が`77.4407`を毎回選択した場合、
それは非常に面白くありません。しかし、それはしていません。以下のことを試してみてください。

```
loop do
 play rrand(50, 100)
 sleep 0.5
end
```

そう！最終的には、ランダムに聞こえますね。ランダム関数へ続いて呼び出されるその後の実行結果は
ランダムな値を返します。ただし、また再生する場合は正確に乱数値の同じシーケンスを生成し、
まったく同じ音が鳴ります。Runボタンが押されるたびに、まるですべてのSonic Piコードが毎回、同じ時間に戻るかように蘇ります。

それはまさにシンセのデジャヴのように繰り返されます！

## ホーンテッド・ベル

ランダム動作を取り入れたゾクッとするようなベルの音を使った楽しい作例です。
繰り返しサンプルのベル音:perc_bellをループさせ、ベル音の再生速度と音の間のsleepにランダムな数値を用いています。

```
loop do
 sample :perc_bell, rate: (rrand 0.125, 1.5)
 sleep rrand(0.2, 2)
end
```

## ランダムなカットオフ

ランダム化のもう一つの楽しみ方の例は、ランダムにシンセのカットオフを加えることです。
これを試してみるのに絶好のシンセは、 :tb303 エミュレータです。

```
use_synth :tb303

loop do
 play 50, release: 0.1, cutoff: rrand(60, 120)
 sleep 0.125
end
```

## ランダムの種(シード)

もし、SonicPiが提供する乱数の特定の配列が気に入らない場合、`use_random_seed`を介すことで
別の開始点を選択することが可能です。シードの標準値は0であるため、異なるランダム体験のために
別のシード番号を入力してみましょう！

下記を考えてみてください：

```
5.times do
 play rrand(50, 100)
 sleep 0.5
end
```

このコードを実行するたびに、 
5音階の同じシーケンスが聞けるでしょう。異なるフレーズを聞くには、シードの値を変更します。

```
use_random_seed 40
5.times do
 play rrand(50, 100)
 sleep 0.5
end
```

こうして異なる5音階のシーケンスを生成します。シードの値を変更することによって、あなたの好きな
フレーズを見つけることができます 他の人と共有するとき、彼らも、あなたが聞いたものとまったく同様のフレーズを
聞くことができるでしょう。

有用なランダム関数をもう少し見ていきましょう。

## choose：選択

一般的な方法は、あらかじめ用意した数値をリストの中からランダムに選択することです。
例えば、60、65または72の中から1音を演奏することができます。`choose`を用いれば、
リストから一つの項目をで選択することができます。まず、カンマで区切った番号のリストを角括弧でラップ（包んで）し、
配置する必要があります：`[60, 65, 72]`。次にそれらを`choose`に渡す必要があります。

```
choose([60, 65, 72])
```

どんな音になるか聞いてみましょう。

```
loop do
 play choose([60, 65, 72])
 sleep 1
end
```

## rrand

すでに`rrand`について触れてきましたが、再び実行してみましょう。これは、2つの値の間の乱数（排他的）を返します。
この意味するところは上部または下部の番号いずれの値も含まれません。常に両者の間にある値です。
そして、その番号は常に浮動小数点になります - それは整数ではなく、分数です。

`rrand(20, 110)`で返される浮動小数点数の例：

* 20.343235
* 42.324324
* 100.93423

## rrand_i

時々、あなたは小数点ではなく、整数の乱数を望むこともあるでしょう。これは`rrand_i`を用いることで
解決できます。それは小数点を除いて`rrand`と同様に最小値および最大値の範囲（この場合、最長値と
最大値も含まれます）に潜在するランダム値を返す動作をします。下記は、`rrand_i(20, 110)`によって返される数値の例です。

* 20
* 46
* 99

## rand

`rand`は、`0`を含む最小値と最大値未満の間のランダムな浮動小数点数を返します。デフォルト（既定値）では
`0`と`1`の間の値を返します。したがって、`amp:`値をランダム化する際に便利です。

```
loop do
 play 60, amp: rand
 sleep 0.25
end
```

## rand_i

`rrand_i`と`rrand`の関係と同様に`rand_i`は`0`と特定の最大値の間の整数値を返します。

## dice:サイコロ

ランダムな数字を出す際に、サイコロ投げをまねてみたくなることもあるでしょう。
これは、常に下の値が1である`rrand_i`の特殊なケースです。
 `dice`を呼び出す時は、サイコロの面の数を指定する必要があります。
 標準的なサイコロは6面で、`dice(6)`では、1 、2 、3 、4 、5 、
 または6を返すサイコロと同様の作用をします。しかし、空想のボードゲームのように、
 4面、12面、または20面サイコロで値を見つけたいこともあるでしょう。ことによっては120面のサイコロだって！

## one_in

最後に、サイコロ投げをまねて、例えば６という一番大きな数値を出してみたくなることもあるでしょう。
`one_in`はサイコロの面の数分の1の確率でtrueを返します。したがって`one_in(6)`では6分1の確率でtrue、それ以外の場合はfalseを返します。`true`と`false`の値は、このチュートリアルの次のセクションで説明するif文で非常に有用です。

さあ、ランダム性を使いこなしてコードをまぜこぜにしていきましょう！