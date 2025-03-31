<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>每日一句 - 文字盒子</title>
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
            background: hsl(145, 42%, 30%);
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .refresh-button:hover {
            transform: scale(1.05);
            background: #245538;
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
                    text: '"曾经有一份真诚的爱情摆在我的面前，我没有珍惜，等到失去的时候才追悔莫及，人世间最痛苦的事情莫过于此。"',
                    source: "《大话西游》"
                },
                {
                    text: '"你要尽难做的事和应该做的事，往往是同一件事。凡是有意义的事都不会容易，成年人的生活里没有容易二字。全力保护你的梦想。"',
                    source: "《天气预报员》"
                },
                {
                    text: '"不管前方的路有多苦，只要走的方向正确，不管多么崎岖不平，都比站在原地更接近幸福。"',
                    source: "宫崎骏 《千与千寻》"
                },
                {
                    text: '"人们喜欢讨论邪恶的东西，你需要自己判断，相信自己的眼睛和耳朵。"',
                    source: "《海蒂和爷爷》"
                },
                {
                    text: '"我们一路奋战，不是为了改变世界，而是为了不让世界改变我们。"',
                    source: "《熔炉》"
                },
                {
                    text: '"没有人愿意沉湎于回忆，只不过美好的都属于过去。"',
                    source: "《小王子》"
                },
                {
                    text: '"幸福，不是长生不老，不是大鱼大肉，不是权倾朝野。幸福是每一个微小的生活愿望达成。当你想吃的时候有得吃，想被爱的时候有人来爱你。"',
                    source: "《飞屋环游记》"
                },
                {
                    text: '"“那个世界好重 压在我身上。你甚至不知道它在哪里结束 你难道从来不为自己生活在无穷选择里而害怕得快崩溃掉吗？”"',
                    source: "《海上钢琴师》"
                },
                {
                    text: '"时光总有一天会将你我拆散，可是即便如此，在那个时刻之前，也让我们在一起吧。"',
                    source: "《萤火之森》"
                },
                


            ],
            literature: [
                {
                    text: '"生存还是毁灭，这是个问题。"',
                    source: "《哈姆雷特》 - 威廉·莎士比亚"
                },
                {
                    text: '"所有幸福的家庭都是相似的，每个不幸的家庭各有各的不幸。"',
                    source: "《安娜·卡列尼娜》 - 列夫·托尔斯泰"
                },
                {
                    text: '"吹灭读书灯，一身都是月。"',
                    source: "桂苓"
                },
                {
                    text: '"世界上任何书籍都不能带给你好运，但它们能让你悄悄成为你自己。"',
                    source: "赫尔曼・黑塞"
                },
                {
                    text: '"彼时的少年站在成长的尽头，回首过去，一路崎岖早已繁花盛开。"',
                    source: "八月长安 《你好，旧时光》"
                },
                {
                    text: '"少年读书如隙中窥月，中年读书如庭中望月，老年读书如台上玩月，皆以阅历之浅深为所得之浅深耳。"',
                    source: "张潮 《幽梦影》"
                },
                {
                    text: '"所有随风而逝的都属于昨天，所有历经风雨留下来的才是面向未来的。"',
                    source: "玛格丽特・米切尔 《飘》"
                },
                {
                    text: '"可其实人从来没有真正拥有过什么未来，人也没有所谓的余生，余生只是愿景，只是想象，人实实在在拥有的，是已经度过的岁月，还有当下这一瞬间。"',
                    source: "公子优 《你的距离》"
                },
                {
                    text: '"一旦抓住了某些蛛丝马迹，一切就变得有迹可循了。"',
                    source: "《暗格里的秘密》"
                },
                {
                    text: '"无法改变其实是不想改变。改变意味着要抛弃、否定“过去的自己”，比起凤凰涅磐的痛苦，维持现状更容易。他们用过去的不幸来解释现在的顿足不前。"',
                    source: "《被讨厌的勇气》"
                },
                {
                    text: '"文学是一字一字的救出自己，书法是一笔一笔的救出自己"',
                    source: "木心 《云雀叫了一整天》"
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
                },
                {
                    text: '"我一脚踏空我就要飞起来了，我向上是迷茫我向下听见你说，这世界是空荡荡，你说一二三打碎了过往消亡，有风吹破了的归途"',
                    source: "《撒野》"
                },
                {
                    text: '"当世界忘了我，时间抛弃我，那个时候我，是不是有了崭新的生活"',
                    source: "《忘了我》"
                },
                {
                    text: '"努力的往前飞，再痛也无所谓，黑夜过后的光芒有多美。"',
                    source: "张杰 《我们都一样》"
                },
                {
                    text: '"七月的风，八月的雨，卑微的我喜欢遥远的你，你还未来我怎敢老去，未来的日子我奉陪到底"',
                    source: "《遥远的你》"
                },
                {
                    text: '"晚风吹起你鬓间的白发，抚平回忆留下伤疤，你的眼中，明暗交杂，一笑生花。"',
                    source: "《起风了》"
                },
                {
                    text: '"也许很累一身狼狈  也许卑微一生无为  谁生来不都是一样  尽管叫我无名之辈"',
                    source: "《无名之辈》 "
                },
                {
                    text: '"回忆是时光里，带着温暖的余悸。最好忘了吧，最坏不过是关上这世界的门。伸出了双手，拥抱当时的我们。。"',
                    source: "杨炅翰 《最好的我们》"
                },
                {
                    text: '"开始总是分分钟都妙不可言，谁都以为热情它永不会减。"',
                    source: "莫文蔚"
                },
                {
                    text: '"我为你翻山越岭 却无心看风景"',
                    source: "《爱就一个字》"
                },
                {
                    text: '"时间从来不回答，生命从来不喧哗"',
                    source: "吴青峰"
                },
                {
                    text: '"让我们红尘作伴活得潇潇洒洒，策马奔腾共享人世繁华，对酒当歌唱出心中喜悦，轰轰烈烈把握青春年华。"',
                    source: "动力火车 《当》"
                },
                {
                    text: '"生命是华丽错觉，时间是贼，偷走一切。"',
                    source: "阿信 《如烟》"
                },
                {
                    text: '"那就祝我们友谊长存，她们说朋友比情人永恒，都怪我太懦弱 不敢开口对你说，我对你的爱 风吹雨落 依然在守候，那就祝我们能做好朋友"',
                    source: "《友谊长存》"
                }
            ],
            proverbs: [
                {
                    text: '"千里之行，始于足下。"',
                    source: "《道德经》"
                },
                {
                    text: '"世界上最浪漫的事便是遥远的相似性"',
                    source: "沃兹基"
                },
                {
                    text: '"世事漫随流水，算来一梦浮生"',
                    source: "李煜"
                },
                {
                    text: '"千里之行，始于足下。我是行人，更送行人去。愁无据。寒蝉鸣处，回首斜阳暮。"',
                    source: "赵彦端"
                },
                {
                    text: '"我渴望有人爆裂的爱我至死不渝，明白爱和死一样强大，并永远的站在我身边，橘子，不是唯一的水果你的人生还有别的可能，每个人讲故事的方式都不一样，只是为了提醒我们，每个人眼里的故事都是不一样的。万物倒塌又被重建，而重建者充满欢愉。"',
                    source: "《橘子不是唯一的水果》"
                },
                {
                    text: '"井蛙不可以语于海者，拘于虚也；夏虫不可以语于冰者，笃于时也；曲士不可以语于道者，束于教也"',
                    source: "无名氏"
                },
                {
                    text: '"相鼠有皮，人而无仪！人而无仪，不死何为？相鼠有齿，人而无止！人而无止，不死何俟？相鼠有体，人而无礼！人而无礼，胡不遄死？"',
                    source: "《诗经》"
                },
                {
                    text: '"古驿道的柳树来年又青，她每个星期日思念着一步一步走来向你述说着后来的生活。"',
                    source: "《我们仨》"
                },
                {
                    text: '"惟有王城最堪隐，万人如海一身藏。"',
                    source: "苏轼"
                },
                {
                    text: '"人生，总会有不期而遇的温暖，和生生不息的希望。"',
                    source: "安东尼"
                },
                {
                    text: '"人世几回伤往事，山形依旧枕寒流"',
                    source: "无名氏"
                },
                {
                    text: '"有不虞之誉，有求全之毁。"',
                    source: "孟子"
                },
                {
                    text: '"凄凉别后两应同，最是不胜清怨月明中。"',
                    source: "纳兰性德"
                },
                {
                    text: '"缺月挂疏桐，漏断人初静。谁见幽人独往来，缥缈孤鸿影。"',
                    source: "苏轼"
                },
                {
                    text: '"悟已往之不谏，知来者之可追。实迷途其未远，觉今是而昨非。"',
                    source: "陶渊明"
                },
                {
                    text: '"即使是最好的朋友也应该保持一定距离，正如里尔克所说，友谊最高的境界是守护彼此的孤独。"',
                    source: "笔记"
                },
                {
                    text: '"不是所有的错误都可以被原谅，也不是所有的伤痛都可以被抚平，总有时间也无能无力的事：比如爱，比如思念"',
                    source: "《海边的曼彻斯特》"
                },
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
