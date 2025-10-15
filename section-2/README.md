# Section 2 – Xây dựng từng Component khớp với Design

Ở phần trước, các em đã cùng thầy dựng xong “bộ khung” cho toàn bộ ứng dụng nghe nhạc. Bây giờ chúng ta sẽ bắt đầu đi sâu vào từng phần cụ thể của giao diện, hay nói cách khác là xây từng component một cách có hệ thống.

Đây chính là giai đoạn “biến khung thành hình”, nơi các em sẽ áp dụng kỹ năng HTML/CSS chi tiết, đồng thời luyện khả năng đọc – hiểu – chuyển đổi bản thiết kế sang mã nguồn thực tế.

## 1. Sidebar – Thanh điều hướng bên trái
### 1.1 Mục tiêu:
Xây dựng thanh sidebar với logo, nhóm nút điều hướng (Home, Browse, Radio, Library…), và vùng playlist ở phía dưới.

### 1.2 Phân tích Design:

- Sidebar có chiều rộng cố định (khoảng 240px – 280px).
- Nền màu xám nhạt ở chế độ Light Mode (#f8f8f8) và tối hơn ở Dark Mode.
- Các icon nằm bên trái chữ, có màu nhấn khi được chọn.
- Font sử dụng cỡ nhỏ hơn phần nội dung chính.

Cấu trúc HTML mẫu:
```html
<aside class="sidebar">
    <div class="sidebar-top">
        <!-- Logo -->
        <div class="logo">
            <div class="circle-icon"></div>
            <span class="logo-text">Circle</span>
        </div>

        <!-- Menu -->
        <nav class="menu">
            <a href="#" class="menu-item active">
                <i class="fa-solid fa-house icon"></i>
                <span>Home</span>
            </a>
            <a href="#" class="menu-item">
                <i class="fa-solid fa-magnifying-glass icon"></i>
                <span>Search</span>
            </a>
            <a href="#" class="menu-item">
                <i class="fa-solid fa-list icon"></i>
                <span>Playlists</span>
            </a>
            <a href="#" class="menu-item">
                <i class="fa-regular fa-bell icon"></i>
                <span>Notifications</span>
                <span class="dot"></span>
            </a>
            <a href="#" class="menu-item">
                <i class="fa-solid fa-gem icon"></i>
                <span>Premium Content</span>
            </a>
        </nav>
    </div>

    <!-- Bottom -->
    <div class="sidebar-bottom">
        <a href="#" class="download-link">
            <i class="fa-solid fa-download"></i> Download Desktop App
        </a>
        <div class="user-card">
            <img src="https://i.pravatar.cc/40" alt="User Avatar" class="avatar" />
            <span class="username">Jonathon Trott</span>
            <i class="fa-solid fa-chevron-down arrow"></i>
        </div>
    </div>
</aside>
```

CSS gợi ý:
```css
/*
* --------------------------
* Sidebar
* --------------------------
*/
.sidebar {
  background-color: var(--color-bg);
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px;
  width: 240px;
  border: 1px solid var(--color-border);
  color: var(--color-text);
  transition: background-color 0.3s, color 0.3s;
}

/* Logo */
.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 30px;
}

.circle-icon {
  width: 20px;
  height: 20px;
  border: 4px solid var(--color-text);
  border-top-color: transparent;
  border-radius: 50%;
  rotate: 45deg;
  opacity: 0.7;
}

.logo-text {
  font-weight: 600;
}

/* Menu */
.menu {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.menu-item {
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  color: var(--color-text);
  font-size: 14px;
  padding: 8px 10px;
  border-radius: 10px;
  position: relative;
  transition: background-color 0.25s, color 0.25s;
}

.menu-item:hover {
  background-color: var(--color-border);
  color: var(--color-primary);
}

.menu-item.active {
  background-color: var(--color-card);
  color: var(--color-primary);
  font-weight: 500;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

/* Notification dot */
.dot {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  width: 6px;
  height: 6px;
  background-color: var(--color-primary);
  border-radius: 50%;
}

/* Bottom section */
.sidebar-bottom {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.download-link {
  text-align: center;
  font-size: 13px;
  color: var(--color-primary);
  text-decoration: none;
  margin-bottom: 10px;
}

.user-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: var(--color-card);
  border-radius: 10px;
  padding: 6px 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s;
}

.avatar {
  width: 28px;
  height: 28px;
  border-radius: 50%;
}

.username {
  flex: 1;
  font-size: 13px;
  margin-left: 6px;
}

.arrow {
  font-size: 14px;
  opacity: 0.7;
}
```

## 2. Main Content – Phần nội dung chính
### 2.1 Mục tiêu:
Phần main (nằm bên phải sidebar) sẽ gồm các thành phần sau:

1) **Thanh danh mục (Category Tabs):** Cho phép chọn giữa các loại nội dung: Music, Podcasts, Jazz, Electronic, Rock Classic, Hip Hop.
    - Đây là các nút (button hoặc thẻ `<a>`) có hiệu ứng hover và trạng thái “active”.
    - Có thể sử dụng display: flex hoặc gap để canh đều các nút.

