<template>
    <div>
        <!-- 导航栏   v-show="toolbar.visible"   -->
        <transition name="el-fade-in-linear">
            <div v-show="toolbar.visible" @mouseenter="hoverEnter = true" @mouseleave="hoverEnter = false"
                :class="[{ hoverEnter: hoverEnter && !toolbar.enter }, { enter: toolbar.enter }, { myCenter: !$common.mobile() }, { myBetween: $common.mobile() || mobile }]"
                class="toolbar-content ">

                <!-- 名称 -->
                <div class="toolbar-title " style="margin-right:15px;">
                    <h2 @click="$router.push({ path: '/' })">Cliqin</h2>
                </div>

                <!-- 手机导航按钮 -->
                <div v-if="$common.mobile() || mobile" class="toolbar-mobile-menu " @click="toolbarDrawer = !toolbarDrawer"
                    :class="{ enter: toolbar.enter }">
                    <i class="el-icon-s-operation"></i>
                </div>

                <!-- 电脑导航栏 -->
                <div v-else>
                    <ul class="scroll-menu">
                        <!-- <li @click="$router.push({ path: '/' })">
                            <div class="my-menu">
                                🏡 <span>Home</span>
                            </div>
                        </li> -->
                        <li v-for="(menu, index) in $store.getters.navigationBar"
                            @click="$router.push({ path: '/sort', query: { sortId: menu.id, labelId: menu.labels[0].id } })"
                            :key="index">
                            <div class="my-menu">
                                {{ bookEmoji[index % bookEmoji.length] }} <span>{{ menu.name }}</span>
                            </div>
                        </li>
                        <li @click="$router.push({ path: 'footprint' })">
                            <div class="my-menu">
                                👣 <span>足迹</span>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </transition>
        <!-- 路由的视图容器 -->
        <div id="main-container">
            <router-view></router-view>
        </div>
        <!-- 底部页面主题设置 -->
        <div class="toolButton">
            <div class="backTop" v-if="toolButton" @click="toTop()">
                <!-- 回到顶部按钮 -->
                <svg viewBox="0 0 1024 1024" width="50" height="50">
                    <path
                        d="M696.741825 447.714002c2.717387-214.485615-173.757803-312.227566-187.33574-320.371729-10.857551 5.430775-190.050127 103.168727-187.33274 320.371729-35.297037 24.435488-73.306463 65.1623-67.875688 135.752376 5.430775 70.589076 76.018851 119.460051 103.168726 116.745664 27.152875-2.716387 19.004713-21.7221 19.004713-21.7221l8.148162-38.011425s40.721814 59.732525 51.583363 59.732525h146.609927c13.574938 0 51.585363-59.732525 51.585363-59.732525l8.147162 38.011425s-8.147162 19.005713 19.004713 21.7221c27.148876 2.714388 97.738951-46.156588 103.168727-116.745664s-32.57965-111.316888-67.876688-135.752376z m-187.33574-2.713388c-5.426776 0-70.589076-2.717387-78.733239-78.737238 2.713388-73.306463 73.306463-78.733239 78.733239-81.450626 5.430775 0 76.02385 8.144163 78.736238 81.450626-8.143163 76.019851-73.305463 78.737238-78.736238 78.737238z m0 0"
                        fill="#000000"></path>
                    <path
                        d="M423.602441 746.060699c6.47054-6.297579 12.823107-7.017417 21.629121-2.784372 34.520213 16.582259 70.232157 19.645568 107.031855 9.116944 8.118169-2.323476 15.974396-5.475765 23.598677-9.22392 13.712907-6.73648 26.003134 0.8878 26.080116 16.13936 0.109975 22.574907-0.024994 45.142816 0.080982 67.709725 0.031993 7.464316-2.277486 13.322995-9.44387 16.608254-7.277358 3.333248-13.765895 1.961558-19.526595-3.264264-3.653176-3.313253-7.063407-6.897444-10.634601-10.304675-6.563519-6.259588-6.676494-6.25259-10.625603 1.603638-8.437097 16.80121-16.821205 33.623415-25.257302 50.423625-2.489438 4.953882-5.706713 9.196925-11.411426 10.775569-8.355115 2.315478-15.772442-1.070758-20.272427-9.867774-8.774021-17.15313-17.269104-34.453228-25.918153-51.669344-3.750154-7.469315-3.9891-7.479313-10.141712-1.514658-3.715162 3.602187-7.31435 7.326347-11.142486 10.800563-5.571743 5.060858-11.934308 6.269586-18.936728 3.207277-6.82746-2.984327-9.869774-8.483086-9.892769-15.685462-0.070984-23.506697-0.041991-47.018393-0.020995-70.532089 0.007998-4.679944 1.46467-8.785018 4.803916-11.538397z"
                        fill="#000000"></path>
                </svg>
            </div>
            <!-- 占个位不会导致ui变形 -->
            <div class="backTop" v-else>
                <svg viewBox="0 0 1024 1024" width="50" height="50">
                    <path
                        d=".018851 119.460051 103.168726 116.745664 27.152875-2.716387 19.004713-21.7221 19.004713-21.7221l8.148162-38.011425s40.721814 59.732525 51.583363 59.732525h146.609927c13.574938 0 51.585363-59.732525 51.585363-59.732525l8.147162 38.011425s-8.147162 19.005713 19.004713 21.7221c27.148876 2.714388 97.738951-46.156588 103.168727-116.745664s-32.57965-111.316888-67.876688-135.752376z m-187.33574-2.713388c-5.426776 0-70.589076-2.717387-78.733239-78.737238 2.713388-73.306463 73.306463-78.733239 78.733239-81.450626 5.430775 0 76.02385 8.144163 78.736238 81.450626-8.143163 76.019851-73.305463 78.737238-78.736238 78.737238z m0 0"
                        fill="#000000"></path>
                    <path
                        d="568 107.031855 9.116944 8.118169-2.323476 15.974396-5.475765 23.598677-9.22392 13.712907-6.73648 26.003134 0.8878 26.080116 16.13936 0.109975 22.574907-0.024994 45.142816 0.080982 67.709725 0.031993 7.464316-2.277486 13.322995-9.44387 16.608254-7.277358 3.333248-13.765895 1.961558-19.526595-3.264264-3.653176-3.313253-7.063407-6.897444-10.634601-10.304675-6.563519-6.259588-6.676494-6.25259-10.625603 1.603638-8.437097 16.80121-16.821205 33.623415-25.257302 50.423625-2.489438 4.953882-5.706713 9.196925-11.411426 10.775569-8.355115 2.315478-15.772442-1.070758-20.272427-9.867774-8.774021-17.15313-17.269104-34.453228-25.918153-51.669344-3.750154-7.469315-3.9891-7.479313-10.141712-1.514658-3.715162 3.602187-7.31435 7.326347-11.142486 10.800563-5.571743 5.060858-11.934308 6.269586-18.936728 3.207277-6.82746-2.984327-9.869774-8.483086-9.892769-15.685462-0.070984-23.506697-0.041991-47.018393-0.020995-70.532089 0.007998-4.679944 1.46467-8.785018 4.803916-11.538397z"
                        fill="#000000"></path>
                </svg>
            </div>
            <el-popover placement="left" :close-delay="500" trigger="hover">
                <div slot="reference">
                    <i class="fa fa-cog iconRotate" aria-hidden="true"></i>
                </div>
                <div class="my-setting">
                    <div>
                        <!-- 太阳按钮 -->
                        <i v-if="isDark" class="el-icon-sunny iconRotate" @click="changeColor()"></i>
                        <!-- 月亮按钮 -->
                        <i v-else class="fa fa-moon-o" aria-hidden="true" @click="changeColor()"></i>
                    </div>
                    <!-- <div>
                        <i class="fa fa-snowflake-o" aria-hidden="true" @click="changeMouseAnimation()"></i>
                    </div> -->
                </div>
            </el-popover>
        </div>

        <!-- 点击动画 -->
        <!-- <canvas v-if="mouseAnimation" id="mousedown" style="position:fixed;left:0;top:0;pointer-events:none;z-index: 1000">
        </canvas> -->

        <!-- 手机导航栏 -->
        <el-drawer :visible.sync="toolbarDrawer" :show-close="false" size="65%" custom-class="toolbarDrawer" title="欢迎光临"
            direction="ltr">
            <div>
                <ul class="small-menu">
                    <li @click="smallMenu({ path: '/' })">
                        <div>
                            🏡 <span>首页</span>
                        </div>
                    </li>
                    <li v-for="(menu, index) in $store.getters.navigationBar"
                        @click="smallMenu({ path: '/sort', query: { sortId: menu.id, labelId: menu.labels[0].id } })"
                        :key="index">
                        <div>
                            📒 <span>{{ menu.name }}</span>
                        </div>
                    </li>
                    <li @click="smallMenu({ path: '/footprint' })">
                        <div>
                            👣 <span>足迹</span>
                        </div>
                    </li>
                </ul>
            </div>
        </el-drawer>
    </div>
