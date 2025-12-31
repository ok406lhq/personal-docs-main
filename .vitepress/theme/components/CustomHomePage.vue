<template>
    <div id="home-page">
        <!-- 背景 GIF -->
        <div id="background-gif"></div>

        <!-- 粒子效果 -->
        <div id="particles-js"></div>

        <!-- 主内容 -->
        <div class="content">
            <h1 class="title">代码江湖</h1>
            <p class="subtitle">行走江湖，指尖生风；编程世界，快意人生。</p>

            <div class="actions">
                <a class="btn" href="/docs/book">踏入江湖</a>
                <a class="btn btn-secondary" href="/docs/about">了解更多</a>
            </div>
        </div>

        <div class="projects-section">
            
            <div class="project-showcase">
                <!-- 左箭头 -->
                <button class="nav-button nav-button-left" @click="scroll('left')" :disabled="isAtStart">
                    <svg viewBox="0 0 24 24" class="arrow-icon">
                        <path d="M15 19l-7-7 7-7" />
                    </svg>
                </button>

                <!-- 右箭头 -->
                <button class="nav-button nav-button-right" @click="scroll('right')" :disabled="isAtEnd">
                    <svg viewBox="0 0 24 24" class="arrow-icon">
                        <path d="M9 5l7 7-7 7" />
                    </svg>
                </button>

                <!-- 卡片容器 -->
                <div class="cards-container" ref="scrollContainer" @scroll="checkScrollPosition">
                    <div class="cards-wrapper">
                        <!-- 项目卡片 -->
                        <template v-for="project in projects" :key="project.id">
                            <!-- 有链接的卡片 -->
                            <a v-if="project.link"
                               :href="project.link"
                               target="_blank"
                               rel="noopener noreferrer"
                               class="project-card">
                                <div class="project-info">
                                    <div class="project-header">
                                        <h3 class="project-title">{{ project.title }}</h3>
                                        <span class="project-status" :class="project.status">
                                            {{ project.status === 'completed' ? '已完成' : '进行中' }}
                                        </span>
                                    </div>
                                    <p class="project-description">{{ project.description }}</p>
                                    <div class="project-tags">
                                        <span v-for="tag in project.tags" :key="tag" class="project-tag">
                                            {{ tag }}
                                        </span>
                                    </div>
                                </div>
                            </a>
                            
                            <!-- 没有链接的卡片 -->
                            <div v-else class="project-card no-link">
                                <div class="project-info">
                                    <div class="project-header">
                                        <h3 class="project-title">{{ project.title }}</h3>
                                        <span class="project-status" :class="project.status">
                                            {{ project.status === 'completed' ? '已完成' : '进行中' }}
                                        </span>
                                    </div>
                                    <p class="project-description">{{ project.description }}</p>
                                    <div class="project-tags">
                                        <span v-for="tag in project.tags" :key="tag" class="project-tag">
                                            {{ tag }}
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { onMounted, ref } from 'vue';

