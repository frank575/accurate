# accurate.sass

[詳細語法教學/範例盡在 codepen](https://codepen.io/frank575/pen/MWwqqgm?editors=1000)

> 準確快速的響應式數據驅動 sass 庫 
(scss, styled-component 雙版本皆棄更(冗長))

## 優勢

(不看切版畫面也可以達到 90% 準確度) 

1. 易學習 
2. 撰寫快 
3. 代碼少 
4. 易維護 
5. 可響應 
6. 精確高 
7. 實例多 

## 更新紀錄

- v3.0.0 第一版完成

## 語法

### init(初始化設定)

```sass
// 初始化設定
// 設計稿是否有滾動條
$HAS_SCROLLBAR: true 
// 面相切版的最外寬度
$DOCUMENT_WIDTH: 1920
// 面相切版的壓縮寬度，可能作圖 1920，但是用 1366 尺寸做，可以設定成 1366
// 這樣 $SCALE 就可以直接壓縮圖片尺寸，符合壓縮結果
$scaleWidth: 1920
@if $HAS_SCROLLBAR == true
  $scaleWidth: $scaleWidth - 17
// 內原始寬
$ORIGIN_SCALE_WIDTH: $scaleWidth
// 圖片路徑
$IMG_LOCAL: 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/1451969/'
// 參閱 bImgT, aImgT, bgImgT 三個語法
$IMG_TYPE: '.png'
// 尺寸是否絕對精確
$exact: false
// 比例尺，決定 =fz =lh =ls =imgW imgW() 壓縮比例，使用度不高
$SCALE: _decimal($ORIGIN_SCALE_WIDTH / $DOCUMENT_WIDTH)
```

---

### 可用參數

```sass
$xl: 1200
$lg: 992
$md: 768
$sm: 576
$ss: 320
```

---

### rwd

```sass
// =rwd(最大寬度)
// rwd max-width
=rwd($rwdMaxWidth: $md)
 
// =rwd(最小寬度)
// rwd min-width
=rwdMin($rwdMinWidth: $md + 1)
 
// =rwd(最小寬度, 最大寬度)
// rwd min-width and max-width
=rwdMinMax($rwdMinWidth: $sm + 1, $rwdMaxWidth: $md)
```

---

### 寬度布局(主要語法)

```sass
// W(寬度)
// 會全域修改 $scaleWidth，也就是 =w 的第二個參數
// 寬度重置及汙染，請搭配 RE 謹慎使用
W($width)
 
// 還原 $scaleWidth 寬度
=RE()
 
// =w(自己呈現寬度, 父元素寬度(無父元素可選，預設 window 寬度))
// 將寬度 px 轉換成 %
=w($selfWidth, $parentWidth: $scaleWidth)
w($paddingWidth, $outSize: $scaleWidth)
 
// =pr[Ch|Ty](一行幾筆, 自己寬度, 父元素寬度)
// 抓取 (child | type-of)，均分右邊距離，通常拿來做產品列表
=prCh($num, $selfWidth, $parentWidth: $scaleWidth)
=prTy($num, $selfWidth, $parentWidth: $scaleWidth)
 
// vw(自己呈現寬度, 父元素寬度)
// 同 =w，只是將單位轉成 vw
vw($paddingWidth, $documentWidth: $scaleWidth)
 
// vh(自己呈現高度, 窗口高度)
// 將高度單位從 px 轉換成 vh
vh($paddingHeight, $documentHeight: 974)
 
// bgH(自己呈現高度, 父元素寬度, 圖片原始寬度, 圖片原始高度)
// 將高度單位從 px 轉換成 vh
=bgH($thisWidth, $documentWidth, $normalWidth, $normalHeight)
 
// =mW(減去的寬度)
// 使用 calc 將斷減去，通常用於 aside 與 content 區塊排版
=mW($minusWidth)
mW($minusWidth)
 
// =cube(尺寸(要帶尺寸))
// 生成正方形
=cube($size)
 
// =imgW(圖片原始寬度)
// 依比例尺壓縮圖片寬度，壓縮後轉成 px 的單位
// 備註：imgWNPx 沒有尺寸
=imgW($size)
imgW($size)
imgWNPx($size)
```

---

### 子層選擇器

```sass
// child
// =ch(索引)
// [不]選擇第幾個 child
=ch($index)
=nCh($index)
 
// =fCh
// [不]選擇第一個 child
=fCh
=nFCh
 
// =lCh
// [不]選擇最後一個 child
=lCh
=nLCh
 
// =douCh
// 選擇第幾個道第地幾個 child
=douCh($index1, $index2)
 
// of-type
// =ty(索引)
// [不]選擇第幾個 type-of
=ty($index)
=nTy($index)
 
// =fTy
// [不]選擇第一個 type-of
=fTy
=nFTy
 
// =lTy
// [不]選擇最後一個 type-of
=lTy
=nLTy
 
// =douTy
// 選擇第幾個道第地幾個 type-of
=douTy($index1, $index2)
```

---

### 偽元素

```sass
// 全部皆有 inline-block
// ::[before]after 乾淨版，只有 inline-block
=b
=a
 
// =b[a]Font(文字)
// 有文字的偽元素
=bFont($text)
=aFont($text)
 
// =b[a]Bg(背景色, 寬度, 高度) 無單位
// 預填被景色的偽元素
=bBg($bgColor, $width, $height)
=aBg($bgColor, $width, $height)
 
// b[a]Img(圖片名稱, 圖片寬度, 圖片高度) 自轉 px
// b[a]ImgT(圖片名稱, 圖片寬度, 圖片高度, 圖片類型: 預設 global 類型) 自轉 px
// 背景圖片版本偽元素
=bImg($imgName, $imgWidth, $imgHeight)
=bImgT($imgName, $imgWidth, $imgHeight, $type: $IMG_TYPE)
=aImg($imgName, $imgWidth, $imgHeight, $type: $IMG_TYPE)
=aImgT($imgName, $imgWidth, $imgHeight, $type: $IMG_TYPE)
```

---

### 背景

```sass
// =bgImg(圖片名稱, background 其餘參數)
// 不帶圖片類型的背景圖引入
=bgImg($imgName, $imgOtherData...)
 
// =bgImgT(圖片名稱, 圖片類型: 自帶 global 類型, background 其餘參數)
// 帶圖片類型的背景圖引入
=bgImgT($imgName, $type: $IMG_TYPE, $imgOtherData...)
```

---

### 置中定位

```sass
// =posC(定位浮動類型: 預設絕對)
// 上下左右置中
=posC($posType: absolute)
 
// =posL[R][T][B]C(定位浮動類型: 預設絕對)
// 左右置中(由 left[right][top][bottom] 定位)
=posLC($posType: absolute)
=posRC($posType: absolute)
=posTC($posType: absolute)
=posBC($posType: absolute)
```

---

### 文字

```sass
// =fz(設計軟件上的文字大小(測試的是 ps))
// 比例尺轉換文字大小
=fz($fontSize)
 
// =lh(設計軟件上的文字行距(測試的是 ps))
// 比例轉換文字行距
=lh($lineHeight)
 
// =ls(設計軟件上的文字間距(測試的是 ps))
// 全半版轉換文字間距
=ls($letterSpacing)
```

---

### 迴圈(for loop)

```sass
// =for(到幾, 從幾)
// 純迴圈語句，底下語法自行實現
// 此 mixin 會汙染全域 $i 變數
// 故可以用 $i 調用對應索引值
=for($to, $from: 1)
 
// =forCh(到幾, 從幾)
// 迴圈 child
// 此 mixin 會汙染全域 $i 變數
// 故可以用 $i 調用對應索引值
=forCh($to, $from: 1)
 
// =forCh(到幾, 從幾)
// 迴圈 type
// 此 mixin 會汙染全域 $i 變數
// 故可以用 $i 調用對應索引值
=forTy($to, $from: 1)
```

---

### 動畫

```sass
// 預設全域動畫變量
$aniTime: 60 // 單位 s
$aniName: ani-name
 
// =kf(動畫名字, 動畫時間)
// 內置全局汙染變量，可以使用 +ani 調用最後一筆 kf
=kf($name: $aniName, $time: 60)
 
// t(執行秒速)
// 可以用時間軸的方式寫動畫，可以參考以下範例
t($second: $aniTime)
 
// =ani(執行速率, 執行模式)
// 可以直接使用 +ani 抓取最後一筆，若要抓取前筆請使用 animation
// 範例：animation: ball 5s ease infinite
=ani($duration: ease, $stopOrIni: infinite)
```

---

### 圖片盒子

```sass
// =coverCubeImg(寬高)
// 定義等比縮放的的盒子，使圖片 cover 在其盒中
=coverImg($thisWidth, $normalImageWidth, $normalImageHeight)
 
// =coverCubeImg(寬高)
// 定義方形寬高為方形的盒子(自帶 px 單位)
=coverCubeImg($cubeSize)
 
// =coverCubeImg(寬高)
// 定義方形寬高為方形的盒子(無單位)
=coverCubeImgNPx($cubeSize)
```

---

### 其他

```sass
// =arrow(方向, 顏色, 上寬, 右寬: 上寬, 下寬: 上寬, 左寬: 上寬)
// 三角形生成語法，帶三個參數就為正三小形
=arrow($direction, $color, $topWidth, $rightWidth: $topWidth, $bottomWidth: $topWidth, $leftWidth: $topWidth)
 
// =gradient(方向, 顏色參數)
// "基礎"漸層
=gradient($bgGradientDirection, $bgGradientData...)
 
// =gradientText(顏色)
// 漸層文字
=gradientText($color)
 
// =gradientTextStroke(顏色, 邊框寬度: 2, 文字填充色: #fff)
// 漸層文字框線
=gradientTextStroke($color, $strokeWidth: 2, $textFillColor: #fff)
 
// 判斷該裝置是否有 hover，基本上 pc 有效果而手機裝置沒效果
=hover
 
// 清除預設的按鈕, 下拉... 等樣式
=reAppear
```

---

### 顏色管理

```sass
// 推薦方法，可以造自己喜好，此處使用 list 管理同色系
// 可以使用 getColor 封裝顏色，接著調用 red(索引) 就能獲取對應顏色了
$red: red, #ff6d6d, #ca1e1e
@function red($index: 1)
  @return getColor($red, $index)
```