<template>
    <div class="message-card">
      <h1 class="title">💝 帆爱萃宝 💝</h1>
      <div class="message-box" @touchstart="onTouchStart" @touchend="onTouchEnd">
        <p class="message-text">{{ currentMessage }}</p>
      </div>
      <button 
        class="get-message-btn" 
        @click="getLoveMessage" 
        :disabled="isLoading"
      >
        {{ buttonText }}
      </button>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const currentMessage = ref('正在加载情话...')
  const isLoading = ref(false)
  const buttonText = ref('点击获取帆帆想对萃宝说的话')
  
  const API_URL = 'https://api.uomg.com/api/rand.qinghua?format=json'
  
  const backupMessages = [
  "有趣的地方都想去掺和一下，比如你的世界",
  "好好照顾自己，不行就让我来照顾你",
  "跟你捉迷藏的话我肯定输，因为爱你的心怎么也藏不住",
  "我不喜欢等，我只喜欢你。",
  "你陪我走过一无所有，我陪你走到岁月尽头。",
  "我的手被划了一道口子 你也划一下 这样我们就是两口子了。",
  "“我觉得你长得像我一个亲戚。”“谁？”“我妈的儿媳妇。”",
  "我想在老的时候，吻你光光的牙床",
  "我的爱情在你这里，永远不会离去",
  "知道我和唐僧什么区别吗？唐僧取经我娶你",
  "“喂，知道风灌进胸膛是什么感觉么”“不知道啊”“就知道不知道，因为进入的人是你啊。”",
  "“你猜我想吃什么?” “不知道啊。” “痴痴地望着你。”",
  "我有超能力，超喜欢你。",
  "“我怀疑你是碳酸饮料。”“为什么?”“因为一遇见你，我就开心地冒泡泡。”",
  "情人眼里出什么？西施？不，是出现你。",
  "把你的名字写在烟上吸进肺里，让你保持在离我心脏最近的地方。",
  "你最可爱了。我说的时候来不及思索，我仔细想过之后，还是会这么说",
  "你是水，我是鱼，相伴相知。",
  "猜猜我的心在哪边?左边。错了，在你那边。",
  "“要是我和你生一个孩子你觉得他会是什么座？”“什么座？双子座？”“不，我们的杰作。”",
  "你知道这道菜怎么吃最好吃吗? 趁热吗? 我喂你吃。",
  "来者何人？你的人",
  "吃烧烤是先考海鲜还是烤肉 烤肉 先考虑你",
  "我在找一匹马。什么马?你的微信号码。",
  "情话都是学来的，但爱你是真的",
  "“你为什么要害我?”“害你什么?”“害我那么喜欢你!”",
  "“我掐指一算，你五行缺我。”“我是迷人的男人，你是迷人。”",
  "每天只想和你做四件事——一日三餐",
  "今天我想你想的，连续换了五条小内裤",
  "你闻到什么味道了吗?没有啊！怎么你一出来空气都是甜的了。",
  "“你觉得你适合穿什么衣服？”“好看的衣服？”“不，是被我征服。”",
  "春雨初生，春树初盛，春风十里，不如你。",
  "“我这么胖，你为什么还这么喜欢我”“你瘦的时候进我心里，胖的时候就卡住了，出不来了”",
  "“你可以做我的奥利奥吗？”“为什么？”“因为我想泡你。”",
  "甜有100种方式，吃糖，蛋糕，还有每天98次的想你",
  "想送你很多很多口红，让你每天还我一点点",
  "你是甜筒吗？为什么我想舔遍你全身",
  "每日每夜都想你，每时每刻都恋你，一生一世只爱你。",
  "我与世界只差一个你，因为是你，晚一点没关系。",
  "传闻人有206根骨头，一遇见你呀，我就有207根啦",
  "你知道现在是几点嘛？是我们幸福的起点",
  "“请管好你的嘴。”“为什么?”“因为我随时会亲它。”",
  "被你赞过的朋友圈，叫甜甜圈。",
  "我最美的年华，不是你，而是爱你的年华里那些甜蜜的小心情。",
  "“既然你把我的心已经弄乱了，那你打算什么时候来弄乱我的床?”",
  "“你可以笑一个吗？”“为什么啊？”“因为我的咖啡忘加糖了。”",
  "你要你在我身边，我会爱你爱的无法无天。",
  "“别人都说你的脸很圆，但我觉得你的脸还蛮方的” “为什么?” “不然怎么那么正”",
  "你生是我的猪，死是我的五花肉",
  "你猜我爱喝酒还是爱打王者 爱打王者吧? 不，爱你",
  "给你的表白话是抄的，但是我对你的爱是24K纯真的。",
  "我房租到期了，可以去你心里住吗",
  "你和所有的前任比起来，他们不过是前戏，你才是我想要的高潮",
  "我们来玩木头人不许动吧 好!我输了，心动了。",
  "你是我的唯一。",
  "我已经规划好你的新身份了，那就是老婆大人。",
  "“这辈子就跟我在一起吧，不行的话我再等等，还不行的话我再想想别的办法。”",
  "“我在找一条路”“春熙路？”“不是，阿姨洗铁路。”",
  "我还是喜欢你，像小时候吃辣条，不看日期。",
  "你知道你像什么人吗?(什么人?)我的女人。",
  "前半生到处浪荡，后半身为你煲汤",
  "“你想养狗吗？”“什么狗？”“单身的那种”",
  "天再冷只要感受到你掌心里的温度就好。",
  "“我想买一块地。”“什么地?”“你的死心塌地。”",
  "“你最近是不是又胖了?” “没有啊，为什么这么说?” “那为什么在我心里的分量越来越重了?”",
  "最近手头有点紧，想借你的手牵一牵",
  "等下可以帮我洗一下东西吗？”“可以啊，洗什么？”“喜欢我。”",
  "我喜欢你，心就特别软，平淡也浪漫，无语也温暖。",
  "我喜欢你超过来两分钟了，不可以撤回了。",
  "你的酒窝没有酒，我却醉的像条狗",
  "“你有男朋友吗”“没有”“那恭喜你，现在有了。”",
  "陪迩去看细水长流",
  "你笑的时候我就好像拥有了全世界。",
  "和你聊天就像考试，事后想想总是觉没有发挥好。",
  "“咱俩友情到头了，该发展成爱情了。”“别人是喜欢你，而我是只喜欢你。”“我可以称呼你为您吗?这样我就可以把你放在心上。”",
  "你近视吗?(不近视啊)那你怎么看不出我喜欢你。",
  "我是九你是三，除了你还是你",
  "立刻有=like you",
  "“嘿，我想像一首歌给你听” “什么歌?” “打呼噜”",
  "我这个人没见过什么世面，见见你就已经足够了。",
  "玫瑰是我偷的，但我爱你是真的。",
  "你有没有问到什么烧焦的味道?那是我的心在燃烧。",
  "“你是不是喜欢我?”找出这句话中重复的字。",
  "你问我多喜欢你，我说不出来。但是我心里明白，我宁愿和你吵架，也不愿意去爱别人",
  "对你这种人，除了恋爱，我没什么和你谈的！",
  "你无需开口，我和天地万物便通通奔向你。",
  "因为知道不能没有你，所以我会更珍惜。",
  "女孩，我十拿九稳 只差你一吻。",
  "我不需要什么奇迹奇迹，我现在就需要你。",
  "“你能不能闭嘴?”“我没有说话啊”“那为什么我满脑子都是你的声音?”",
  "我没什么事儿，就想听听你的声音。",
  "番茄和西红柿，土豆和马铃薯，我喜欢的人和你",
  "“最近我看到你就觉得头很晕”“为什么?”“因为爱情使人头昏脑胀”",
  "从今以后我只能称呼你为您了，因为，你在我心上。",
  "刚起床，因为梦里有你，舍不得醒来",
  "今天你有点怪，哪里怪？怪好看的！",
  "蓝莓、草莓、蔓越莓，今天想我了没？",
  "心里给你留了一块地，我的死心塌地",
  ]
  
  const getLoveMessage = async () => {
    if (isLoading.value) return
    
    isLoading.value = true
    buttonText.value = '获取中...'
    currentMessage.value = '正在加载情话...'
  
    try {
      const response = await fetch(API_URL)
      if (!response.ok) {
        throw new Error('网络请求失败')
      }
      const data = await response.json()
      if (data.code === 1 && data.content) {
        currentMessage.value = data.content
      } else {
        throw new Error('API返回格式错误')
      }
    } catch (error) {
      console.error('获取情话失败:', error)
      // 使用本地备用情话
      const randomIndex = Math.floor(Math.random() * backupMessages.length)
      currentMessage.value = backupMessages[randomIndex]
    } finally {
      isLoading.value = false
      buttonText.value = '点击获取帆帆想对萃宝说的话'
    }
  }
  
  const onTouchStart = (e) => {
    e.target.style.transform = 'scale(0.98)'
  }
  
  const onTouchEnd = (e) => {
    e.target.style.transform = 'scale(1)'
  }
  
  onMounted(() => {
    getLoveMessage()
  })
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .message-card {
    background: rgba(255, 255, 255, 0.95);
    border-radius: 20px;
    padding: 40px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    text-align: center;
    max-width: 600px;
    width: 90%;
  }
  
  .title {
    color: #ff6b6b;
    font-size: 2.5em;
    margin-bottom: 30px;
    font-weight: bold;
  }
  
  .message-box {
    background: #fff5f5;
    border-radius: 15px;
    padding: 20px;
    margin: 20px 0;
    min-height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.2s ease;
  }
  
  .message-text {
    font-size: 1.2em;
    color: #ff4757;
    line-height: 1.6;
    margin: 0;
  }
  
  .get-message-btn {
    background: #ff6b6b;
    color: white;
    border: none;
    padding: 15px 30px;
    border-radius: 25px;
    font-size: 1.1em;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-top: 20px;
  }
  
  .get-message-btn:hover {
    background: #ff4757;
    transform: translateY(-2px);
  }
  
  .get-message-btn:disabled {
    background: #ffb8b8;
    cursor: not-allowed;
    transform: none;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .message-box {
    animation: fadeIn 0.5s ease-out;
  }
  </style> 
