【事前の予測】今回の単純な関数の要約と検証タスクであれば、HaikuとSonnetの間で出力結果に大差はないと予測する  
「Haikuの出力:」 
javascript
function getDiscount(price, code) {
  if (typeof price !== "number" || !Number.isFinite(price) || price < 0) {
    throw new TypeError("price は0以上の有限な数値である必要があります");
  }
  if (code != null && typeof code !== "string") {
    throw new TypeError("code は文字列である必要があります");
  }
  if (code === "SAVE10") return price * 0.9;
  if (code === "SAVE20") return price * 0.8;
  return price;
}
要約: 価格とクーポンコードを受け取り、SAVE10なら10%引き、SAVE20なら20%引きの価格を返し、それ以外のコードでは元の価格をそのまま返す関数です。
追加した検証は、価格が0以上の有限な数値か、コードが文字列(または未指定)かを確認し、違反時はTypeErrorを投げます。
入力検証を追加した版です。
「Sonnetの出力:」 
javascript
// 価格に割引コードを適用した金額を返す(SAVE10は10%引き、SAVE20は20%引き、それ以外は定価のまま)
function getDiscount(price, code) {
  if (typeof price !== "number" || !Number.isFinite(price) || price < 0) {
    throw new TypeError("price は0以上の有限な数値で指定してください");
  }
  if (typeof code !== "string") {
    throw new TypeError("code は文字列で指定してください");
  }

  const normalized = code.trim().toUpperCase();

  if (normalized === "SAVE10") return price * 0.9;
  if (normalized === "SAVE20") return price * 0.8;
  return price;
}
1行要約: 価格と割引コードを受け取り、SAVE10 なら10%引き、SAVE20 なら20%引きの金額を返し、未知のコードは定価をそのまま返す関数です。
追加した検証のポイント
price は数値で、有限(NaN や Infinity を除く)かつ0以上であることを確認します。
code は文字列であることを確認します。
コードは前後の空白を除き、大文字に揃えてから比較するため、" save10 " のような入力も受け付けます。大文字小文字を区別したい場合は normalized を code に戻してください。
不正な入力は黙って定価を返さず、TypeError を投げます。エラーにせず定価を返す方針がよければ、throw を return price に置き換えられます。
なお、関数名は getDiscount ですが、返すのは「割引額」ではなく「割引後の価格」です。混乱を避けるなら applyDiscount などへの改名も検討してみてください。

結果の比較と考察 
両モデルの出力を比較した結果、要約の内容や追加されたコードに実務上の大差はみられなかった。一度きりの利用ならSonnetを利用するのも良いが、200万回実行する業務を想定した場合、この微小な差のために高い料金を支払う価値はない。したがって、スケール時にはコストとスピードに優れるHaikuを採用すべきである。  