</template>
  
<!-- 我已经在main.js引入一次jquery了,为啥还要在这引入一次呢? -->
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.12.4/jquery.min.js"></script>

<script>
import axios from "axios";
export default {
    data() {
        return {
            bookEmoji:["📓","📙","📘","📒","📗","📕","📔"],
            hoverEnter: false,
            scrollTop: 0,
            toolButton: false,
            // mouseAnimation: false,
            isDark: false,
            toolbarDrawer: false,
            mobile: false
        }
    },
    mounted() {
        // if (this.mouseAnimation) {
        //     mousedown();
        // }
        //绑定鼠标滚轮监听
        window.addEventListener("scroll", this.onScrollPage);
    },
    destroyed() {
        //取消绑定鼠标滚轮监听
        window.removeEventListener("scroll", this.onScrollPage);
    },
    computed: {
      toolbar() {
        return this.$store.state.toolbar;
      }
    },
    watch: {
        scrollTop(scrollTop, oldScrollTop) {
            //如果滑动距离超过屏幕高度三分之一视为进入页面，背景改为白色
            let enter = scrollTop > window.innerHeight / 2;
            const top = scrollTop - oldScrollTop < 0;
            let isShow = scrollTop - window.innerHeight > 30;
            this.toolButton = isShow;
            //这里设置回到顶部火箭
            if (isShow && 1) {
                if (window.innerHeight > 950) {
                    $(".cd-top").css("top", "0");
                } else {
                    $(".cd-top").css("top", window.innerHeight - 950 + "px");
                }
            } else if (!isShow && 1) {
                $(".cd-top").css("top", "-900px");
            }
            //导航栏显示与颜色
            let toolbarStatus = {
                enter: enter,
                visible: top,
            };
            this.$store.commit("changeToolbarStatus", toolbarStatus);
        },
    },
    created() {
        this.getSortInfo();
    },
    methods: {
        getSortInfo() {
            axios({
				method: 'get',
				url: this.$constant.baseURL+ "/article/sort/",
			}).then(res => {
                if (!this.$common.isEmpty(res.data)) {
                    this.$store.commit("loadSortInfo", res.data.data);
                }
			}).catch((error) => {
				this.$message({
                    message: error.message,
                    type: "error"
                });
			})
        },
        smallMenu(data) {
            this.$router.push(data)
            this.toolbarDrawer = false;
        },
        onScrollPage() {
            this.scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
        },
        toTop() {
            window.scrollTo({
                top: 0,
                behavior: "smooth"
            });
        },
        changeColor() {
            this.isDark = !this.isDark;
            let root = document.querySelector(":root");

            if (this.isDark) {
            root.style.setProperty("--background", "#272727");
            root.style.setProperty("--fontColor", "white");
            root.style.setProperty("--borderColor", "#4F4F4F");
            root.style.setProperty("--borderHoverColor", "black");
            root.style.setProperty("--articleFontColor", "#E4E4E4");
            root.style.setProperty("--articleGreyFontColor", "#D4D4D4");
            root.style.setProperty("--commentContent", "#D4D4D4");
            root.style.setProperty("--favoriteBg", "#1e1e1e");
            } else {
            root.style.setProperty("--background", "white");
            root.style.setProperty("--fontColor", "black");
            root.style.setProperty("--borderColor", "rgba(0, 0, 0, 0.5)");
            root.style.setProperty("--borderHoverColor", "rgba(110, 110, 110, 0.4)");
            root.style.setProperty("--articleFontColor", "#1F1F1F");
            root.style.setProperty("--articleGreyFontColor", "#616161");
            root.style.setProperty("--commentContent", "#F7F9FE");
            root.style.setProperty("--favoriteBg", "#f7f9fe");
            }
        },
        // changeMouseAnimation() {
        //     this.mouseAnimation = !this.mouseAnimation;
        //     if (this.mouseAnimation) {
        //     this.$nextTick(() => {
        //         mousedown();
        //     });
        //     }
        // }
    }
}

