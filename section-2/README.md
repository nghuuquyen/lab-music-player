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
  width: 40px;
  height: 40px;
  border: 10px solid var(--color-text);
  border-top-color: transparent;
  border-radius: 50%;
  rotate: 45deg;
  opacity: 0.7;
}

.logo-text {
  font-weight: 600;
  font-size: 25px;
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

## 3. Right Bar – Thanh thông tin bên phải
### 3.1 Mục tiêu:
Xây dựng thanh sidebar bên phải chứa các thông tin bổ sung và tương tác nhanh, bao gồm tìm kiếm, nghệ sĩ phổ biến, podcast nổi bật, và hàng đợi phát nhạc.

### 3.2 Phân tích Design:
Right Bar được chia thành 4 phần chính từ trên xuống dưới:

1. **Search Bar:** Thanh tìm kiếm với icon ở bên phải
2. **Popular Artists:** Lưới các nghệ sĩ phổ biến dạng 3x2
3. **Top Podcasts:** Danh sách podcast ngang có thể scroll
4. **Next in Queue:** Hàng đợi bài hát tiếp theo

### 3.3 Cấu trúc HTML:
```html
<aside class="rightbar">
    <!-- Search Bar -->
    <div class="search-bar">
        <input type="text" placeholder="Search for something">
        <i class="fa-solid fa-magnifying-glass search-icon"></i>
    </div>

    <!-- Popular Artists -->
    <section class="popular-artists">
        <h3>Popular Artists</h3>
        <div class="artists-grid">
            <div class="artist-card">
                <img src="https://i.pravatar.cc/80?img=1" alt="Sarah Melody">
                <span class="artist-name">Sarah Melody</span>
            </div>
            <div class="artist-card">
                <img src="https://i.pravatar.cc/80?img=2" alt="Javier Cruz">
                <span class="artist-name">Javier Cruz</span>
            </div>
            <!-- Thêm các nghệ sĩ khác... -->
        </div>
    </section>

    <!-- Top Podcasts -->
    <section class="top-podcasts">
        <h3>Top Podcasts</h3>
        <div class="podcasts-list">
            <div class="podcast-card">
                <img src="https://placehold.co/100x100?text=P1" alt="Podcast 1">
                <h4>Mindful Musings</h4>
            </div>
            <div class="podcast-card">
                <img src="https://placehold.co/100x100?text=P2" alt="Podcast 2">
                <h4>The Creative</h4>
            </div>
            <!-- Thêm các podcast khác... -->
        </div>
    </section>

    <!-- Next in Queue -->
    <section class="next-queue">
        <h3>Next in Queue</h3>
        <div class="queue-list">
            <div class="queue-item">
                <img src="https://placehold.co/50x50?text=Q1" alt="Queue 1">
                <div class="song-info">
                    <h4>Eternal Echoes</h4>
                    <p>Sarah Melody</p>
                </div>
                <span class="time">04:32</span>
                <i class="fa-solid fa-heart queue-heart active"></i>
            </div>
            <!-- Thêm các bài hát khác... -->
        </div>
    </section>
</aside>
```

### 3.4 CSS chi tiết:
```css
/*
* --------------------------
* Right Bar
* --------------------------
*/
.rightbar {
  background-color: var(--color-bg);
  border-radius: 20px;
  padding: 20px;
  border: 1px solid var(--color-border);
  color: var(--color-text);
  transition: background-color 0.3s, color 0.3s;
  display: flex;
  flex-direction: column;
  gap: 30px;
  height: fit-content;
}

.rightbar h3 {
  margin: 0 0 15px 0;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--color-text);
}

/* Search Bar */
.search-bar {
  position: relative;
  margin-bottom: 10px;
}

.search-bar input {
  width: 100%;
  padding: 10px 40px 10px 15px;
  border: 1px solid var(--color-border);
  border-radius: 25px;
  background-color: var(--color-card);
  color: var(--color-text);
  font-size: 0.9rem;
  outline: none;
  transition: border-color 0.3s, background-color 0.3s;
}

.search-bar input::placeholder {
  color: var(--color-muted);
}

.search-bar input:focus {
  border-color: var(--color-primary);
}

.search-icon {
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--color-muted);
  font-size: 0.9rem;
}

/* Popular Artists */
.popular-artists .artists-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.artist-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.artist-card:hover {
  transform: translateY(-2px);
}

.artist-card img {
  width: 60px;
  height: 60px;
  border-radius: 10px;
  object-fit: cover;
  margin-bottom: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.artist-name {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--color-text);
  line-height: 1.2;
}

/* Top Podcasts */
.podcasts-list {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  padding-bottom: 5px;
}

.podcasts-list::-webkit-scrollbar {
  height: 4px;
}

.podcasts-list::-webkit-scrollbar-track {
  background: var(--color-border);
  border-radius: 2px;
}

.podcasts-list::-webkit-scrollbar-thumb {
  background: var(--color-muted);
  border-radius: 2px;
}

.podcast-card {
  flex: 0 0 auto;
  width: 100px;
  text-align: center;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.podcast-card:hover {
  transform: translateY(-2px);
}

.podcast-card img {
  width: 100px;
  height: 100px;
  border-radius: 12px;
  object-fit: cover;
  margin-bottom: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.podcast-card h4 {
  margin: 0;
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--color-text);
  line-height: 1.2;
}

/* Next in Queue */
.queue-list {
  margin-top: 1rem;
}

.queue-item {
  display: flex;
  align-items: center;
  gap: 15px;
  background: var(--color-card);
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.queue-item:hover {
  background-color: var(--color-border);
}

.queue-item img {
  width: 50px;
  height: 50px;
  border-radius: 6px;
  object-fit: cover;
}

.queue-item .song-info {
  flex: 1;
}

.queue-item .song-info h4 {
  margin: 0;
  font-size: 0.9rem;
}

.queue-item .song-info p {
  margin: 0;
  color: var(--color-muted);
  font-size: 0.8rem;
}

.queue-item .time {
  color: var(--color-muted);
  font-size: 0.8rem;
  margin-right: 10px;
}

.queue-heart {
  font-size: 0.9rem;
  cursor: pointer;
  transition: color 0.2s ease;
  color: var(--color-muted);
}

.queue-heart.active {
  color: var(--color-primary);
}

.queue-heart:hover {
  color: var(--color-primary);
}
```

## 4. Player Bar – Thanh điều khiển phát nhạc
### 4.1 Mục tiêu:
Xây dựng thanh player bar cố định ở cuối màn hình - đây chính là "trái tim" của ứng dụng nghe nhạc. Player bar sẽ chứa toàn bộ thông tin và điều khiển cho bài hát đang phát.

### 4.2 Phân tích Design:
Dựa theo thiết kế, player bar được chia thành 3 phần chính theo chiều ngang:

1. **Current Song Info (Bên trái):** Hiển thị ảnh cover, tên bài hát, nghệ sĩ và nút yêu thích
2. **Player Controls (Giữa):** Các nút điều khiển phát nhạc và thanh tiến độ
3. **Right Controls (Bên phải):** Danh sách phát, âm lượng và fullscreen

**Đặc điểm thiết kế nổi bật:**
- Background gradient xanh dương (màu primary của app)
- Text màu trắng để tương phản với nền
- Các button có hiệu ứng hover
- Progress bar interactive với handle xuất hiện khi hover
- Layout responsive, ưu tiên phần controls ở giữa

### 4.3 Cấu trúc HTML:
```html
<footer class="player-bar">
    <!-- Current Song Info -->
    <div class="current-song">
        <img src="https://placehold.co/60x60?text=CS" alt="Current Song" class="song-cover">
        <div class="song-details">
            <h4 class="song-title">Rhythmic Echoes</h4>
            <p class="song-artist">Alex Harmon</p>
        </div>
        <i class="fa-regular fa-heart song-heart"></i>
    </div>

    <!-- Player Controls -->
    <div class="player-controls">
        <div class="control-buttons">
            <button class="control-btn">
                <i class="fa-solid fa-backward-step"></i>
            </button>
            <button class="control-btn">
                <i class="fa-solid fa-backward"></i>
            </button>
            <button class="play-pause-btn">
                <i class="fa-solid fa-pause"></i>
            </button>
            <button class="control-btn">
                <i class="fa-solid fa-forward"></i>
            </button>
            <button class="control-btn">
                <i class="fa-solid fa-forward-step"></i>
            </button>
        </div>
        
        <div class="progress-section">
            <span class="current-time">03:19</span>
            <div class="progress-bar">
                <div class="progress-track">
                    <div class="progress-fill" style="width: 65%;"></div>
                    <div class="progress-handle"></div>
                </div>
            </div>
            <span class="total-time">05:08</span>
        </div>
    </div>

    <!-- Right Controls -->
    <div class="right-controls">
        <button class="control-btn">
            <i class="fa-solid fa-list"></i>
        </button>
        <button class="control-btn">
            <i class="fa-solid fa-volume-high"></i>
        </button>
        <div class="volume-bar">
            <div class="volume-track">
                <div class="volume-fill" style="width: 80%;"></div>
            </div>
        </div>
        <button class="control-btn">
            <i class="fa-solid fa-up-right-and-down-left-from-center"></i>
        </button>
    </div>
</footer>
```

### 4.4 CSS chi tiết:
```css
/*
* --------------------------
* Player Bar
* --------------------------
*/
.player-bar {
  background: linear-gradient(135deg, var(--color-primary) 0%, #4a90e2 100%);
  padding: 15px 25px;
  border-radius: 20px 20px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 25px;
  color: #fff;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.1);
}

/* Current Song Section */
.current-song {
  display: flex;
  align-items: center;
  gap: 15px;
  min-width: 250px;
}

.song-cover {
  width: 50px;
  height: 50px;
  border-radius: 8px;
  object-fit: cover;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
}

.song-details {
  flex: 1;
}

.song-title {
  margin: 0 0 4px 0;
  font-size: 0.95rem;
  font-weight: 600;
  color: #fff;
}

.song-artist {
  margin: 0;
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.8);
}

.song-heart {
  font-size: 1.1rem;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.8);
  transition: color 0.2s ease;
}

.song-heart:hover,
.song-heart.active {
  color: #fff;
}

/* Player Controls */
.player-controls {
  flex: 1;
  max-width: 500px;
}

.control-buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin-bottom: 12px;
}

.control-btn {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.8);
  font-size: 1rem;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.control-btn:hover {
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
}

.play-pause-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: #fff;
  font-size: 1.2rem;
  cursor: pointer;
  padding: 12px;
  border-radius: 50%;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 45px;
  height: 45px;
}

.play-pause-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.05);
}

/* Progress Section */
.progress-section {
  display: flex;
  align-items: center;
  gap: 12px;
}

.current-time,
.total-time {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.8);
  min-width: 35px;
  text-align: center;
}

.progress-bar {
  flex: 1;
  cursor: pointer;
}

.progress-track {
  position: relative;
  height: 6px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #fff;
  border-radius: 3px;
  position: relative;
  transition: width 0.1s ease;
}

.progress-handle {
  position: absolute;
  right: -6px;
  top: 50%;
  transform: translateY(-50%);
  width: 12px;
  height: 12px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  opacity: 0;
  transition: opacity 0.2s ease;
}

.progress-bar:hover .progress-handle {
  opacity: 1;
}

/* Right Controls */
.right-controls {
  display: flex;
  align-items: center;
  gap: 15px;
  min-width: 200px;
  justify-content: flex-end;
}

.volume-bar {
  width: 80px;
}

.volume-track {
  position: relative;
  height: 4px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 2px;
  cursor: pointer;
}

.volume-fill {
  height: 100%;
  background: #fff;
  border-radius: 2px;
  transition: width 0.1s ease;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .player-bar {
    padding: 12px 20px;
    gap: 15px;
  }
  
  .current-song {
    min-width: auto;
  }
  
  .right-controls {
    min-width: auto;
  }
  
  .volume-bar {
    display: none;
  }
}
```