1) K**hối “Popular Albums” (Các album phổ biến):** Hiển thị danh sách các album nổi bật dạng card nằm ngang.
   - Mỗi card có ảnh bìa, tiêu đề, nghệ sĩ, và nút “Play”.
   - Có thể triển khai bằng display: flex hoặc grid.

1) **Khối “Picked for you” (Gợi ý dành riêng cho bạn):** Là một playlist card lớn hơn, có màu nền khác biệt, chiếm toàn bộ chiều ngang.
   - Bên trái là ảnh nghệ sĩ hoặc hình nền.
   - Bên phải là nội dung: tiêu đề playlist, mô tả, số lượt thích, và các tag.
   - Có thể tạo bố cục bằng display: flex.

1) **Khối “Recent” (Các bài hát vừa nghe):** Dạng danh sách (list) các bài hát với thứ tự, tiêu đề, nghệ sĩ, năm, thời lượng, và icon “like”.
   - Có thể tạo bằng `<table>`  hoặc `<div>` lặp lại theo hàng.
   - Cần chú ý canh giữa theo hàng ngang.

Cấu trúc HTML gợi ý cho toàn bộ phần nội dung:
```html
<main class="main-content">
   <!-- 1. Category Tabs -->
   <div class="categories">
       <button class="active">Music</button>
       <button>Podcasts</button>
       <button>Jazz</button>
       <button>Electronic</button>
       <button>Rock Classic</button>
       <button>Hip Hop</button>
   </div>

   <!-- 2. Popular Albums -->
   <section class="popular-albums">
       <h2>Popular Albums</h2>
       <div class="album-list">
           <div class="album-card">
               <img src="https://placehold.co/200x150?text=C" alt="Album 1">
               <h3>Ephemeral Dreams</h3>
               <p>Sarah Melody</p>
               <div class="meta">
                   <span>12 Tracks</span> • <span>31K Likes</span>
               </div>
               <div class="album-actions">
                   <i class="fa-regular fa-heart"></i>
                   <button><i class="fa-solid fa-play"></i></button>
               </div>
           </div>

           <div class="album-card">
               <img src="https://placehold.co/200x150?text=C" alt="Album 2">
               <h3>Ephemeral Dreams</h3>
               <p>Sarah Melody</p>
               <div class="meta">
                   <span>12 Tracks</span> • <span>31K Likes</span>
               </div>
               <div class="album-actions">
                   <i class="fa-regular fa-heart"></i>
                   <button><i class="fa-solid fa-play"></i></button>
               </div>
           </div>

           <div class="album-card">
               <img src="https://placehold.co/200x150?text=C" alt="Album 3">
               <h3>Ephemeral Dreams</h3>
               <p>Sarah Melody</p>
               <div class="meta">
                   <span>12 Tracks</span> • <span>31K Likes</span>
               </div>
               <div class="album-actions">
                   <i class="fa-regular fa-heart"></i>
                   <button><i class="fa-solid fa-play"></i></button>
               </div>
           </div>
       </div>
   </section>

   <!-- 3. Picked for you -->
   <section class="picked">
       <h2>Picked for you</h2>
       <div class="picked-list">
           <div class="picked-card">
               <img src="https://placehold.co/200x150?text=C" alt="Playlist">
               <div class="picked-info">
                   <span class="label">PLAYLIST</span>
                   <h3>New Music Friday Pakistan</h3>
                   <p>Listen to the fresh new music from Pakistan and around the world.</p>

                   <div class="meta">
                       <span>03:21:23</span>
                       <span>1.3M Likes</span>
                       <span>32 Tracks</span>
                   </div>

                   <div class="tags">
                       <span>Podcasts</span>
                       <span>Audio Books</span>
                       <span>Classic Fusion</span>
                   </div>

                   <div class="actions">
                       <div class="action-left">
                           <button class="icon-btn"><i class="fa-regular fa-heart"></i></button>
                           <button class="icon-btn"><i class="fa-solid fa-share-nodes"></i></button>
                       </div>
                       <button class="play-btn"><i class="fa-solid fa-play"></i></button>
                   </div>
               </div>
           </div>
       </div>
   </section>

   <!-- 4. Recent -->
   <section class="recent">
       <h2>Recent</h2>

       <div class="recent-list">
           <div class="recent-item">
               <span class="index">1</span>
               <img src="https://placehold.co/100x100?text=C" alt="Eternal Echoes">
               <div class="song-info">
                   <h4>Eternal Echoes</h4>
                   <p>Sarah Melody</p>
               </div>
               <div>
                   <h4>Serenity Symphony</h4>
                   <span class="year">2022</span>
               </div>
               <span class="time">04:32</span>
               <i class="fa-regular fa-heart"></i>
           </div>

           <div class="recent-item">
               <span class="index">2</span>
               <img src="https://placehold.co/100x100?text=C" alt="Rhythmic Echoes">
               <div class="song-info">
                   <h4>Rhythmic Echoes</h4>
                   <p>Alex Harmon</p>
               </div>
               <div>
                   <h4>Serenity Symphony</h4>
                   <span class="year">2022</span>
               </div>
               <span class="time">03:12</span>
               <i class="fa-regular fa-heart"></i>
           </div>
       </div>
   </section>
</main>
```

