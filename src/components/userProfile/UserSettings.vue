<template>
    <div class="p-4 lg:p-8">
        <!-- 非編輯模式 -->
        <div v-if="!isEditing">
            <!-- 個人照片 -->
            <div class="flex justify-center">
                <img
                    :src="profilePicture"
                    alt="Profile Picture"
                    class="w-24 h-24 rounded-full object-cover border border-gray-300"
                />
            </div>

            <!-- 名稱和數據 -->
             <!-- 若無法順利回傳加入v-if="userData" -->
            <div class="text-center mt-4" >
                <h2 class="text-2xl font-bold text-gray-700">{{ userData?.name || username }}</h2>  
                <div class="flex justify-center space-x-4 text-sm text-gray-500 mt-2">
                    <div>
                        <span class="font-bold text-gray-700">0</span> 餐廳評論
                    </div>
                    <div>
                        <span class="font-bold text-gray-700">0</span> 位粉絲
                    </div>
                </div>
            </div>

            <!-- 個人簡介 -->
            <div class="text-center mt-4">
                <p class="text-gray-600">愛食記新手</p>
            </div>

            <!-- 社群連結 -->
            <div class="mt-4 text-center">
                <p class="text-sm text-gray-500 mb-2">社群連結：</p>
                <a
                    :href="instagramLink"
                    class="text-amber-500 underline hover:text-amber-400"
                    v-if="instagramUsername"
                    target="_blank"
                >
                    Instagram: {{ instagramUsername }}
                </a>
                <p v-else class="text-gray-500"><font-awesome-icon :icon="['fab', 'instagram']" class="text-amber-500 w-5 h-5 mr-1"/>Instagram 未連結</p>
            </div>

            <!-- 編輯個人檔案按鈕 -->
            <div class="mt-6 flex justify-center items-center relative">
                <div class="w-full"></div>
                <!-- 編輯個人檔案按鈕 (保持居中) -->
                <button
                    @click="toggleEditMode"
                    class="w-full min-w-36 py-2 mx-2 bg-amber-500 text-white rounded-md hover:bg-amber-400 focus:outline-none focus:ring-2 focus:ring-amber-500"
                >
                    編輯個人檔案
                </button>

                <!-- ⋯按鈕 (距離編輯個人檔案按鈕右側4px) -->
                <div class="relative w-full ">
                    <button
                        @click="toggleMenu"
                        class=" p-1 text-gray-500 text-2xl hover:bg-amber-100 rounded-md focus:outline-none"
                        >
                        ⋯
                    </button>

                    <!-- 下拉選單 -->
                    <div
                        v-if="menuVisible"
                        class="absolute left-0 mt-2 w-40 bg-white shadow-lg rounded-lg z-10"
                    >
                        <button @click="writeReview" class="block w-full text-left px-4 py-2 text-amber-500 hover:bg-amber-100">
                            撰寫食記
                        </button>
                        <button @click="logout" class="block w-full text-left px-4 py-2 text-amber-500 hover:bg-amber-100">
                            登出
                        </button>
                    </div>
                </div>
            </div>
        
        </div>

<!-- ---------------------------------------------------------------------------------------- -->
        
        <!-- 編輯模式 -->
        <div v-else>
            <!-- 更換照片 -->
            <div class="relative flex justify-center">
                <img
                    :src="profilePicture"
                    alt="Profile Picture"
                    class="w-24 h-24 rounded-full object-cover border border-gray-300"
                />
                <label
                    for="photo-upload"
                    class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-20 rounded-full cursor-pointer"
                    >
                    <span class="text-white text-xl"><font-awesome-icon :icon="['fas', 'camera']" /></span>
                </label>
                <input
                    id="photo-upload"
                    type="file"
                    @change="onPhotoChange"
                    accept="image/*"
                    class="hidden"
                />
            </div>

            <!-- 編輯名字 -->
            <div class="text-center mt-4">
                <input
                    v-model="username"
                    type="text"
                    class="border border-gray-300 rounded-md p-2 text-center w-1/2 focus:outline-none focus:ring-2 focus:ring-amber-500"
                    placeholder="輸入名字"
                />
            </div>

            <!-- 編輯 IG 帳號 -->
            <div class="mt-4 text-center">
                <p class="text-sm text-gray-500 mb-2">您的社群連結：</p>
                <span class="text-amber-500 text-2xl mr-2"><font-awesome-icon :icon="['fab', 'instagram']" class="text-amber-500 w-5 h-5 mr-1"/></span>
                <div class="flex items-center justify-center">
                    <input
                        v-model="instagramUsername"
                        type="text"
                        placeholder="輸入 IG 帳號"
                        class="border border-gray-300 rounded-md p-2 w-1/2 focus:outline-none focus:ring-2 focus:ring-amber-500"
                    />
                </div>
            </div>

            <!-- 保存/取消按鈕 -->
            <div class="mt-6 flex justify-center space-x-4">
                <button
                @click="saveProfile"
                class="px-4 py-2 bg-amber-500 text-white rounded-md hover:bg-amber-400 focus:outline-none focus:ring-2 focus:ring-green-500"
                >
                儲存
                </button>
                <button
                @click="cancelEdit"
                class="px-4 py-2 bg-gray-400 text-white rounded-md hover:bg-gray-500 focus:outline-none focus:ring-2 focus:ring-gray-500"
                >
                取消
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "UserProfile",
    data() {
        return {
            menuVisible: false,
            isEditing: false, // 是否處於編輯模式
            profilePicture: "https://via.placeholder.com/100", // 頭像
            username: "編輯名稱", // 使用者名稱
            instagramUsername: "", // IG 帳號

            userData: null,  //用戶資料
        };
    },
    computed: {
        instagramLink() {
            // 生成 IG 連結
            return `https://instagram.com/${this.instagramUsername}`;
        },
    },
    methods: {
        toggleEditMode() {
            this.isEditing = true; // 進入編輯模式
        },
        saveProfile() {
            // 保存並退出編輯模式
            this.isEditing = false;
        },
        cancelEdit() {
            // 取消編輯，清空 IG 帳號
            this.isEditing = false;
            this.instagramUsername = "";
        },
        toggleMenu() {
        this.menuVisible = !this.menuVisible;
        },
        writeReview() {
        alert("撰寫食記功能即將啟用！");
        },
        // logout() {
        // alert("已登出！");
        // },
        onPhotoChange(event) {
        // 更新頭像
        const file = event.target.files[0];
            if (file) {
                this.profilePicture = URL.createObjectURL(file);
            }
        },

        //拿取用戶資料
        getUserDataFromLocalStorage() {
          const storedUserData = localStorage.getItem("userData"); 
          if (storedUserData) {
            this.userData = JSON.parse(storedUserData); // 
          }
        },
      
        // 登出後清除資料，返回首頁
        logout() {
          localStorage.removeItem("userData"); 
          this.userData = null; // 
          this.$router.push({ name: "home" }); 
        },
    },
    mounted() {
  this.$nextTick(() => {
    this.getUserDataFromLocalStorage();
  });
}

};
</script>

<style scoped>
    /* 標籤覆蓋樣式 */
    label {
        width: 6rem; 
        height: 6rem; 
        transition: background 0.3s;
        left: 50%;
        transform: translateX(-50%);
    }

    label:hover {
        background: rgba(0, 0, 0, 0.5);
    }

    input[type="file"] {
        display: none;
    }
</style>