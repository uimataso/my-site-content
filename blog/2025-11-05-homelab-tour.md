---
tags: ["zh", "self-hosted"]
---

# HomeLab Tour

最近這一個月白天的工作跟地獄一樣忙，而且還會再忙至少一個月。
所以趁著白天工作快要殺了我的時候，我決定再多找一些事情給自己做。好欸。

想要做的事情是介紹一下我家 HomeLab 的情況，也趁著這次的機會好好審視我使用的服務跟 workflow。

## 3rd Party Service

先從外部服務開始。

雖然不用外部服務也可以設置 HomeLab （甚至可以說不用外部服務才是 Self-hosted 的精神），但是基於我的完美主義加想太多的個性，我還是使用並訂閱了這些服務：

### [Tailscale](https://tailscale.com)

VPN 服務，我用的是免費方案。

VPN 可以讓我隨時隨地訪問在不同網路底下的設備，就是讓我在家外面也可以連到 HomeLab。

為什麼選擇 Tailscale 呢？
老實說，我只要遇到任何的網路問題，不管是在自己家也好，或是在工作也好，我就是用 Tailscale 把所有的設備都糊在一起，讓它們彼此能互通就行。
真的很好用，是我無腦的選擇。

### [Porkbun](https://porkbun.com)

Domain 供應商，我目前有一個 domain （`uimataso.com`），$11.06 / 年。

這部分其實完全是我完美主義在作祟。
我不想用醜醜的 IP + Port（像是 `100.80.0.2:3000`）來訪問我的 HomeLab。這樣不僅難看，還不能使用 SSL 加密，瀏覽器上會出現紅紅的警告圖示說網站不安全。

