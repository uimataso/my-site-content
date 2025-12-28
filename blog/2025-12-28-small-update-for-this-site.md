---
tags: ["zh", "blog"]
---

# 小小更新一下這個網站

半年前實做的這個 static site generator 到現在都沒更新過一次，那時決定之後在加的功能也沒有加上。最近終於想起來要更新了。就算 blog 寫很慢，網站本身是不能輸的。

分享一下更新的東西：

## Blog URL

Blog page 的 URL 從原本的 `/blog/blog-name` 變成 `/blog/yyyy-mm-dd-blog-name`。

其實原本我的 markdown 的檔名就是 `/blog/yyyy-mm-dd-blog-name.md`，只是上一版的 generator 把日期拿掉了，想說這樣 URL 會比較乾淨一點。但經過一些的考慮後，我決定讓 URL 跟 markdown 檔名保持一致，所以做了這個更新。

唯一不好的地方是，因為 URL 改了，所以 RSS 閱讀器會把舊的文章當成新發佈的文章，讓有訂閱我 RSS 的人的 inbox 被汙染。抱歉！

## Theme

如果是直接在這個網站上閱讀的人，應該馬上就會發現 theme 變了。現在換成我平時用的 Gruvbox Dark。其實上一版的 theme 也是我之前自己使用的顏色，但是我在大概一年前就換成使用 Gruvbox 了。

那為什麼之前不用 Gruvbox 呢？

Gruvbox 是比較兩極的 theme，喜歡的很喜歡，不喜歡的很討厭。所以之前選擇用比較一般的顏色。但我現在覺得讓我的網站長得跟我日常的 terminal 一模一樣好像蠻好玩的，所以趁這次機會換了一下。

BTW，Gruvbox 的白色（黃？）讓我連想到夕陽跟黃銅，有一次認真看了這個 theme 後就離不開了。

## Code Syntax Highlight

現在我的網站有 Syntax Highlighting 了！可喜可賀！

```rust
fn main() {
    // a comment
    let name = "uima";
    println!("Hello, {}!", name);
}
```

```sh
# make sure to subscript my rss feed
printf "hello, %s\n" "uima"
```

現在這個 highlight 的顏色跟我自己平常用的可以說是一模一樣。

literal 橘色、keyword 粗體、符號跟註釋是灰色，其他都是白色。

我覺得顏色太多反而讓我沒辦法專心，所以之前自己用了一個很簡單的 theme。

## Favicon

現在這個網站的 icon 是用 [`.svg`](../favicon.svg) 而不是 `.ico`。不止檔案變小了（13kb -> 1.25kb），像素的邊緣也變銳利了。

我的 icon 是 16x16 的像素圖，所以理論上我可以存成 16x16 的超小圖片。但實際上如果我存成 16x16 的 `png` 或 `ico`，在顯示成網站 icon 時會被自動放大，導致像素的邊緣被處理的很模糊。所以我之前直接把 icon 存成 256x256 的圖片，這樣像素的邊緣就會變得銳利一些，但是檔案大小就變的很可笑的大（13kb）。

所以這次我決定使用向量格式的 `svg`。我去隨便找的像素畫網站，把 icon 畫上去，匯出成 `svg`。

啊怎麽還是 13kb。

邊緣是比較銳利啦，但就幾個像素，是有必要這麼大嗎。

於是我打開那個 `svg`，我才知道 `svg` 其實是用 `html` 的語法，才發現那個網站的 `svg` 是一個像素一個像素畫正方形用出來的。

既然 `svg` 是 `html`，我自己畫應該可以用的比較小吧？

所以我就直接編輯 `svg` 檔案。結果我也很滿意。現在整個 icon 只要 1.25 kb，而且因為是向量圖，所以邊緣一定是鋒利的，想要換顏色的話，直接編輯裡面的色碼就可以了。

有興趣的話，用瀏覽器開 [/favicon.svg](/favicon.svg)，然後用開發者工具看，或是直接下載 [/favicon.svg](/favicon.svg) 然後用文字編輯器打開即可。

## Monospace & Code Block

這次也花了一點時間在調整 `css`，主要是讓每個元件都可以對齊 monospace 字形的字元。也修復了一些小細節。

但是，還有一個小問題，我用的中文字形跟英文字形沒有對齊。在 terminal 裡，中文字的寬度會是剛好 2 個英文字元，現在這個網站用的不是。之後在處理吧...

然後為了讓網站更像我平時用的 terminal，code block 的背景顏色一定要是背景的黑色，剛好覺得 [マリウス](https://xn--gckvb8fzb.com) 的網站很帥，就抄了一些設計過來 :D。

## Workflow

最後的更新跟大家比較沒有關係。

原本的 generator 是設計成一個 service，它會每一陣子就抓一次 git 有沒有更新，自動生成新網站。

但考慮到我更新的頻率，手動生成、部署會比較好除錯，所以把自動化的部分拿掉了。
