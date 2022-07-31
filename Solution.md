# Note by speed
- when something wrong, vulbr will just panic!.

## 必須課題1: HTML

H1と同様にH2も書けば良い. サイズが違うだけ. x-largeとする.

## 必須課題2: 自由発表


## 追加課題1: CSS
layout/color.rsに#ffffffの形式を"black"などに変換する関数が定義されているため, それを用いれば良い.

## 追加課題2: JavaScript
3-2.html
```
VariableDeclaration { declarations: [Some(VariableDeclarator { id: Some(Identifier("target")), init: Some(CallExpression { callee: Some(MemberExpression { object: Some(Identifier("document")), property: Some(Identifier("getElementById")) }), arguments: [Some(StringLiteral("target"))] }) })] }
```
以上のjavascript astを, exal()で再帰的に剥がしていきながら評価する.

MemberExpressionのobjectで"document"があるため,
```
 return Some(RuntimeValue::HtmlElement {
                                object: self.dom_root.clone().expect("failed to get root node"),
                                property: Some(property_value.to_string()),
                            });
                        }
```
CallExpressionの引数にHtmlElementが返される.


### 脆弱性ポイント
innerHTMLを使っているため, 任意のHTMLを埋め込むことができる. つまり, injection系の攻撃ができてしまう.
https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML

3-3.html
```
---------- document object model (dom) ----------
Document
  Element(Element { kind: Html, attributes: [] })
    Element(Element { kind: Head, attributes: [] })
      Element(Element { kind: Style, attributes: [] })
        Text(".blue {\n      color: blue;\n    }\n  ")
    Element(Element { kind: Body, attributes: [] })
      Element(Element { kind: H1, attributes: [] })
        Text("Hello World!")
      Element(Element { kind: P, attributes: [Attribute { name: "class", value: "blue" }] })
        Text("This is a test page for JavaScript.")
      Element(Element { kind: P, attributes: [Attribute { name: "id", value: "target" }] })
        Text("abc")
      Element(Element { kind: Script, attributes: [Attribute { name: "type", value: "text/javascript" }] })
        Text("var target=document.getElementById(\"target\");\n\n    console.log(target);\n\n    target.innerHTML=\" \n  \n\n\n")
```
domを作成する時点でtarget.innerHTMLが崩れているため, DOM constructorも変えないといけない.
"<"があることが原因で, 間違った処理をしてしまっている.


## 追加課題3: ネットワーク

## 追加課題4: CWE

## 追加課題5: 自由発表
