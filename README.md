<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>每日一句 - 文字盒子</title>
   
</head>
<body>
    <header>
        <h1>每日一句</h1>
        <p>文字向你发出一封简讯</p>
    </header>

    <div class="container">
        <!-- 主页卡片 -->
        <div class="card-container" id="mainPage">
            <div class="category-card" data-category="movies">
                <h2>🎬 电影台词</h2>
                <p>精选经典电影对白</p>
            </div>

            <div class="category-card" data-category="literature">
                <h2>📚 文学作品</h2>
                <p>名著经典语录</p>
            </div>

            <div class="category-card" data-category="songs">
                <h2>🎵 歌词精选</h2>
                <p>动人歌词收藏</p>
            </div>

            <div class="category-card" data-category="proverbs">
                <h2>💡 谚语格言</h2>
                <p>智慧结晶</p>
            </div>
        </div>

        <!-- 电影详情页 -->
        <div class="detail-page" id="movies">
            <button class="back-button">返回</button>
            <h2>🎬 电影台词</h2>
            <div class="quote-container"></div>
            <button class="refresh-button">换一句</button>
        </div>

        <!-- 文学详情页 -->
        <div class="detail-page" id="literature">
            <button class="back-button">返回</button>
            <h2>📚 文学作品</h2>
            <div class="quote-container"></div>
            <button class="refresh-button">换一句</button>
        </div>

         <!-- 歌词详情页 -->
         <div class="detail-page" id="songs">
            <button class="back-button">返回</button>
            <h2>🎵 歌词精选</h2>
            <div class="quote-container"></div>
            <button class="refresh-button">换一句</button>
        </div>

         <!-- 谚语详情页 -->
         <div class="detail-page" id="proverbs">
            <button class="back-button">返回</button>
            <h2>💡 谚语格言</h2>
            <div class="quote-container"></div>
            <button class="refresh-button">换一句</button>
        </div># newdailyquote
