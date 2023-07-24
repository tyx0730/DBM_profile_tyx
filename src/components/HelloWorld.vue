<template>
  <v-app>
    <!-- 抽屉式导航栏 -->
    <v-navigation-drawer v-model="drawer" app>
      <v-list dense>
        <v-list-item v-for="item in nav_items" :key="item.title" @click="navigate(item.route)">
          <v-btn rounded :class="{'nav-item-active': item.route === currentModule.route}">
            <v-icon>{{ item.icon }}</v-icon>
            <span>{{ item.title }}</span>
          </v-btn>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- 页面上方标题 -->
    <v-app-bar app class="appbar_color">
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>{{ currentModule.title }}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon @click="openNotifications">
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      <v-btn icon @click="openSettings">
        <v-icon>mdi-cog-outline</v-icon>
      </v-btn>
    </v-app-bar>

    <!-- 页面内容 -->
    <v-main>
      <!-- 根据当前模块显示对应的内容 -->
      <v-container fluid>
        <template v-if="currentModule.route === 'home'">
          <h1>首页内容</h1>
        </template>
        <template v-else-if="currentModule.route === 'community'">
          <h1>社区内容</h1>
        </template>
        <template v-else-if="currentModule.route === 'training'">
          <h1>训练内容</h1>
        </template>
        <template v-else-if="currentModule.route === 'profile'">
          <v-col cols="6">
            <!-- 个人信息卡片 -->
            <v-card
              class="mx-auto mb-10 first_list"
              width="400"
            >
              <v-layout>
                <v-container fluid>
                  <v-list style="background-color: transparent;">
                    <v-list-item :prepend-avatar="avatar" :title="title" class="text-white mt-4">
                      <v-list-item-content>
                        <v-list-item-subtitle>
                          <v-icon>mdi-pen</v-icon>
                          {{ subtitle }}
                        </v-list-item-subtitle>
                      </v-list-item-content>
                    </v-list-item>
                    <v-divider></v-divider>
                    <div class="d-flex align-center flex-column pa-4 text-white" style="height: 70px;">
                      <v-btn-toggle v-model="toggle" divided variant="outlined">
                        <v-btn v-for="item in four_items" :key="item.icon" icon>
                          <v-icon class="text-white">{{ item.icon }}</v-icon>
                          <div class="text-white">{{ item.text }}</div>
                        </v-btn>
                      </v-btn-toggle>
                    </div>
                    <!-- <v-row class="mt-1 icon-row">
                      <v-col cols="2"></v-col>
                      <v-col v-for="item in four_items" :key="item.icon" cols="2">
                        <v-icon :color="item.color" class="icon">{{ item.icon }}</v-icon>
                        <div class="text">{{ item.text }}</div>
                      </v-col>
                    </v-row> -->
                    <v-divider></v-divider>
                  </v-list>
                  <v-list
                    style="background-color: transparent;"
                    class="text-white"
                  >
                    <v-list-item
                      v-for="(item, i) in prof_items"
                      :key="i"
                      :value="item"
                      color="primary"
                    >
                      <template v-slot:prepend>
                        <v-icon :icon="item.icon"></v-icon>
                      </template>

                      <v-list-item-title>{{ item.text }}</v-list-item-title>
                    </v-list-item>
                  </v-list>
                </v-container>
              </v-layout>
            </v-card>
            <!-- 俱乐部认证卡片 -->
            <v-card
              class="mx-auto mb-10"
              max-width="400"
            >
              <v-img
                src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
                height="150px"
                cover
              ></v-img>

              <v-card-title>{{club_title}}</v-card-title>
              <v-card-subtitle>{{ club_subt }}</v-card-subtitle>

              <v-card-actions>
                <v-btn
                  color="orange-lighten-2"
                  variant="text"
                >
                  进入主页
                </v-btn>
                <v-btn
                  color="orange-lighten-2"
                  variant="text"
                >
                  修改认证
                </v-btn>

                <v-spacer></v-spacer>

                <v-btn
                  :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
                  @click="show = !show"
                ></v-btn>
              </v-card-actions>

              <v-expand-transition>
                <div v-show="show">
                  <v-divider></v-divider>

                  <v-card-text>
                    <v-list
                    :lines="false"
                    density="compact"
                    nav
                    >
                      <v-list-item
                        v-for="(item, i) in club_items"
                        :key="i"
                        :value="item"
                        color="primary"
                      >
                        <template v-slot:prepend>
                          <v-icon :icon="item.icon"></v-icon>
                        </template>

                        <v-list-item-title>{{ item.text }}</v-list-item-title>
                      </v-list-item>
                    </v-list>
                  </v-card-text>
                </div>
              </v-expand-transition>
            </v-card>
            <!-- 赛事认证卡片 -->
            <v-card
              class="mx-auto"
              width="400"
            >
              <v-img
                src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
                height="150px"
                cover
              ></v-img>
              <v-card-title prepend-icon="mdi-medal">赛事认证</v-card-title>


              <v-card-text>
                <v-table
                  fixed-header
                  height="300px"
                >
                  <thead>
                    <tr>
                      <th class="text-left">
                        比赛
                      </th>
                      <th class="text-left">
                        奖项
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in desserts"
                      :key="item.name"
                    >
                      <td>{{ item.name }}</td>
                      <td>{{ item.calories }}</td>
                    </tr>
                  </tbody>
                </v-table>  
                <v-card-actions>
                <v-btn
                  color="orange-lighten-2"
                  variant="text"
                >
                  添加奖项
                </v-btn>
                <v-btn
                  color="orange-lighten-2"
                  variant="text"
                >
                  编辑
                </v-btn>

                <v-spacer></v-spacer>

                <v-btn
                  :icon="show_com ? 'mdi-chevron-up' : 'mdi-chevron-down'"
                  @click="show_com = !show_com"
                ></v-btn>
              </v-card-actions>            
              </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="6">
            <!-- 右侧栏内容 -->
          </v-col>
            
        </template>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false, // 控制导航栏的显示与隐藏
      currentModule: { title: 'tyx,欢迎来到主页', route: 'profile' }, // 当前选中的模块
      nav_items: [
        { title: '首页', icon: 'mdi-home', route: 'home' },
        { title: '社区', icon: 'mdi-account-group', route: 'community' },
        { title: '训练', icon: 'mdi-dumbbell', route: 'training' },
        { title: '个人', icon: 'mdi-account', route: 'profile' }
      ],
      prof_items: [
        { text: '123123123123', icon: 'mdi-cellphone' },
        { text: 'mkbk@123.com', icon: 'mdi-email' },
        { text: 'mkbk俱乐部', icon: 'mdi-account-group' },
      ],
      four_items: [
      { icon: 'mdi-account-plus', text: '100', color: 'white' },
      { icon: 'mdi-account-multiple', text: '500', color: 'white' },
      { icon: 'mdi-medal', text: '10', color: 'white' },
      { icon: 'mdi-trophy', text: '30', color: 'white' }
      ],
      avatar: 'http://0img.mgtv.com/oldpic/preview/star_images/2014/laerfufanensi/laerfufanensi20141231151811857.jpg',
      title: 'mkbk',
      subtitle: '个性签名',
      show: false,
      show_com: false,
      club_title: 'mkbk俱乐部',
      club_subt:'成员',
      club_items: [
        { text: '321321321321', icon: 'mdi-cellphone' },
        { text: 'mkbkclub@123.com', icon: 'mdi-email' },
        { text: '100', icon: 'mdi-account-group' },
        { text: '随便在哪吧', icon: 'mdi-map-marker'}
      ],
    };
  },
  methods: {
    navigate(route) {
      this.currentModule = this.items.find(item => item.route === route);
      this.drawer = false; // 点击后关闭导航栏
    },
    openNotifications() {
      // 打开消息通知
    },
    openSettings() {
      // 打开设置
    }
  }
};
</script>

<style scoped>
.appbar_color{
  background-color:#1296DB;
}
.nav-item-active {
  background-color: #e0e0e0;
}
.v-btn {
  margin: 5px;
}
h1 {
  text-align: center;
  margin-top: 50px;
}
.icon-row {
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;
}
.icon {
  font-size: 20px; /* 将图标字体大小缩小一号 */
}
.text {
  font-size: 10px; /* 将文本字体大小缩小一号 */
  color:white
}
.first_list{
  background-image: url('https://cdn.pixabay.com/photo/2020/07/12/07/47/bee-5396362_1280.jpg');
  height:350px; 
}

</style>