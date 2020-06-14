# 文字列とユニコード

## 目的

- JavaScriptにおいてUnicodeのUTF-16を意識しないと行けない場面を学ぶ
- 文字列がCode Unitが並んでいるという事実を知る

## 目的ではない

- Unicodeの歴史や用語を正確に学ぶこと

## 扱いきれなかった

- i18n
    - 文字列のID sortについて自然な文字列についてを知る
    - 文字列の自然な区切り


## アウトライン

- 文字列とUnicode
    - JavaScriptはUTF-16でエンコードするという話をしていました
    - JavaScriptでは内部コードとしてUTF-16でエンコードします
    - そのため、文字列のAPIもUTF-16で変換されたものを扱います
    - 多くのケースでは気にせずに文字列処理ができる
    - しかし、絵文字や文字数カウントでこれを意識しないと行けない場面がでてくる
    - この章ではそのようなケースを見ていく
    - 一方でUnicodeの解説するのが目的ではないので、最小限で
- Code Point
    - Unicodeはすべての文字(制御文字などの画面に表示されない文字も含む)に対してIDを定義する目的でつくられています。
    - この文字に対する一意のIDのことをCode Point（符号位置）と呼びます。
    - ES2015で追加された`String#codePointAt`メソッドを使うことで、文字列からのCode Pointの値（番号）を取得できます。
    - 例) 文字列 -> 符号位置
    - 一方で、Code Pointから文字列を作成することもできます。
    - 例) Code Point -> 文字列
    - また、ES2015からユニコードエスケープを使うことで、文字列中にCode Pointで文字を書けます。
    - 文字によってはFontが対応してないと正しく表示できない文字を文字列中に使いたい場合があります。
    - そのような際にはユニコードエスケープを使うことで安全に(どのエディタでも表示できるように)埋め込ます
    - 例) ユニコードエスケープ
- Code Unit !== Code Point
    - JavaScriptの内部表現はUTF-16で変換されたCode Unit(符号単位)で文字列を表現していることと紹介していました。
    - 多くのケースではCode PointとCode Unitは同じ値を返します。
    - 例) サロゲートペアじゃない文字列
    - しかし、UTF-16でのCode Unitのサイズは16bit(2バイト)です。
    - 現在Unicodeに収録されているCode Pointの数は10万種類を超えます。
    - 一方で2バイトで表現できる範囲は65536種類のみです。65,536 (= 2の16乗) 
    - 1つUnitですべての文字を表現することはできません
    - そのため、UTF-16では複数のCode Unitで1つのCode Pointを表現できます。
    - 例)
    - これがサロゲートペア
- サロゲートペア
    - サロゲートの仕組みを解説?
- ここまでのまとめ
    - 「文字列」は「Code Unit」が順番に並んだもの
    - 「文字列」は「Code Point」ごとに扱う方法が別途用意されている
    - このCode Pointごとに扱うメソッドなどはES2015から導入されたものが殆どです。
    - そのため、ES2015より前に実装されていたメソッドなどは基本的にCode Unitごとに文字列を扱っています。
    - Iterator（`for...of`や`Array.from`など）
    - メソッドに`CodePoint`という名前を含むもの
    - `u`（Unicode）フラグが有効化されている正規表現
- 文字列をCode Pointで扱う
    - 正規表現
        - ES2015で`/./u` `u` = unicode フラグ追加された
        - 例) `/(𩸽)のひらき/` -> から $1を取り出したいというとき
    - Iterator
        - Array.from、spread、for of
        - length
            - 複数のCode Unitで1文字を表現すると、`String#length`への影響もあります。
            - `length`の数はCode Unitの数です
            - そのため、先ほどの`length`の値は次のようになります。
            - 例)
            - Code Pointの数を数えることで、
        - for...of
- 未使用
    - Adbanced
        - ZWJによる装飾文字(絵文字のスタイル) - これはUnicode
        - 異体字セレクタ(variation selector) - これはUnicode
    - 文字列と自然なソート