<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>每日一句 - 名言集锦</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f5f5;
            line-height: 1.6;
        }

        header {
            background-color: #2c3e50;
            color: white;
            text-align: center;
            padding: 2rem;
            margin-bottom: 2rem;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* 主页卡片样式 */
        .card-container {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            grid-auto-rows: minmax(200px, auto);
            gap: 1.5rem;
            padding: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .category-card {
            background: white;
            border-radius: 15px;
            padding: 2rem;
            cursor: pointer;
            transition: transform 0.3s, box-shadow 0.3s;
            display: flex;
            flex-direction: column;
            justify-content: center;
            min-height: 250px; /* 固定最小高度 */
            border-left: 5px solid;
        }

        /* 响应式调整 */
        @media (max-width: 1024px) {
            .card-container {
                grid-template-columns: 1fr;
                max-width: 800px;
            }
            
            .category-card {
                min-height: 180px;
                padding: 1.5rem;
            }
        }

        @media (max-width: 480px) {
            .card-container {
                padding: 10px;
                gap: 1rem;
            }
            
            .category-card {
                min-height: 150px;
                padding: 1rem;
            }
            
            .category-card h2 {
                font-size: 1.1rem;
            }
            
            .category-card p {
                font-size: 0.9rem;
            }
        }

        /* 新增卡片尺寸控制 */
        .category-card:nth-child(odd) {
            margin-right: 0.75rem;
        }
        
        .category-card:nth-child(even) {
            margin-left: 0.75rem;
        }
        /* 详情页样式 */
        .detail-page {
            display: none;
            background: white;
            border-radius: 15px;
            padding: 2rem;
            margin-top: 2rem;
            position: relative;
        }

        .back-button {
            position: absolute;
            top: 20px;
            right: 30px;
            padding: 10px 20px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s;
        }

        .back-button:hover {
            background: #2980b9;
        }

        .refresh-button {
            display: block;
            margin: 20px auto;
            padding: 10px 30px;
            background: #39d178;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .refresh-button:hover {
            transform: scale(1.05);
            background: #27ae60;
        }

        /* 调整名言容器样式 */
        .quote-container {
            min-height: 200px;
            padding: 20px;
            position: relative;
        }
        .quote {
            margin: 1.5rem 0;
            padding: 1rem;
            border-left: 4px solid #3498db;
            background-color: #f8f9fa;
        }

        /* 动画效果 */
        .show-detail {
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @media (max-width: 768px) {
            .card-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>每日一句</h1>
        <p>点击分类查看精选内容</p>
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
        </div>

        
    </div>
    <script>
        // 名言数据结构
        const quotesData = {
            movies: [
                {
                    text: '"希望是好事，也许是人间至善，而美好的事永不消逝。"',
                    source: "《肖申克的救赎》 - 安迪·杜佛兰"
                },
                {
                    text: '"生活就像一盒巧克力，你永远不知道你会得到什么。"',
                    source: "《阿甘正传》 - 阿甘"
                },
                {
                    text: '"你要尽全力保护你的梦想。"',
                    source: "《当幸福来敲门》"
                }
            ],
            literature: [
                {
                    text: '"生存还是毁灭，这是个问题。"',
                    source: "《哈姆雷特》 - 威廉·莎士比亚"
                },
                {
                    text: '"所有幸福的家庭都是相似的，每个不幸的家庭各有各的不幸。"',
                    source: "《安娜·卡列尼娜》 - 列夫·托尔斯泰"
                }
            ],
            songs: [
                {
                    text: '"多少人曾爱慕你年轻时的容颜，可知谁愿承受岁月无情的变迁"',
                    source: "《一生有你》 - 水木年华"
                },
                {
                    text: '"爱一朵花不猜它能开多久，放宽了心情，把什么都变美了。"',
                    source: "《在树上唱歌》 - 郭静"
                }
            ],
            proverbs: [
                {
                    text: '"千里之行，始于足下。"',
                    source: "《道德经》"
                }
            ]
        };

        // 当前状态记录
        let currentCategory = null;
        let currentQuoteIndex = -1;

        // 卡片点击事件
        document.querySelectorAll('.category-card').forEach(card => {
            card.addEventListener('click', function() {
                currentCategory = this.dataset.category;
                showCategoryPage(currentCategory);
            });
        });

        // 显示分类页
        function showCategoryPage(category) {
            // 隐藏主页
            document.getElementById('mainPage').style.display = 'none';
            
            // 显示对应详情页
            const detailPage = document.getElementById(category);
            detailPage.style.display = 'block';
            
            // 生成随机名言
            updateQuoteDisplay(category);
        }

        // 更新名言显示
        function updateQuoteDisplay(category) {
            const quotes = quotesData[category];
            const container = document.querySelector(`#${category} .quote-container`);
            
            // 获取新的随机索引（确保不重复）
            let newIndex;
            do {
                newIndex = Math.floor(Math.random() * quotes.length);
            } while (quotes.length > 1 && newIndex === currentQuoteIndex)
            
            currentQuoteIndex = newIndex;
            
            // 生成HTML
            container.innerHTML = `
                <div class="quote">
                    <p>${quotes[currentQuoteIndex].text}</p>
                    <div class="source">${quotes[currentQuoteIndex].source}</div>
                </div>
            `;
        }

        // 绑定换句按钮事件（事件委托）
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('refresh-button')) {
                updateQuoteDisplay(currentCategory);
            }
        });

    // 卡片点击事件
    document.querySelectorAll('.category-card').forEach(card => {
        card.addEventListener('click', function() {
            const category = this.dataset.category;
            
            // 隐藏主页
            document.getElementById('mainPage').style.display = 'none';
            
            // 显示对应详情页
            const detailPage = document.getElementById(category);
            detailPage.style.display = 'block';
            detailPage.classList.add('show-detail');
        });
    });

    // 返回按钮事件
    document.querySelectorAll('.back-button').forEach(btn => {
        btn.addEventListener('click', function() {
            // 隐藏所有详情页
            document.querySelectorAll('.detail-page').forEach(page => {
                page.style.display = 'none';
                page.classList.remove('show-detail');
            });
            
            // 显示主页
            document.getElementById('mainPage').style.display = 'grid';
        });
    });

    // 动态颜色生成
    const cards = document.querySelectorAll('.category-card');
    const colors = ['#3498db', '#2ecc71', '#e67e22', '#9b59b6'];
    
    cards.forEach((card, index) => {
        card.style.borderLeft = `5px solid ${colors[index]}`;
        card.addEventListener('mouseover', () => {
            card.style.transform = 'scale(1.02)';
        });
        card.addEventListener('mouseout', () => {
            card.style.transform = 'scale(1)';
        });
    });
</script>
</body>
</html>
