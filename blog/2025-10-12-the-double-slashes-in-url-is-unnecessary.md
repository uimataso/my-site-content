---
tags: ["zh", "fun-fact"]
---

# URL 中的 `//` 是失敗作

在今天 [ゆるコンピュータ科学ラジオ](https://www.youtube.com/@yurucom) 的節目「[URLを徹底解剖したら、衝撃の連続でした。](https://www.youtube.com/watch?v=_DEyG6ef8CY&t=228s)」中，主持人提到 [URL](https://wikipedia.org/wiki/URL) 中的 `//` 是失敗作，連發明 URL 的人都承認，它其實根本不是必要的！

![URLを分解しよう](./images/yurucom-url-explained.jpg)

……笑死。去查了一下還真的是這樣。

## The 採訪

URL 的發明者（或者應該說 World Wide Web 的發明者）[Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) 在 2009 年與 New York Times 的採訪中被問到：「有什麼事是你後悔、想要重來的嗎？」

他笑了笑，然後說，他或許會改變一件小事：

> "I would get rid of the double slash `//` after the `http:` in Web addresses."

那兩條斜線，其實根本不需要：

> "Turned out to not be really necessary."

他接著補充說：

> "Really, if you think about it, it doesn’t need the `//`. I could have designed it not to have the `//`."

其實只要有 `:` 就夠了：

> "There you go, it seemed like a good idea at the time."

然後他還半開玩笑地說：

> “Look at all the paper and trees,” he said, “that could have been saved if people had not had to write or type out those slashes on paper over the years — not to mention the human labour and time spent typing those two keystrokes countless millions of times in browser address boxes.”

## 後記

想像一下如果今天的 URL 長這樣 `https:uimataso.com/blog/the-double-slashes-in-url-is-unnecessary`。也許是我已經習慣 `//` 了，我覺得沒有 `//` 後，Scheme 跟 Domain 變得不好分辨了，因為 `:` 跟小寫的英文字母一樣高，沒有明確分段的感覺，就感覺 `https:uimataso.com` 是一個整體，不是很喜歡。

在我的印象中，好像很偶爾也會出現沒有 `//` 的 URL，像是 `mailto:me@uimataso.com`，好像之前在工作的時候也有遇過一次。

最後為了完整性，附上 URL 的標準：

- [RFC 3986 - Uniform Resource Identifier (URI): Generic Syntax](https://www.rfc-editor.org/rfc/rfc3986)
- [RFC 1738 - Uniform Resource Locators (URL)](https://www.rfc-editor.org/rfc/rfc1738)。

### 來源

我沒有找到 Berners-Lee 被採訪的影片檔案（都 404 了），以下是我找到的新聞文章：

- https://archive.nytimes.com/bits.blogs.nytimes.com/2009/10/12/the-webs-inventor-regrets-one-small-thing
- https://www.itnews.com.au/news/sorry-about-the-says-tim-berners-lee-158135
- http://news.bbc.co.uk/2/hi/technology/8306631.stm
- https://www.beet.tv/2009/10/webs-inventor-sir-tim-bernerslee-double-backslashes-were-unnecessary.html
- https://www.independent.co.uk/tech/web-creator-sorry-for-slashes-1803067.html