export default {
    name: 'CustomHomePage',
    setup() {
        const scrollContainer = ref(null);
        const isAtStart = ref(true);
        const isAtEnd = ref(false);
        const loading = ref(true);

        const projects = [
            {
                id: 1,
                title: 'RTCoin',
                status: 'completed',
                description: '💻一个React Native开发的跨平台界面框架，可以作为学习和构建跨平台移动应用的基础记录，支持iOS和Android双平台。',
                tags: ['React Native', 'App', '跨平台'],
                link: 'https://github.com/ok406lhq/RTCoin'
            },
            {
                id: 2,
                title: 'BabySongs',
                status: 'completed',
                description: '👶一个专为婴幼儿设计的音乐应用，提供丰富的儿歌和童谣资源，帮助宝宝在愉快的音乐氛围中成长。',
                tags: ['Android', '音乐播放器', 'App'],
                link: 'https://github.com/ok406lhq/BabySongs'
            },
            {
                id: 3,
                title: 'genshin-web-game',
                status: 'completed',
                description: '🤣原神但网页版！一个用AI开发的基于Web技术开发的原神游戏网页版，提供类似原神的游戏体验，方便玩家在浏览器中畅玩。',
                tags: ['原神', 'H5', 'OpenAI'],
                link: 'https://github.com/ok406lhq/genshin-web-game'
            },
            {
                id: 4,
                title: 'personal-docs-main',
                status: 'completed',
                description: '🔗一个收录了笔者平时遇见的工具、资源、站点导航站，方便笔者自己使用，也分享给有需要的朋友。',
                tags: ['工具', '资源', '导航'],
                link: 'https://github.com/ok406lhq/personal-docs-main'
            },
            {
                id: 5,
                title: '阅读路线',
                status: 'completed',
                description: '📚一个电子书阅读器App，基于MD设计模式的Android应用，支持多种格式的电子书阅读，提供良好的阅读体验。',
                tags: ['App', '阅读', '电子书'],
                link: 'https://gitee.com/github-22291214/ReadingBook'
            },
            {
                id: 6,
                title: '游戏资讯小程序',
                status: 'in-progress',
                description: '🕹️一款只记录关注游戏的相关资讯，包括工作室声明和开发进度，以及各种攻略等内容的轻量级小程序。',
                tags: ['小程序', '游戏', '资讯'],
                // link: '#'
            },

            // 添加更多项目...
        ];

        const checkScrollPosition = () => {
            if (!scrollContainer.value) return;
            const container = scrollContainer.value;
            const scrollLeft = container.scrollLeft;
            const maxScrollLeft = Math.max(0, container.scrollWidth - container.clientWidth);

            // 容差 1 像素，避免像素取整导致的误判
            isAtStart.value = scrollLeft <= 1;
            isAtEnd.value = scrollLeft >= Math.max(0, maxScrollLeft - 1);
        };

        const scroll = (direction) => {
            if (!scrollContainer.value) return;

            const container = scrollContainer.value;
            const cardWidth = 320; // 卡片宽度
            const gap = 24; // 卡片间距
            const scrollAmount = cardWidth + gap; // 每次滚动的距离

            if (direction === 'left') {
                container.scrollBy({ left: -scrollAmount, behavior: 'smooth' });
            } else {
                container.scrollBy({ left: scrollAmount, behavior: 'smooth' });
            }

            // 稍后检查位置（scroll 事件也会触发 checkScrollPosition）
            setTimeout(checkScrollPosition, 300);
        };

        const getStatusClass = (status) => {
            return {
                'completed': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200',
                'in-progress': 'bg-amber-100 text-amber-800 dark:bg-amber-900 dark:text-amber-200'
            }[status] || 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-200';
        };

        onMounted(() => {
            // 动态加载 particles.js 脚本
            const script = document.createElement('script');
            script.src = 'https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js';
            script.onload = () => {
                particlesJS('particles-js', {
                    particles: {
                        number: { value: 20, density: { enable: true, value_area: 800 } },
                        color: { value: ['#000000', '#4a4a4a', '#7f7f7f'] },
                        shape: { type: 'circle' },
                        opacity: { value: 0.6, random: true },
                        size: { value: 10, random: true },
                        move: { enable: true, speed: 1 },
                    },
                    interactivity: {
                        detect_on: 'canvas',
                        events: {
                            onhover: { enable: true, mode: 'grab' },
                            onclick: { enable: true, mode: 'push' },
                        },
                    },
                    retina_detect: true,
                });
            };
            document.body.appendChild(script);

            checkScrollPosition();
            window.addEventListener('resize', checkScrollPosition);

            // 模拟加载
            setTimeout(() => {
                loading.value = false;
            }, 1000);
        });

        return {
            scrollContainer,
            isAtStart,
            isAtEnd,
            projects,
            checkScrollPosition,
            scroll,
            getStatusClass,
            loading
        };
    },
};
</script>

<style scoped>
/* 页面容器 */
#home-page {
    min-height: 100vh; /* 改为最小高度，允许内容自然延伸 */
    width: 100%;  /* 改为 100% 而不是 100vw 避免水平滚动条 */
    position: relative;
    font-family: 'Liu Jian Mao Cao', cursive;
    display: flex;
    flex-direction: column;
    overflow-x: hidden; /* 只隐藏水平溢出 */
    overflow-y: visible; /* 允许垂直方向内容显示 */
}

/* 背景 GIF 样式 */
#background-gif {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url('https://binarycoder777-site.oss-cn-chengdu.aliyuncs.com/demo.gif') no-repeat center center;
    background-size: cover;
    z-index: -2;
}

/* 粒子效果容器 */
#particles-js {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
}

/* 主内容样式 */
.content {
    position: relative;
    text-align: center;
    color: #333;
    z-index: 1;
    padding-top: 15vh; /* 使用 vh 单位替代百分比 */
    padding-bottom: 10vh; /* 使用 vh 单位替代百分比 */
}

.title {
    font-size: 4rem;
    margin-bottom: 2rem;
    color: #000;
}

.subtitle {
    font-size: 1.5rem;
    margin-bottom: 2rem;
}

.actions {
    display: flex;
    justify-content: center;
    gap: 1rem;
}

