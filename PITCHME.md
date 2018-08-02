# HelloCoreML

オフラインでも動く人工知能iOSアプリ

---

## 自己紹介

![icon](https://user-images.githubusercontent.com/16277668/41948274-ebbee344-79f6-11e8-8f2a-57fe6e2bf4ee.jpg)  
白井 誠 (shirai makoto)

普段はiPhoneアプリの開発をしています。

- GitHub: [@shirai](https://github.com/shirai)
- Qiita: [@sirai](https://qiita.com/sirai)
- Twitter: [@shirai_makoto](https://twitter.com/shirai_makoto)

---

## 本日のお話

iOSチームの開発リーダーとして  
日々最新OSバージョンのキャッチアップに努めています。  
今回はその活動内で得た知見の共有を  
させていただこうと思います。

いくつかネタはあるのですが、  
その中でも「ホットでキャッチー」な

- **iOSで人工知能（画像解析）アプリを作る**

というお話をしたいと思います。

---

## 話さないこと

時間も限られているので  
今回は以下の内容には触れません。

- 人工知能・機械学習とは？という話
- 自然言語解析など
- サーバーサイド機械学習（[IBM Watson](https://developer.apple.com/ibm/)とか）
- プロダクトに適用する上での課題とか  
  （そもそもまだ研究段階なので。。）

---

## 目次

1. iOSで使える人工知能の仕組みについて
2. 今できること（iOS11）
3. これからできるようになること（iOS12〜）

---

## iOSで使える人工知能の仕組みについて

---

<img src="https://devimages-cdn.apple.com/assets/elements/icons/core-ml/core-ml-128x128_2x.png" width="100px" />

iOS11で新機能 **「Core ML」** が登場し、  
iPhoneだけで動く人工知能アプリが作成可能になりました

---

### Core ML利用のメリット

- 人工知能・機械学習の専門知識  
  → 不要
- 外部サーバーとの通信  
  → 不要
  - オフラインでも利用可能
  - 個人情報の送信なども発生しない

---

### Core ML利用のデメリット

- 学習モデルの容量分、アプリの容量が大きくなる
- 消費電力が大きい（らしい）

---

### Core MLを拡張するツール

**Vision**

Visionは画像を解析する枠組みで、その結果をCore MLに送れます。  
画像を識別するための学習モデルを用意すれば、画像から撮影した場所（例：空港・寝室・森・海岸）や、主な被写体（例：木・動物・食品・車・人）などを判別できます。

---

## 作ったもの

画像を選択 → Core MLに渡す → 「African Elephant」の画像という結果を得る

<img src="https://user-images.githubusercontent.com/16277668/43588161-d973f59a-96a6-11e8-9ac4-065edbedeb04.gif">

---

<img width="817" alt="2018-08-02 22 59 49" src="https://user-images.githubusercontent.com/16277668/43588612-d3831480-96a7-11e8-843d-6b50c1820e23.png">
<img width="820" alt="2018-08-02 22 59 29" src="https://user-images.githubusercontent.com/16277668/43588611-d34ead80-96a7-11e8-8519-64ba90b82af8.png">

## 書いたコード

諸々パフォーマンス気を気にせず「画像解析するだけ」なら10行程度書けば実装できます。

```swift
/// 画像を予測する
/// - Parameter inputImage: 解析対象の画像
private func predict(inputImage: UIImage) {
  myTextView.text = ""
  let model = try! VNCoreMLModel(for: Resnet50().model)
  let request = VNCoreMLRequest(model: model) { request, error in
    for result in request.results as! [VNClassificationObservation] {
      let per = Int(result.confidence * 100)
      self.myTextView.text.append("これは\(per)%の確率で\(result.identifier)です。\n")
    }
  }
  let ciImage = CIImage(image: inputImage)!
  try? VNImageRequestHandler(ciImage: ciImage).perform([request])
}
```

## 今できること（iOS11）

---

**学習することはできない**

「学習モデルを作る」ことができず、  
「用意した学習モデルに対してデータを流して解析させる」  
ことしかできないのが現状です。

---

## これからできるようになること（iOS12〜）

**学習することができるようになる**

「学習モデルを作る」ことができるようになります！

Create ML

---

### おわり


---

おまけ: 参考サイト

- [機械学習 \- Apple Developer](https://developer.apple.com/jp/machine-learning/)


---

tempItems

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/African_Bush_Elephant.jpg/440px-African_Bush_Elephant.jpg" width="100px" /> → 
<img width="130px" alt="african elephant" src="https://user-images.githubusercontent.com/16277668/43587929-4aced166-96a6-11e8-95fa-eddaa7273679.png">

