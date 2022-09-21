# pc8001_1200bps
How to Modify your NEC PC-8801 hard and soft to support 1200bps CMT I/F

NEC PC-8001のカセットインターフェースを1200bpsをサポートするためのハードとソフトです。

ハードの改造は6本のジャンパ線の追加のみで、専用ソフト(mat)で300/600/1200bpsを切り替え可能です。

専用ソフトを使わない場合は無改造機と互換の動作をします。

おそらく、PC-8001のカセットインターフェースをスピードアップする方策の中で最安値です。

# docディレクトリ
ハード、ソフトの解説(JPEG形式8枚、1986/6/8)

# softwareディレクトリ
mon-EDD0.cmt サポートソフトmat　(cmt形式。エミュレータj80で読み込み確認済み)

mat.src matのソースコード　(Z80, ただし逆アセンブル結果から起こしたもので読みにくい)

# その他の補足
ドキュメントには書いてありませんが300bpsを選択するSコマンドが存在します。

# C, C+コマンドの補足
CコマンドはROMの内容をRAMに転送してから1.1相当にパッチを当ててRAMでN-BASICを動作させます。

C+コマンドはCコマンドの動作に加えて開始アドレスを8020Hから6020Hに切り換え、RAM40KモードでN-BASICを利用可能にします。

C, C+コマンドは以下の条件を満たしたメモリ拡張を使用している場合のみ使用できます。

・out $78,$80で0〜7fffhがRAMに切り替わる
・ROMモードで書き込みを行うとRAMに書き込む

ただし、この仕様を満たすPC-8001は川俣が自分で改造した1台きりと思われるので、matにパッチを当てる方が良いと思われます。

(パッチを当てるとセルフテストをパスしなくなるので、チェックサムが合うようにダミーを書き換える必要あり)

# matを書き換える場合の注意
matはワークエリアの未使用領域に強引に入り込むので、フリーエリアは減らしませんが、壊れやすいと言えます。ですので、セルフテストを行います。

セルフテストはコードのバイトの総和が0かどうかで判定しています。

総和を0にするためにダミーのバイトが入っています。

おそらく、ソース上の実行される可能性のないRRCAがそれにあたると思われます。

もし、コードを書き換えた場合はコードのバイトの総和が0になるように、この命令を書き換えます。

実行されないので、どんな値でも構いません。

# PC-8801のN-BASICモードで使う場合のヒント
以下の情報は無印PC-8801用です。他のモデルでも同じかは分かりません。

matは、PC-8801のN-BASICモードで1200bpsのロードセーブを行う手段として使用できます。その際改造は必要としません。

しかし、PC-8801のN-BASICモードで読み込むと使用ワークエリアの相違からセルフテストを通りません。

GEE98で直接起動すると動くことは動きます。

この時、セレクタの仕様の相違から速度選択のコマンドが変化します。

1200bpsはHコマンドではなくSコマンドで選択します。(無改造で1200bpsを使用できます)

PC-8801では300bpsはサポートしておらず使用できません。

# matのソースコードの注意
matはもともとハンドアセンブルで開発されていたので、このソースコードは逆アセンブル結果から起こしたものと思われます。

そのため、本来DB命令で書かれるべき部分がZ80のニモニックで書かれている箇所があり、読みにくいと言えます。

文字に相当するコードも16進数のままです。

そこは注意してご覧下さい。

コードの順番がごちゃごちゃなのも、既存のコードに追加する形でハンドアセンブルでコードを追加したためです。

# エミュレータでの使用について
T88, CMTなどの形式でデータを扱う場合は、WAV形式の音声データから変換する際に速度差を吸収してしまうので、matの1200bpsモードで保存したデータであっても、無修正のエミュレータで読み込めます。

つまり以下の流れは可能です。

1) matの1200bpsモードで保存
2) 音声データをWAVファイルに取り込み
3) Wav2T88でT88形式に変換
4) 無修正のj80でT88ファイルを読み込む
