# Share Slides with GitHub Pages

這隻程式可以幫助你將 GitHub Repository 中的檔案，透過 GitHub Pages 來分享給其他人觀看，並且可以自動產生檔案列表，並且直接在網頁上瀏覽該檔案，讓使用者可以快速找到想要的檔案。

主要使用到的服務與技術有：

- GitHub Pages
- GitHub API
- Google Docs Viewer
- Bootstrap
- JavaScript

## 取得指定 GitHub Repository 的檔案列表及檔案路徑

GitHub 提供取得指定版本庫中所有檔案的 API，只需要提供使用者名稱 `user`、版本庫名稱 `repository`、分支名稱 `branch`，並使用 HTTP GET 即可取得檔案列表，API 的 URL 如下：

```bash
https://api.github.com/repos/[user]/[repository]/git/trees/[branch]?recursive=1
```

例如要取得 `poychang/demo-gp-share-slides` 這個版本庫 `main` 分支下的所有檔案和資料夾資訊，API 的 URL 就會是：

```bash
https://api.github.com/repos/poychang/demo-gp-share-slides/git/trees/master?recursive=1
```

回傳的結果會是 JSON 格式，其中會友 `tree` 這個陣列，裡面就是所有檔案的資訊，包含檔案名稱、路徑、大小等等，如下：

```json
{
    "sha": "7ff86a22bd5ec278ca0a5bfc8916f1e5103c6a17",
    "url": "https://api.github.com/repos/poychang/demo-gp-share-slides/git/trees/7ff86a22bd5ec278ca0a5bfc8916f1e5103c6a17",
    "tree": [
        {
            "path": "slides",
            "mode": "040000",
            "type": "tree",
            "sha": "a191896fcfda0581b95e3a63e5b5d7a06ae98bfc",
            "url": "https://api.github.com/repos/poychang/demo-gp-share-slides/git/trees/a191896fcfda0581b95e3a63e5b5d7a06ae98bfc"
        },
        {
            "path": "slides/demo-pdf.pdf",
            "mode": "100644",
            "type": "blob",
            "sha": "07cb664367e91b6e00dee93d834eefba264ec6a9",
            "size": 21309,
            "url": "https://api.github.com/repos/poychang/demo-gp-share-slides/git/blobs/07cb664367e91b6e00dee93d834eefba264ec6a9"
        },
        // 略...
    ],
    "truncated": false
}
```

這裡的重點是 `path` 相對路徑，篩選出有檔案名稱的 `path` 之後，搭配下列 URL 就可以取得該版本庫中的指定檔案：

```bash
https://raw.githubusercontent.com/[user]/[repository]/[branch]/[path]
或是
https://github.com/[user]/[repository]/raw/[branch]/[path]
```

## 線上顯示檔案內容

有了可以用來下載檔案的 URL 之後，就可以使用 Google Doc Viewer 這套線上工具來顯示檔案內容，只要將檔案的 URL 傳給 Google Doc Viewer，就可以產生一個線上的網址，讓使用者可以直接在瀏覽器上觀看檔案內容，例如：

```bash
https://docs.google.com/viewer?url=[fileURL]&embedded=true
```

關於 Google Doc Viewer 的基本使用方式，可以參考這篇[快速在網頁上預覽 Office 檔案](https://blog.poychang.net/office-word-excel-powerpoint-online-viewer/)文章。

特別要注意的是，Google Docs Viewer 僅支援檔案大小為 25 MB 以下，並且僅支援下列檔案類型：

- Image files (.JPEG, .PNG, .GIF, .TIFF, .BMP)
- Video files (WebM, .MPEG4, .3GPP, .MOV, .AVI, .MPEGPS, .WMV, .FLV)
- Text files (.TXT)
- Markup/Code (.CSS, .HTML, .PHP, .C, .CPP, .H, .HPP, .JS)
- Microsoft Word (.DOC and .DOCX)
- Microsoft Excel (.XLS and .XLSX)
- Microsoft PowerPoint (.PPT and .PPTX)
- Adobe Portable Document Format (.PDF)
- Apple Pages (.PAGES)
- Adobe Illustrator (.AI)
- Adobe Photoshop (.PSD)
- Tagged Image File Format (.TIFF)
- Autodesk AutoCad (.DXF)
- Scalable Vector Graphics (.SVG)
- PostScript (.EPS, .PS)
- TrueType (.TTF)
- XML Paper Specification (.XPS)
- Archive file types (.ZIP and .RAR)