### 2.2 CSS gợi ý cho phần nội dung chính:
```css
/*
* --------------------------
* Main Content
* --------------------------
*/

/* Category Tabs */
.categories {
  display: flex;
  gap: 12px;
  margin-bottom: 2rem;
}

.categories button {
  background: #e5e5e5;
  border: none;
  border-radius: 25px;
  padding: 8px 18px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
}

.categories button.active,
.categories button:hover {
  background: var(--color-primary);
  color: #fff;
}

/* Popular Albums */
.popular-albums .album-list {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.album-list {
  margin-top: 1rem;
}

.album-card {
  background: var(--color-card);
  border-radius: 12px;
  padding: 15px;
  width: 300px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s;
  position: relative;
}

.album-card:hover {
  transform: translateY(-5px);
}

.album-card img {
  width: 100%;
  border-radius: 10px;
  margin-bottom: 10px;
}

.album-card h3 {
  margin: 5px 0;
  font-size: 1rem;
}

.album-card p {
  color: var(--color-muted);
  font-size: 0.9rem;
}

.album-card .meta {
  color: var(--color-muted);
  font-size: 0.8rem;
  margin-top: 20px;
}

.album-actions {
  position: absolute;
  bottom: 10px;
  right: 10px;
  display: flex;
  gap: 8px;
  align-items: center;
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.album-actions button {
  background: var(--color-primary);
  border: none;
  color: #fff;
  border-radius: 50%;
  width: 32px;
  height: 32px;
  cursor: pointer;
}

/* Picked for You */
.picked {
  margin-top: 1rem;
}

.picked h2 {
  margin-bottom: 1rem;
  color: var(--color-text);
}

.picked-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
  gap: 20px;
}

.picked-card {
  display: flex;
  background: var(--color-primary);
  color: #fff;
  border-radius: 16px;
  overflow: hidden;
  transition: transform 0.2s ease;
}

.picked-card:hover {
  transform: translateY(-3px);
}

.picked-card img {
  width: 200px;
  object-fit: cover;
}

.picked-info {
  padding: 20px;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.picked-info .label {
  font-size: 0.8rem;
  letter-spacing: 1px;
  opacity: 0.8;
}

.picked-info h3 {
  margin-top: 8px;
  font-size: 1.2rem;
}

.picked-info p {
  font-size: 0.9rem;
  margin: 10px 0;
}

.picked-card .meta {
  font-size: 0.8rem;
  opacity: 0.8;
  display: flex;
  gap: 15px;
  letter-spacing: 1px;
}

.picked-info .tags {
  margin-top: 10px;
}

.picked-info .tags span {
  display: inline-block;
  background: rgba(255, 255, 255, 0.2);
  padding: 4px 10px;
  border-radius: 12px;
  margin-right: 6px;
  font-size: 0.8rem;
}

.picked-info .actions {
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.actions .action-left {
  display: flex;
  gap: 10px;
}

.icon-btn {
  background: none;
  border: none;
  color: inherit;
  font-size: 1.1rem;
  cursor: pointer;
  transition: opacity 0.2s ease;
}

.icon-btn:hover {
  opacity: 0.8;
}

.play-btn {
  background: #fff;
  color: var(--color-primary);
  border: none;
  width: 42px;
  height: 42px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  transition: transform 0.2s ease;
}

.play-btn:hover {
  transform: scale(1.1);
}

/* Recent */
.recent {
  margin-top: 1rem;
}

.recent-list {
  margin-top: 1rem;
}

.recent-item {
  display: flex;
  align-items: center;
  gap: 15px;
  background: var(--color-card);
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 10px;
}

.recent-item img {
  width: 50px;
  height: 50px;
  border-radius: 6px;
  object-fit: cover;
}

.recent-item .song-info {
  width: 300px;
}

.recent-item .song-info h4 {
  margin: 0;
  font-size: 1rem;
}

.recent-item .song-info p {
  margin: 0;
  color: var(--color-muted);
  font-size: 0.85rem;
}

.recent-item .index {
  font-weight: bold;
  color: var(--color-muted);
  width: 20px;
}

.recent-item .year,
.recent-item .time {
  margin-left: auto;
  color: var(--color-muted);
  font-size: 0.85rem;
}
```