.btn {
    padding: 10px 20px;
    font-size: 1rem;
    color: #fff;
    background-color: #007bff;
    text-decoration: none;
    border-radius: 5px;
    transition: background-color 0.3s ease;
}

.btn:hover {
    background-color: #0056b3;
}

.btn-secondary {
    background-color: #6c757d;
}

.btn-secondary:hover {
    background-color: #545b62;
}

/* 评论组件样式 */
.comments {
    position: relative;
    z-index: 1;
    /* 确保评论在背景层之上 */
    padding: 20px;
    background: rgba(255, 255, 255, 0.8);
    /* 给评论区加一个半透明背景，保证可读性 */
    border-top: 1px solid #ddd;
    /* 分隔线 */
}

.projects-section {
    max-width: 1200px;
    width: 90%; /* 添加宽度控制 */
    margin: 0 auto 5vh auto; /* 添加底部间距 */
    padding: 40px 20px;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    position: relative; /* 确保位置正确 */
    z-index: 1; /* 确保在背景之上 */
}

.projects-title {
    font-size: 24px;
    font-weight: bold;
    text-align: center;
    margin-bottom: 32px;
    color: var(--vp-c-text-1);
}

.project-showcase {
    position: relative;
    padding: 20px 0;
    margin-top: 20px; /* 增加顶部间距 */
}

/* 添加项目展示区域的标题样式 */
.projects-section::before {
    content: "个人部署";
    display: block;
    text-align: center;
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 32px;
    color: #333;
}

/* 暗色模式适配 */
:root.dark .projects-section {
    background: rgba(0, 0, 0, 0.2);
}

:root.dark .projects-section::before {
    color: #fff;
}

/* 导航按钮样式 */
.nav-button {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.9);
    border: none;
    cursor: pointer;
    z-index: 2;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

.nav-button:hover {
    background: rgba(255, 255, 255, 1);
    transform: translateY(-50%) scale(1.1);
}

.nav-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

.nav-button-left {
    left: -20px;
}

.nav-button-right {
    right: -20px;
}

.arrow-icon {
    width: 24px;
    height: 24px;
    fill: none;
    stroke: currentColor;
    stroke-width: 2;
    stroke-linecap: round;
    stroke-linejoin: round;
}

/* 卡片容器样式 */
.cards-container {
    overflow: hidden;
    margin: 0 20px;
    padding-bottom: 20px; /* 添加底部内边距，防止阴影被裁剪 */
}

.cards-wrapper {
    display: flex;
    gap: 24px;
    transition: transform 0.5s ease;
    padding-right: 20px; /* 添加右侧内边距，确保最后一张卡片完全显示 */
}

/* 项目卡片基础样式 */
.project-card {
    flex: 0 0 320px;
    min-width: 320px;
    background: var(--vp-c-bg-soft);
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    padding: 20px;
}

/* 有链接的卡片样式 */
a.project-card {
    text-decoration: none;
    color: inherit;
    cursor: pointer;
}

a.project-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

a.project-card:active {
    transform: translateY(-2px);
}

/* 无链接的卡片样式 */
.project-card.no-link {
    cursor: default;
}

.project-card.no-link:hover {
    transform: none;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* 新增项目头部样式 */
.project-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
}

.project-status {
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 500;
}

.project-status.completed {
    background: rgba(72, 187, 120, 0.1);
    color: #48bb78;
}

.project-status.in-progress {
    background: rgba(236, 201, 75, 0.1);
    color: #ecc94b;
}

.project-title {
    font-size: 18px;
    font-weight: bold;
    color: var(--vp-c-text-1);
}

.project-description {
    font-size: 14px;
    color: var(--vp-c-text-2);
    margin-bottom: 16px;
    line-height: 1.5;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

.project-tag {
    padding: 4px 12px;
    background: var(--vp-c-bg-mute);
    color: var(--vp-c-text-2);
    border-radius: 16px;
    font-size: 12px;
}

/* 暗色模式适配 */
:root.dark .nav-button {
    background: rgba(0, 0, 0, 0.6);
}

:root.dark .nav-button:hover {
    background: rgba(0, 0, 0, 0.8);
}

/* 响应式设计 */
@media (max-width: 768px) {
    .content {
        padding-top: 15vh;
        padding-bottom: 10vh;
    }

    .projects-section {
        padding: 20px 12px;
    }

    .nav-button {
        width: 32px;
        height: 32px;
    }

    .nav-button-left {
        left: -16px;
    }

    .nav-button-right {
        right: -16px;
    }

    .project-card {
        flex: 0 0 280px;
    }
}
</style>