所以，作為一個成熟的大人，我完全清楚這件事完全只是外觀偏好，實際上就算不加密也沒什麼問題，務實的我當然是選擇買一個網域，這樣就可以用像 `service.uimataso.com` 這樣漂亮的網址來訪問我的服務了。
而且我還可以使用 [DNS Challenge](https://letsencrypt.org/docs/challenge-types/#dns-01-challenge) 申請 SSL 憑證，所以瀏覽器就不會跳出自簽加密的警告。
看起來乾淨又舒服，好誒。

好啦，除了上述的原因，我想要有自己網域的 Email 也是我買 Domain 的理由，雖然這也不是必須的東西就是了。

### [Backblaze](https://backblaze.com)

雲端儲存空間，$6 / TB / 月。

現在完全只是為了備份。
選擇 Backblaze 主要原因是價格，有興趣的可以看[我用 restic 自動備份](./2025-10-06-backup-with-restic-and-backblaze.md)。

### [Proton Mail](https://proton.me/mail) + [SimpleLogin](https://simplelogin.io)

Proton Mail 是 Email 服務，我訂閱了 Mail Plus 方案，$47.88 / 年。

SimpleLogin 是 Email alias 服務，$36 / 年。

其實 Email 跟我的 [DeGoogle](https://en.wikipedia.org/wiki/DeGoogle) 計劃比較有關係，跟 Self-hosted 反而關係沒這麼大，為了完整性而列在這邊。
之後有機會的話再跟大家分享我現在的設置，雖然我不是 100% 滿意就是了。
而且現在看，我覺得有一點貴，看看明年要不要換成其他的供應商。

## Hardware

現在用來當作 HomeLab 的電腦是 [Orange Pi 5 Plus](https://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-5-plus-32GB.html)。
它是類[樹莓派](https://www.raspberrypi.com)的 [Single-Board computer](https://en.wikipedia.org/wiki/Single-board_computer)。

當時（2、3 年前）選擇它而不是樹莓派，
是因為它可以直接使用普通尺寸 2280 的 SSD，
還有標準尺寸的 HDMI（樹莓派，請問 mini-HDMI 是尛），
效能又比當時最新的 [Raspberry Pi 4B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b) 還要好，
而且比普通電腦便宜，
看上了它的 CP 值。

而買回來後沒遇過什麼問題。
唯一有的抱怨是，因為 Orange Pi 跟樹莓派一樣是 ARM 架構，所以有時候會遇到想安裝的服務沒有提供 ARM 架構的 Docker Image。
雖然我好像只遇過 2、3 次，但在安裝想要的服務到一半時才發現沒有 Docker Image 可以用的時候還是滿困擾的。

對我而言，因為我沒有 Self-host LLM 之類的，Orange Pi 的效能其實就已經完全夠用了，所以暫時不會動到硬體的部分了，吧。

## Infra

終於進入我 Self-hosted 的部分了。

我所有的服務都是用 [Docker](https://www.docker.com) 運行，並直接用 Docker Compose 管理的。
平常要維護或更新服務時，我就是直接 SSH 進我的 HomeLab，用 Neovim 編輯 `docker-compose.yaml`，然後使用 `docker compose` 指令部署。
沒有甚麼 fancy 的東西。

我的 Infra Services 有：

- [Traefik](https://traefik.io/traefik)`: Reverse Proxy
- [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome): AdBlocker, DNS, DHCP
- [Beszel](https://github.com/henrygd/beszel): Monitoring

Traefik 是一個 [Reverse proxy](https://en.wikipedia.org/wiki/Reverse_proxy) 服務。
我選擇它的主要原因是設定方便，整合 Docker 的支援很好。
設定方便是指要新增服務的時候，只需要在該服務的 `docker-compose.yaml` 裡新增 `labels`，Traefik 就會自動偵測並建立對應的 Routing。
這樣就不需要動到 Traefik 自己的設定檔，避免 Routing 設定檔愈來愈大、難以維護。

AdGuard Home 我現在用得比較少，它的功能很多：DNS、DHCP、AdBlocker 都有。
現在我主要當它是 DNS Level 的 AdBlocker。
但實際上我主要還是比較依賴 [uBlock Origin](https://github.com/gorhill/uBlock) 做 AdBlocker，
只有在手機上沒有 AdBlocker，或是跑爬蟲服務時才會用到。

Beszel 是簡單的監測服務，主要就是看看 CPU 或記憶體的用量。
選擇這個是因為它很簡單，我在家也用不到什麼複雜的功能，畢竟就一台設備要監測而已，複雜的東西在工作上用就好了。

## Password Manager

我的密碼管理器的選擇是 [BitWarden](https://bitwarden.com)，而且使用 Self-hosted 版本 [VaultWarden](https://github.com/dani-garcia/vaultwarden) 作為後端。

選擇 BitWarden 是因為它的功能很完整，對於密碼管理器而言，我只想要一個 All-in-One Solution 不要煩惱太多事情。

而使用 Self-hosted 密碼管理器最大的擔憂就是當災害來臨時，會不會失去所有的密碼。
但因為 BitWarden 原本就會在手機的 App 本地端儲存一份資料，可以離線存取。
所以就算我連不到我的 HomeLab，我還是可以依靠手機上的 App 來取得我的密碼。
加上我現在還可以依靠我的備份來還原資料。
所以現在我選擇自己 Self-hosted。
但是，考慮到 BitWarden 的 Premium plan 也不算貴（1 美金/月），我是有考慮訂閱，然後用它當作備份，順便支持開發者，但之後再說吧.....

## Note

我所有的筆記都在筆電上，所以我想要一個可以讓我從手機快速記錄事情的服務，讓我之後可以慢慢整理進筆記裡。

好久之前，對我來說這個服務是 Discord，我會開一個只有我自己的 Server，然後有什麼東西就丟到上面。
你看，Discord 的上傳上限不是很大嗎，它又有文字搜尋功能、支援 Markdown、程式有顏色，丟鏈接的話還會自動抓標題或[預覽圖](https://ogp.me)。
簡直就是完美的筆記、書籤加 [Pastebin](https://en.wikipedia.org/wiki/Pastebin) 服務啊。

但自從開始 Self-hosted 之後我就換成了 [Memos](https://github.com/usememos/memos)。
使用的邏輯跟我用 Discord 時基本上是一模一樣，只是 Memos 是 Tags 系統，而不是 Discord 的資料夾（Channel）系統。
最近開發好像也蠻活躍的，個人是蠻推薦的。

## Email

No，我沒有想要自己搞一個 mail 服務。

主要是想要有一個服務可以自動備份我的郵件。而我要用的服務是 [OpenArchiver](https://github.com/LogicLabs-OU/OpenArchiver)。

但，我現在正在使用 [Proton Mail](https://proton.me)。
而因為 Proton Mail 有 end-to-end encryption，
如果我想要使用 SMTP 跟 IMAP 協定的話，
我必須要在我的電腦上跑 [Proton Mail Bridge](https://proton.me/mail/bridge)，
讓它處理 encryption，
我要用的第三方 Mail Client 才可以透過 bridge 使用 SMTP 跟 IMAP 接到 Proton Mail。
...多麼方便啊 ;)。

所以為了讓其他的服務也可以接到 Proton Mail，
我還要跑 [ProtonMail IMAP/SMTP Bridge Docker Container](https://github.com/shenxn/protonmail-bridge-docker)（非官方 Image）在 HomeLab 上。

而實際運行起來是沒什麼大問題，只要 OpenArchiver 可以備份我的郵件我就很開心了。

## Search Engine

[SearXNG](https://github.com/searxng/searxng) 是一個元搜尋引擎，它可以把來自各個搜尋引擎的結果彙整在一起。

是我現在主要的搜尋引擎，我覺得的最大的優點是沒有 AI Summary 跟廣告 ;)。

## E-Book

我現在是用 [Calibre-Web Automated](https://github.com/crocodilestick/Calibre-Web-Automated) 來管理電子書。

簡單介紹，[Calibre](https://github.com/kovidgoyal/calibre) 是一個電子書管理軟體，可轉檔、整理與編輯多格式書籍等，功能非常的多。

但，長話短說，因為 Calibre 是一個桌面 GUI 軟體，如果想要在 HomeLab 跑，當作服務來用的話，體驗不是很好。

所以後面出現了 [Calibre-Web](https://github.com/janeczku/calibre-web)，根據 Calibre 開發的服務。只是功能比原本的 Calibre 少了一些。

然後再後面又出現了 [Calibre-Web Automated](https://github.com/crocodilestick/Calibre-Web-Automated)。
它是 Calibre-Web 的 fork，目標是結合 Calibre-Web 的現代 Web UI 跟 Calibre 強大的功能。
也就是我現在用的服務。

只是 Calibre 只是負責管理電子書，實際的閱讀還要用額外的 App。
所以像是閱讀進度、書籤等，Calibre 好像就處理不了。
不知道有什麼好方法。

我現在閱讀是使用 [Anx-reader](https://github.com/Anxcye/anx-reader)，其實只是因為它的 UI 好看 ;)。
因為我也才剛用不久而已，就不多說了。
在更之前是使用 [LibreraReader](https://github.com/foobnix/LibreraReader) 閱讀，
但它沒辦法正確的顯示日文的[振り仮名](wikipedia.org/wiki/Furigana)（見[這個 issue](https://github.com/foobnix/LibreraReader/issues/718)），
所以現在才在找其他的閱讀 App。

## RSS

RSS 服務用的是 [Miniflux](https://github.com/miniflux/v2)。

因為我習慣在作者的網站上，或是用 [ReadYou](https://github.com/ReadYouApp/ReadYou) App 在手機上閱讀。
我希望我的 RSS 服務可以簡單一點，所以選擇了 Miniflux。

## Podcast

Podcast 服務用的是 [Audiobookshelf](https://github.com/advplyr/audiobookshelf)。
雖然 Audiobookshelf 主要的功能好像是有聲書，但我只有用它的 Podcast 功能。

我覺得 Audiobookshelf 的功能完全符合我對 Podcast 服務的想像：

- 可以在 App 內搜尋新的 Podcast
- 自動下載音檔到*本地*
- 有手機 APP（我使用的是非官方的 [Lissen](https://github.com/GrakovNe/lissen-android)，純粹是因為這個的 UI 比較好看）
- 網頁跟手機的進度同步

我覺得我已經沒什麼好要求的了。

BTW，其實我之前是沒有在聽 Podcast 的，
但最近因為在學習日語，
在工作的時候也想要有日文的輸入（為了 [Immersive learning](https://en.wikipedia.org/wiki/Immersive_learning)），所以才有在聽 Podcast。
但老實講，如果邊聽 Podcast 邊工作的話，感覺 Podcast 也沒聽到，工作效率也下降了。
但專心只聽 Podcast 的話感覺也很怪，可能 Podcast 就不適合我吧 :(。

## Music

音樂的管理真的很困難，所以我決定把這個話題留給下一篇 Blog :)。

## Photo

我現在使用看起來最 Promising 的 [Immich](https://github.com/immich-app/immich)。
主要是拿來自動備份手機的相片，平時不太會打開來用。
目前使用起來沒什麼問題，但硬要說的話，我覺得缺了簡單的編輯功能。

## Document

[Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) 的功能是管理電子文件。
你可以自己上傳電子文件，而實體文件如合約、通知單等，也可以用手機掃描成 PDF 管理。

我覺得 Paperless-ngx 最好的地方是它的 [workflow](https://docs.paperless-ngx.com/usage/#usage-recommended-workflow)。
不知為何，我覺得官方的 Recommended workflow 跟我的腦袋很合，尤其是它有 Inbox 的部分。
所以我可以先上傳文件，之後再慢慢處理、歸檔，然後相信每個在 Inbox 之外的文件都是有正確處理過的。
如果沒有 Inbox 的話，依我的個性，我一定會花很多時間在確認哪個文件是有還是沒有處理過。

另外，它還可以從信箱裡設條件自動抓文件，像是銀行寄的對賬單，或是訂閱服務的發票等。
或是設定一個專門的信箱，有想要歸檔的文件就寄到那個信箱，讓 Paperless-ngs 自動上傳等。

功能蠻完整的，我個人用的蠻開心的。

## Money

我正在用 [Actual](https://github.com/actualbudget/actual) 記賬。

在不久前我對錢相關的話題是完全沒有興趣的。
但最近轉了一個想法，覺得我只是在逃避我不熟悉的事情而已，我還是必須要好好的利用它，然後用它達到我想要完成的事情。

所以我現在開始記賬，想要先從瞭解自己的習慣開始。

但老實講，我還不知道我自己在做什麼，所以不要相信我講的有關於錢的任何事情 :)。

## Misc

其他有在用但沒這麼重要的服務：

- [Glance](https://github.com/glanceapp/glance)
- [FileBrowser](https://github.com/filebrowser/filebrowser)
- [Cyberchef](https://github.com/gchq/CyberChef)
- [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF)

## Ending

這就是我 HomeLab 的現況了。
其實還有一些平常會用到的東西我也想要 Self-hosted，像是翻譯或地圖，又或者是 Self-hosted LLM，但這些就留給之後的我吧。

快一個月沒更新了。
最近白天的工作真的花掉了我所有的精力，這篇 Blog 又花了我預想之上的時間。
感覺最近我的生活破破爛爛的，不知道自己在做什麼 :(。

Anyway，其實我是覺得周更的頻率蠻好的，之後會朝這個方向繼續努力。
下次再見！
