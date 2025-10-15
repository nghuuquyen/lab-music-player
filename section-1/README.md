# Section 1 – Phân tích giao diện và tổ chức source code

Chào các em,
Trong bài học hôm nay, chúng ta sẽ cùng thực hành xây dựng giao diện ứng dụng nghe nhạc giống như các hình mẫu mà thầy đã trình chiếu trên lớp — gồm Dark Mode và Light Mode.
Mục tiêu là giúp các em hiểu quy trình phát triển giao diện web một cách có hệ thống, thay vì chỉ viết mã theo cảm tính.

## BƯỚC 1 — PHÂN TÍCH GIAO DIỆN (UI Analysis)

Trước khi bắt tay vào code, việc đầu tiên chúng ta phải làm là phân tích giao diện.

### 1.1. Mục tiêu của bài thực hành

Sau bài này, các em cần nắm được:

- Cách phân chia layout của một trang web phức tạp.
- Biết cách xây dựng component tái sử dụng được.
- Hiểu tổ chức mã nguồn HTML/CSS sao cho rõ ràng, dễ mở rộng.
- Áp dụng Font Awesome để thêm icon chuyên nghiệp.
- Tạo Dark Mode / Light Mode bằng biến CSS mà không cần JavaScript.

### 1.2. Cấu trúc bố cục giao diện

Giao diện mẫu có tên "Circle Music Player", gồm 2 phiên bản: Light Mode và Dark Mode.

Giao diện Light Mode:
![](./Music%20Player%20-%20Light%20Mode.png)

Giao diện Dark Mode:
![](./Music%20Player%20-%20Dark%20Mode.png)

Nhìn vào giao diện mẫu, ta chia trang thành 4 vùng rõ ràng:

| Vùng                   | Nội dung chính                 | Gợi ý kỹ thuật           |
| ---------------------- | ------------------------------ | ------------------------ |
| **Sidebar trái**       | Logo, Menu, User               | Flexbox theo cột         |
| **Nội dung trung tâm** | Albums, Playlist, Recent       | Grid layout              |
| **Sidebar phải**       | Search, Artist, Podcast, Queue | Grid/Flex                |
| **Footer (Player)**    | Thanh phát nhạc cố định        | Flexbox + position fixed |

## BƯỚC 2 — TỔ CHỨC SOURCE CODE

Thầy muốn các em làm việc có cấu trúc, giống như một lập trình viên chuyên nghiệp.
Hãy tạo thư mục dự án như sau:

```bash
music-player/
│
├── index.html
├── css/
│   ├── base.css          # Reset, fonts, biến màu
│   ├── layout.css        # Chia layout tổng thể
│   ├── components.css    # Các phần tử nhỏ (card, button, nav...)
│   └── theme.css         # Quản lý Dark/Light mode
├── assets/
│   ├── images/
│   └── icons/
└── README.md
```
Các em chưa cần viết mã vội. Hãy tạo khung thư mục trước, để hình thành tư duy “dự án phải có tổ chức”.

## BƯỚC 3 — CẤU HÌNH CƠ SỞ CSS (BASE LAYER)

Trước tiên, ta tạo file `css/base.css` để khai báo font, reset và biến màu.

```css
/* base.css */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --font-main: "Inter", sans-serif;
  --color-bg: #f5f5f5;
  --color-text: #111;
  --color-card: #fff;
  --color-primary: #3b82f6;
  --color-border: #e5e7eb;
}

/* Dark theme */
[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-text: #e2e8f0;
  --color-card: #1e293b;
  --color-border: #334155;
}
```

Khi chúng ta đổi thuộc tính `data-theme="dark"` trên `<body>`, toàn bộ màu sắc của trang sẽ thay đổi tự động. Đây là nền tảng cho Dark/Light mode.

## BƯỚC 4 — DỰNG KHUNG LAYOUT CHÍNH

Trong file `index.html`, các em tạo khung layout gồm 4 phần:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <link rel="stylesheet" href="css/base.css">
    <link rel="stylesheet" href="css/layout.css">
    <link rel="stylesheet" href="css/components.css">
</head>

<body data-theme="light">
    <div class="app">
        <aside class="sidebar"> ... </aside>
        <main class="main-content"> ... </main>
        <aside class="rightbar"> ... </aside>
        <footer class="player-bar"> ... </footer>
    </div>
</body>
</html>
```

Bây giờ chúng ta định nghĩa bố cục bằng CSS Grid trong `layout.css`:

```css
/* layout.css */
.app {
  display: grid;
  grid-template-columns: 250px 1fr 320px;
  grid-template-rows: 1fr auto;
  height: 100vh;
  background: var(--color-bg);
  color: var(--color-text);
  font-family: var(--font-main);
  overflow: hidden;
}

.sidebar {
  grid-row: 1 / span 2;
  padding: 1.5rem;
  background: var(--color-card);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  max-height: 100%;
  overflow-y: auto;
}

.rightbar {
  max-height: 100%;
}

.main-content,
.rightbar {
  padding: 2rem;
  overflow-y: auto;
}

.player-bar {
  grid-column: 2 / span 2;
  background: var(--color-card);
  border-top: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 2rem;
}

/* Hide scrollbars but allow scrolling */
.sidebar,
.rightbar,
.main-content {
  overflow-y: auto;
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE 10+ */
}