</script>
  
<style scoped>
.toolbar-content {
    width: 100%;
    height: 60px;
    color: var(--white);
    /* 固定位置，不随滚动条滚动 */
    position: fixed;
    z-index: 100;
    /* 禁止选中文字 */
    user-select: none;
    transition: all 0.3s ease-in-out;
}

.toolbar-content.enter {
    background: var(--toolbarBackground);
    color: var(--toolbarFont);
    box-shadow: 0 1px 3px 0 rgba(0, 34, 77, 0.05);
}

.toolbar-content.hoverEnter {
    background: rgba(0, 0, 0, 0.5);
    box-shadow: 0 1px 3px 0 rgba(0, 34, 77, 0.05);
}


.toolbar-title {
    margin-left: 30px;
    cursor: pointer;
}

.toolbar-title :hover {
    color: orange;
}

.toolbar-mobile-menu {
    font-size: 30px;
    margin-right: 15px;
    cursor: pointer;
}

.scroll-menu {
    margin: 0 25px 0 0;
    display: flex;
    justify-content: flex-end;
    padding: 0;
}

.scroll-menu>li {
    list-style: none;
    margin: 0 12px;
    font-size: 17px;
    height: 60px;
    line-height: 60px;
    position: relative;
    cursor: pointer;
}

.scroll-menu>li:hover .my-menu span {
    color: orange;
}

