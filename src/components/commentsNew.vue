<template>
  <div class="replys">
    <div v-clickoutside="hideReplyBtn" @click="inputFocus" class="my-reply">
      <el-avatar class="header-img" :size="40" :src="myHeader"></el-avatar>
      <div class="reply-info">
        <!--        @focus是元素获取焦点时所触发的事件
                    @input适用于实时查询，每输入一个字符都会触发该事件
                    tabindex: 全局属性 指示其元素是否可以聚焦，以及它是否/在何处参与顺序键盘导航（通常使用Tab键，因此得名）。
        -->
        <div
            tabindex="0"
            contenteditable="true"
            id="replyInput"
            spellcheck="false"
            placeholder="输入评论..."
            class="reply-input"
            @focus="showReplyBtn"
            @input="onDivInput($event)"
        >
        </div>
      </div>
      <div class="reply-btn-box" v-show="btnShow">
        <el-button class="reply-btn" size="medium" @click="sendComment" type="primary">发表评论</el-button>
      </div>
    </div>
    <div v-for="(item,i) in comments" :key="i" class="author-title reply-father">
      <!--      评论区头像-->
      <el-avatar class="header-img" :size="40" :src="item.headImg"></el-avatar>
      <div class="author-info">
        <!--        用户名及发布时间-->
        <span class="author-name">{{item.name}}</span>
        <span class="author-time">{{item.time}}</span>
      </div>
      <div class="icon-btn icon-zifu">
        <!--        回复数量-->
        <span @click="showReplyInput(i,item.name,item.id)">
                    <i class="iconfont icon-duihuaxinxitianchong"></i>{{item.commentNum}}</span>
        <!--        点赞数量-->
        <i class="iconfont icon-dianzan_kuai" @click="likeClick(item)"></i>{{item.like}}
      </div>
      <!--      一级评论信息-->
      <div class="talk-box">
        <p>
          <span class="reply">{{item.comment}}</span>
        </p>
      </div>
      <!--      二级评论信息-->
      <div class="reply-box">
        <div v-for="(reply,j) in item.reply" :key="j" class="author-title">
          <el-avatar class="header-img" :size="40" :src="reply.fromHeadImg"></el-avatar>
          <div class="author-info">
            <span class="author-name">{{reply.from}}</span>
            <span class="author-time">{{reply.time}}</span>
          </div>
          <div class="icon-btn icon-zifu">
            <span @click="showReplyInput(i,reply.from,reply.id)">
<!--              <i class="iconfont icon-duihuaxinxitianchong"></i>{{reply.commentNum}}-->
              <i class="iconfont icon-duihuaxinxitianchong"></i>回复
            </span>
            <i class="iconfont icon-dianzan_kuai" @click="likeClick(reply)"></i>{{reply.like}}
          </div>
          <div class="talk-box">
            <p>
              <span>回复 {{reply.to}}:</span>
              <span class="reply">{{reply.comment}}</span>
            </p>
          </div>
          <div class="reply-box">

          </div>
        </div>
      </div>
      <!--回复二级评论-->
      <div v-show="_inputShow(i)" class="my-reply my-comment-reply">
        <el-avatar class="header-img" :size="40" :src="myHeader"></el-avatar>
        <div class="reply-info">
          <div tabindex="0" contenteditable="true" spellcheck="false" placeholder="输入评论..."
               @input="onDivInput($event)" class="reply-input reply-comment-input"></div>
        </div>
        <div class=" reply-btn-box">
          <el-button class="reply-btn" size="medium" @click="sendCommentReply(i,j)" type="primary">发表评论
          </el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/font/iconfont.css'
import imageone from '@/assets/images/1.jpg'
import imagetwo from '@/assets/images/2.jpg'
import imagethree from '@/assets/images/3.jpg'
import imagefour from '@/assets/images/4.jpg'