.sidebar::-webkit-scrollbar,
.rightbar::-webkit-scrollbar,
.main-content::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera */
}
```

Đoạn code layout trên được hiểu như sau:
- `.app` sử dụng CSS Grid để chia bố cục thành 3 cột và 2 hàng. Cột trái rộng 250px, cột giữa chiếm phần còn lại, cột phải rộng 320px. Hàng dưới cùng dành cho thanh Player.
- `.sidebar` chiếm toàn bộ chiều cao 2 hàng bên trái, dùng Flexbox để sắp xếp nội dung theo cột.
- `.main-content` và `.rightbar` có padding và cho phép cuộn nội dung.
- `.player-bar` nằm ở dưới cùng, trải dài 2 cột bên phải, dùng Flexbox để căn chỉnh các phần tử bên trong.
- Các phần tử trong `.sidebar`, `.main-content`, và `.rightbar` đều có khả năng cuộn nội dung khi vượt quá chiều cao.
- Các thanh cuộn sẽ bị ẩn đi để tạo sự liền mạch cho giao diện.
- `.player-bar` nằm ở dưới cùng, trải dài 2 cột bên phải, dùng Flexbox để căn chỉnh các phần tử bên trong.

Sau bước này, các em sẽ thấy bố cục chia thành 3 cột rõ ràng, có khung player ở dưới.

## BƯỚC 5 — XÂY DỰNG COMPONENT

### 5.1. Sidebar Navigation

Thêm đoạn sau vào phần `<aside class="sidebar">`:

```html
<aside class="sidebar">
  <div class="logo">Circle</div>

  <nav class="menu">
    <a href="#" class="active"><i class="fa-solid fa-house"></i> Home</a>
    <a href="#"><i class="fa-solid fa-magnifying-glass"></i> Search</a>
    <a href="#"><i class="fa-solid fa-list"></i> Playlists</a>
    <a href="#"><i class="fa-solid fa-bell"></i> Notifications</a>
    <a href="#"><i class="fa-solid fa-star"></i> Premium</a>
  </nav>

  <div class="user">
    <img src="assets/images/user.jpg" alt="User">
    <span>Jonathan Trott</span>
  </div>
</aside>
```

CSS trong `components.css`:

```css
/* Sidebar */
.sidebar .menu a {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  color: var(--color-text);
  text-decoration: none;
  transition: 0.3s;
}

.sidebar .menu a:hover,
.sidebar .menu a.active {
  background: var(--color-primary);
  color: white;
}
```

### 5.2. Album Card

Trong `<main class="main-content">`:

```html
<div class="album-card">
  <img src="assets/images/album1.jpg" alt="Album Cover">
  <div class="info">
    <h4>Ephemeral Dreams</h4>
    <p>Sarah Melody</p>
    <div class="meta">
      <span>12 Tracks</span> • <span>31K Likes</span>
    </div>
  </div>
  <button class="play-btn"><i class="fa-solid fa-play"></i></button>
</div>
```

CSS trong `components.css`:

```css
/* Album Card */
.album-card {
  background: var(--color-card);
  border-radius: 12px;
  padding: 1rem;
  position: relative;
  transition: 0.3s;
}

.album-card:hover {
  transform: translateY(-4px);
}

.play-btn {
  position: absolute;
  bottom: 1rem;
  right: 1rem;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
}
```

## BƯỚC 6 — CHUYỂN DARK / LIGHT MODE

Để đổi giao diện, các em chỉ cần đổi thuộc tính:

```html
<body data-theme="dark">...</body>
```

## NHẬN XÉT

Ở phần trên, các em đã cùng thầy đi qua toàn bộ quy trình hình dung và tổ chức một giao diện phức tạp, từ khâu phân tích thiết kế cho đến xây dựng cấu trúc mã nguồn. Cụ thể, chúng ta đã:

- Xác định mục tiêu và phạm vi giao diện – hiểu rõ giao diện này gồm những phần nào, mục đích học tập là gì.
- Phân tích bố cục tổng thể và chia thành các khối chức năng rõ ràng: sidebar, nội dung chính, thanh phát nhạc, v.v.
- Xây dựng hệ thống component cơ bản, giúp mỗi phần giao diện có thể tái sử dụng.
- Tổ chức cấu trúc file HTML và CSS hợp lý, chia tách thành base, layout, components, và theme.
- Đặt nền tảng cho Dark Mode / Light Mode thông qua biến CSS – bước khởi đầu cho việc làm việc chuyên nghiệp với theme.

Toàn bộ những bước đó chưa giúp giao diện giống hoàn toàn bản thiết kế, nhưng lại rất quan trọng vì nó giúp các em hình thành tư duy làm việc có hệ thống. Một lập trình viên giao diện giỏi không bắt đầu bằng việc “viết CSS cho đẹp”, mà bắt đầu bằng việc phân tích, chia nhỏ và định hình cấu trúc rõ ràng để mọi chi tiết sau này đều có chỗ hợp lý để đặt vào.

Ở phần tiếp theo, thầy và các em sẽ cùng nhau đi vào chi tiết từng phần giao diện, bắt đầu với Sidebar, sau đó đến các thẻ Album, Artist, Podcast, và cuối cùng là thanh Player. Mỗi phần sẽ có ví dụ mã HTML/CSS cụ thể, cùng cách áp dụng Font Awesome cho icon và cách tinh chỉnh theme để giao diện ngày càng sát với bản thiết kế gốc.

> Nói ngắn gọn: Các em đã học “cách dựng khung”, ở phần sau chúng ta sẽ bắt đầu “xây từng mảng tường” để giao diện thật sự hoàn chỉnh.