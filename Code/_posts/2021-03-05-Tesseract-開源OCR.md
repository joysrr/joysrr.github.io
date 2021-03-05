---
title: "Tesseract-開源OCR"
tagline: "文字辨識"
header:
  overlay_image: /assets/images/headerIMG01.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
classes: wide
excerpt: "Tesseract應用實例"
tags:
  - code
  - Tesseract
---

## 前言

前一陣子工作上老闆突然給了一個任務，因為使用者的懶惰，我們要協助將軟體上的文字透過Email傳送出去，方便他們辨識結果，Email這部分容易，但是要怎麼辨識文字呢?所以就被推坑拉...

## Tesseract是什麼?

Tesseract 原本是HP(惠普)公司進行研發，由Greeley Colorado 在 1985年 至 1994年 間進行維護，在1996年為了與Windows對接進行改版，並且於1998年使用C++改寫。
在2005年時，HP將Tesseract開源，現在由Google在2006年維護至今，目前版本已進入測試版(5.0)正式版(4.1.1)，並持續進行開發維護中。

[開源網址](https://github.com/tesseract-ocr/tesseract)

> 直接下結論了，由Google爸爸主導維護開發，相信一定有品質保證，可以放心使用了
> 希望不會落得跟Inbox以及其他上百個項目一樣的下場[Killed By Google](https://killedbygoogle.com/)

## 我的環境

網路上目前都有很多的範例，包含在各個平台的應用，還有Python上結合[OpenCV](https://opencv.org/)改善辨識的圖片，再丟到Tesseract進行辨識，目前我的目標先設定在建置(老闆是這樣跟我說的><)，其他的坑就有空再來補吧!

此文章目標是希望透過WebServices把圖片的Stream傳送到Server，然後Server再將結果回傳，環境為Window+WebForm。

> 幾十年的老架構了，好像換新的啊......

## 建置

因為GitHub上的內容要經過建置才能使用，這不符合我快速達成目標的方式，所以此篇就不先討論了，如果你想用Console進行使用，可以透過安裝檔將其安裝至windows的環境中，可以參考[Tesseract at UB Mannheim](https://github.com/UB-Mannheim/tesseract/wiki)，此GitHub有提供編譯好的安裝檔，只要設定好環境變數後就可以使用，而我這邊則是選擇Nuget直接將dll檔導入到我的專案中，單純的我以為這樣就可以了，但是因為我的Visual Studio專案是以網頁方式編譯，所以這個方法是不行的啊，我這邊手動把dll以及需要的內容移至Bin檔中，才終於可以，以下附上檔案(版本4.1.1)，直接放在Bin就可以囉~
[Tesseract-for-webform](https://github.com/joysrr/Tesseract-for-webform)
包含Tesseract.dll、資料夾x86、x64、tessdata。

> 哭啊 這個問題我測了半天才發現......

放上去後我們的起手式就完成了，接下來最重要的是OCR的訓練檔案，也就是上方有提到的tessdata資料夾的內容，這邊我就直接使用他們提供的檔案，你也可以自己訓練，這部分就沒有在此篇介紹，說不定哪天會補坑吧~
他們有提供BEST以及FAST的訓練資料可以下載，

1. [tessdate](https://github.com/tesseract-ocr/tessdata) => 測試後中文辨識最好
2. [tessdata_best](https://github.com/tesseract-ocr/tessdata_best)
3. [tessdata_fast](https://github.com/tesseract-ocr/tessdata_fast)

接下來依照要辨識的語言下載資料，英文是eng.traineddata，繁體中文是chi_tra.traineddata，還有直向版chi_tra_vert.traineddata，檔案放置好後一切準備就緒就可以開始Coding啦

## 程式碼

	[WebMethod(Description = "載入圖片讀取文字(OCR)")]
    public string LoadText(Stream imgStream, string lang="eng")
    {
		string mean = "", result = "";
        if (imgStream != null)
        {
            using (var engine = new TesseractEngine(Server.MapPath(@"~/Bin/tessdata"), lang, EngineMode.Default))
            {
                // have to load Pix via a bitmap since Pix doesn't support loading a stream.
                using (var image = new System.Drawing.Bitmap(imgStream))
                {
                    using (var pix = PixConverter.ToPix(image))
                    {
                        using (var page = engine.Process(pix))
                        {
                            mean = String.Format("{0:P}", page.GetMeanConfidence());
                            result = page.GetText();
                        }
                    }
                }
            }
        }
        return mean+","+result;
    }

## 結論
蛤? 程式碼怎麼就這樣?
沒錯就是這麼簡單，輕輕鬆鬆就可以開始使用辨識~

但其實後面還有無數個坑，可以辨識，不代表結果就是正確的，如果你是特定的環境下人為拍攝的照片，肯定會有很多需要克服的問題，需要透過OpenCV去除雜訊，甚至需要重新進行訓，在訓練之前最好是要先設定一個目標，畢竟訓練資料如果太雜亂，可能效果也不是很好，那麼我的Tesseract之旅大概就到這邊告一段落，弄了一天，之後再來研究一下OpenCV還有Training的東東吧!










<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg4OTgzNTA1NywtMTUzNTAzMDI2NywtMj
Y5MzA4NDk2LC0xNjMwNTQwOTkxLDExMTg0MDIxNCwxOTA0NTY1
ODg3XX0=
-->