const clickoutside = {
  // 初始化指令
  // eslint-disable-next-line
  bind(el, binding, vnode) {
    function documentHandler(e) {
      // 这里判断点击的元素是否是本身，是本身，则返回
      if (el.contains(e.target)) {
        return false;
      }
      // 只有不是本身 才会响应事件
      // 判断指令中是否绑定了函数
      if (binding.expression) {
        // 如果绑定了函数 则调用那个函数，此处binding.value就是handleClose方法
        binding.value(e);
      }
    }

    // 给当前元素绑定个私有变量，方便在unbind中可以解除事件监听
    el.vueClickOutside = documentHandler;
    document.addEventListener('click', documentHandler);
  },
  // update() {
  // },
  // vue 弹出框 下面 响应
  // eslint-disable-next-line
  unbind(el, binding) {
    // 解除事件监听
    document.removeEventListener('click', el.vueClickOutside);
    delete el.vueClickOutside;
  },
};
export default {
  name: 'ArticleComment',
  data() {
    return {
      //我的评论
      btnShow: false,
      index: '0',
      replyComment: '',
      myName: '张三',
      // myHeader: 'https://p9-passport.byteacctimg.com/img/mosaic-legacy/3795/3047680722~300x300.image',
      myHeader: imageone,
      myId: 19870621,
      to: '',
      toId: -1,
      //评论区
      comments: [
        {
          name: '张三',
          id: 19870621,
          // headImg: 'https://p9-passport.byteacctimg.com/img/mosaic-legacy/3795/3047680722~300x300.image',
          headImg: imageone,
          comment: '1212122',
          time: '2022年8月14日 8:43',
          commentNum: 2,
          like: 15,
          inputShow: false,
          reply: [
            {
              from: '王二',
              fromId: 19891221,
              // fromHeadImg: 'https://p6-passport.byteacctimg.com/img/mosaic-legacy/3791/5070639578~300x300.image',
              headImg: imagetwo,
              to: '张三',
              toId: 19870621,
              comment: '21312313123',
              time: '2022年8月14日 8:43',
              commentNum: 1,
              like: 15,
              inputShow: false
            },
            {
              from: '李四',
              fromId: 1123,
              // fromHeadImg: 'https://p6-passport.byteacctimg.com/img/user-avatar/22358593c3d2416498aa150ccf8d645f~300x300.image',
              headImg: imagethree,
              to: '张三',
              toId: 19870621,
              comment: '3131312123',
              time: '2022年8月14日 8:43',
              commentNum: 0,
              like: 5,
              inputShow: false

            }
          ]
        },
        {
          name: '王二',
          id: 19891221,
          // headImg: 'https://p6-passport.byteacctimg.com/img/mosaic-legacy/3791/5070639578~300x300.image',
          headImg: imagetwo,
          comment: '213131313',
          time: '2022年8月14日 8:43',
          commentNum: 1,
          like: 5,
          inputShow: false,
          reply: [
            {
              from: '张三',
              fromId: 19870621,
              // fromHeadImg: 'https://p9-passport.byteacctimg.com/img/mosaic-legacy/3795/3047680722~300x300.image',
              headImg: imageone,
              to: '麻子',
              toId: 19891221,
              comment: '23123122113',
              time: '2022年8月14日 8:43',
              commentNum: 25,
              like: 5,
              inputShow: false

            }
          ]
        },
        {
          name: '麻子',
          id: 20190830,
          // headImg: 'https://p9-passport.byteacctimg.com/img/mosaic-legacy/3795/3047680722~300x300.image',
          headImg: imagefour,
          comment: '12312312312',
          time: '2022年8月14日 8:43',
          commentNum: 0,
          like: 5,
          inputShow: false,
          reply: []
        },
      ]
    }
  },
  //自定义组件
  directives: {clickoutside},
  methods: {
    messageTitleClick(num) { // 点击了具体某条消息
      console.log("点击了消息", num);
    },
    //获取评论框时,为评论框添加一个ID,修改样式
    inputFocus() {
      var replyInput = document.getElementById('replyInput');
      replyInput.style.padding = "8px 8px"
      replyInput.style.border = "2px solid blue"
      //在文档加载后，为文本域设置焦点:
      replyInput.focus()
    },
    //显示评论框
    showReplyBtn() {
      this.btnShow = true
    },
    hideReplyBtn() {
      let replyInput = document.getElementById('replyInput')
      this.btnShow = false
      replyInput.style.padding = "10px"
      replyInput.style.border = "none"
    },
    //获取回复人的信息,并修改是否显示输入框
    showReplyInput(i, name, id) {
      this.comments[this.index].inputShow = false
      this.index = i
      this.comments[i].inputShow = true
      this.to = name
      this.toId = id
    },
    //返回是否显示输入框
    _inputShow(i) {
      return this.comments[i].inputShow
    },
    //创建一个一级评论
    sendComment() {
      if (!this.replyComment) {
        this.$message({
          showClose: true,
          type: 'warning',
          message: '评论不能为空'
        })
      }
      else {
        let a = {}
        let input = document.getElementById('replyInput')
        let timeNow = new Date().getTime();
        let time = this.dateStr(timeNow);
        a.name = this.myName
        a.comment = this.replyComment
        a.headImg = this.myHeader
        a.time = time
        a.commentNum = 0
        a.like = 0
        //添加,解决新发布的评论,无法回复
        a.reply = []
        this.comments.push(a)
        this.replyComment = ''
        input.innerHTML = '';

      }
    },
    //创建一个二级评论
    // eslint-disable-next-line
    sendCommentReply(i, j) {
      if (!this.replyComment) {
        this.$message({
          showClose: true,
          type: 'warning',
          message: '评论不能为空'
        })
      } else {
        let a = {}
        let timeNow = new Date().getTime();
        let time = this.dateStr(timeNow);
        a.from = this.myName
        a.to = this.to
        a.fromHeadImg = this.myHeader
        a.comment = this.replyComment
        a.time = time
        a.commentNum = 0
        a.like = 0
        this.comments[i].reply.push(a)
        this.replyComment = ''
        document.getElementsByClassName("reply-comment-input")[i].innerHTML = ""
      }
    },
    onDivInput: function (e) {
      //e.target就是指发生事件的对象，而this则是谁调用它则指向谁。
      //e.target.innerText 获取触发onDivInput事件的对象中的内容
      this.replyComment = e.target.innerText;
    },
    dateStr(date) {
      //获取js 时间戳
      var time = new Date().getTime();
      //去掉 js 时间戳后三位，与php 时间戳保持一致
      time = parseInt((time - date) / 1000);
      //存储转换值
      var s;
      if (time < 60 * 10) {//十分钟内
        return '刚刚';
      } else if ((time < 60 * 60) && (time >= 60 * 10)) {
        //超过十分钟少于1小时
        s = Math.floor(time / 60);
        return s + "分钟前";
      } else if ((time < 60 * 60 * 24) && (time >= 60 * 60)) {
        //超过1小时少于24小时
        s = Math.floor(time / 60 / 60);
        return s + "小时前";
      } else if ((time < 60 * 60 * 24 * 30) && (time >= 60 * 60 * 24)) {
        //超过1天少于30天内
        s = Math.floor(time / 60 / 60 / 24);
        return s + "天前";
      } else {
        //超过30天ddd
        // var date = new Date(parseInt(date));
        date = new Date(parseInt(date));
        return date.getFullYear() + "/" + (date.getMonth() + 1) + "/" + date.getDate();
      }
    },
    //点赞
    likeClick(item) {
      item.like++;
    },
  },
}
</script>