.scroll-menu>li:hover .my-menu i {
    color: orange;
    animation: scale 1.5s ease-in-out infinite;
}

/*小黄条加载动画*/
.scroll-menu>li .my-menu:after {
    content: "";
    display: block;
    position: absolute;
    bottom: 0;
    height: 6px;
    background-color: orange;
    width: 100%;
    max-width: 0;
    transition: max-width 0.25s ease-in-out;
}

.scroll-menu>li:hover .my-menu:after {
    max-width: 100%;
}

.el-dropdown {
    font-size: unset;
    color: unset;
}

.el-popper[x-placement^=bottom] {
    margin-top: -8px;
}

.el-dropdown-menu {
    padding: 5px 0;
}

.el-dropdown-menu__item {
    font-size: unset;
}

.el-dropdown-menu__item:hover {
    background-color: var(--white);
    color: orange;
}

.toolButton {
    position: fixed;
    right: 3vh;
    bottom: 3vh;
    animation: slide-bottom 0.5s ease-in-out both;
    z-index: 100;
    cursor: pointer;
    font-size: 25px;
}

.my-setting {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    cursor: pointer;
    font-size: 20px;
}

.my-setting i {
    padding: 5px;
}

.my-setting i:hover {
    color: orange;
}

.cd-top {
    background: var(--toTop) no-repeat center;
    position: fixed;
    right: 5vh;
    top: -900px;
    z-index: 99;
    width: 70px;
    height: 900px;
    background-size: contain;
    transition: all 0.5s ease-in-out;
    cursor: pointer;
}

.backTop {
    transition: all 0.3s ease-in;
    position: relative;
    bottom: 1vh;
    left: -13px;
}

.backTop:hover {
    color: var(--themeBackground);
    top: -13px;
}

@media screen and (max-width: 400px) {
    .toolButton {
        right: 0.5vh;
    }
}
</style>
  