<style scoped lang="css">
.icon-zifu{
  /*使文字无法被选中*/
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.replys{
  width: 50%;
  margin: 0 auto;

}
.my-reply {
  padding: 10px;
  background-color: #fafbfc;
}

.my-reply .header-img {
  display: inline-block;
  /*使元素及其后代元素的顶部与整行的顶部对齐*/
  vertical-align: top;
}

.my-reply .reply-info {
  display: inline-block;
  margin-left: 5px;
  width: 90%;
}
/*最大宽度为1200px,则评论宽度为80%*/
@media screen and (max-width: 1200px) {
  .my-reply .reply-info {
    width: 80%;
  }
}

.my-reply .reply-info .reply-input {
  min-height: 20px;
  line-height: 22px;
  padding: 10px 10px;
  color: #252933;
  background-color: rgba(178, 186, 194, 0.3);
  border-radius: 5px;
}
/*attr() 理论上能用于所有的 CSS 属性但目前支持的仅有伪元素的 content 属性
attr() 用来获取选择到的元素的某一 HTML 属性值，并用于其样式*/
/*:empty CSS 伪类 代表没有子元素的元素*/
/*:before 创建一个伪元素，其将成为匹配选中的元素的第一个子元素。常通过 content 属性来为一个元素添加修饰性的内容。此元素默认为行内元素。*/
.my-reply .reply-info .reply-input:empty:before {
  content: attr(placeholder);
}
/*:focus表示获得焦点的元素（如表单输入）。当用户点击或触摸元素或通过键盘的 “tab” 键选择它时会被触发。*/
.my-reply .reply-info .reply-input:focus:before {
  content: none;
}

.my-reply .reply-info .reply-input:focus {
  padding: 8px 8px;
  border: 2px solid #00f;
  /*box-shadow 属性用于在元素的框架上添加阴影效果。*/
  box-shadow: none;
  /*outline 属性是在一条声明中设置多个轮廓属性的简写属性*/
  outline: none;
}

.my-reply .reply-btn-box {
  height: 25px;
  margin: 10px 0;
}

.my-reply .reply-btn-box .reply-btn {
  position: relative;
  float: right;
  margin-right: 15px;
}

.my-comment-reply {
  margin-left: 50px;
}

.my-comment-reply .reply-input {
  /*width: flex;*/
}
/*:not() 用来匹配不符合一组选择器的元素*/
/*:last-child CSS 伪类 代表父元素的最后一个子元素。*/
.author-title:not(:last-child) {
  border-bottom: 1px solid rgba(178, 186, 194, 0.3);
}

.author-title {
  margin-top: 1rem;
  padding: 10px;
}

.author-title .header-img {
  display: inline-block;
  vertical-align: top;
}

.author-title .author-info {
  display: inline-block;
  margin-left: 5px;
  width: 60%;
  height: 40px;
  line-height: 20px;
}

.author-title .author-info > span {
  display: block;
  /*cursor CSS 属性设置光标的类型（如果有），在鼠标指针悬停在元素上时显示相应样式。
 cursor: pointer; 悬浮鼠标为小手
 */
  cursor: pointer;
  overflow: hidden;
  /*white-space CSS 属性是用来设置如何处理元素中的 空白
  连续的空白符会被合并。但文本内的换行无效。
  */
  white-space: nowrap;
  /*text-overflow CSS 属性用于确定如何提示用户存在隐藏的溢出内容。其形式可以是裁剪、显示一个省略号（“…”）或显示一个自定义字符串
  ellipsis 关键字会用一个省略号（'…'、U+2026 HORIZONTAL ELLIPSIS）来表示被截断的文本。
  */
  text-overflow: ellipsis;
}

.author-title .author-info .author-name {
  color: #000;
  font-size: 18px;
  font-weight: bold;
}

.author-title .author-info .author-time {
  font-size: 14px;
}

.author-title .icon-btn {
  width: 30%;
  padding: 0 !important;
  float: right;
}

@media screen and (max-width: 1200px) {
  .author-title .icon-btn {
    width: 20%;
    padding: 7px;
  }
}

.author-title .icon-btn > span {
  cursor: pointer;
}

.author-title .icon-btn .iconfont {
  margin: 0 5px;
}

.author-title .talk-box {
  margin: 0 50px;
}

.author-title .talk-box > p {
  margin: 0;
}

.author-title .talk-box .reply {
  font-size: 16px;
  color: #000;
}

.author-title .reply-box {
  margin: 10px 0 0 50px;
  background-color: #efefef;
}

</style>
