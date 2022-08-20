<template>
  <div class="reply">
<!--    评论区域-->
    <div class="my-reply" >
      <div class="header">
<!--        标题-->
        <span class="header-title">评论</span>
      </div>
<!--      评论输入框及发布按钮-->
      <div class="content">
<!--        评论发布人头像-->
        <div class="avatar-box">
          <img :src="commentThis.myHeader" alt="" class="avatar">
        </div>
        <div class="form-box">
<!--          评论输入框-->
          <div class="auth-card">
            <div class="input-box">
              <div
                  class="rich-input empty"
                  contenteditable="true"
                  placeholder="请输入内容"
                  spellcheck="false"
                  disabled="disabled"
                  @focus="showReplyBtn"
                  @input="onDivInput($event)"
              ></div>
            </div>
          </div>
<!--          评论输入框下的发布按钮,当输入框获取焦点时,显示发布按钮-->
          <div class="action-box" v-show="commentThis.btnShow">
            <div class="emoji-container emoji-btn">
              <div class="emoji-box">
                <i class="iconfont icon-shoucang icon"><span>表情</span></i>
              </div>
            </div>
            <div class="image-btn">
              <i class="iconfont icon-duihuaxinxitianchong icon"><span>图片</span></i>
            </div>
            <div class="submit">
              <span>Ctrl + Enter</span>
              <!--             disabled 赋予该属性时该元素将变得不可交互-->
              <button class="submit-btn"  @click="sendComment">发布评论</button>
            </div>
          </div>
        </div>
      </div>
    </div>
<!--    显示所有评论的区域-->
    <div class="others-reply" @click="UnShowReplyBtnOne">
<!--      标题-->
      <div class="title">
        全部评论
        <i></i>
      </div>
      <!--      一级评论区-->
      <div v-for="(item,i) in commentThis.comments" :key="i" class="list">
        <div class="comment">
<!--          评论人头像-->
          <div class="popover-box ">
            <a href="#" class="user-link">
              <img :src="item.headImg" alt="" class="avatar">
            </a>
          </div>

          <div class="content-box">
            <div class="comment-main">
              <div class="user-box">
<!--              显示评论人名称  -->
                <div class="user-popover">
                  <a href="#" class="username">
                    <span class="name">{{item.name}}</span>
                  </a>
                </div>
<!--               显示评论发布时间 -->
                <time class="time">{{item.time}}</time>
              </div>
<!--              显示评论内容-->
              <div class="content">{{item.comment}}</div>
<!--              点赞及回复-->
              <div class="action-box">
<!--                点赞-->
                <div class="item dig-item" @click="addThumbsUp(item)">
                  <i class="iconfont icon-dianzan_kuai"><span>{{item.like}}</span></i>
                </div>
<!--                回复-->
                <div class="item " @click="showReplyInput(i,item.name,item.id)">
                  <i class="iconfont icon-duihuaxinxitianchong"><span>{{item.commentNum}}</span></i>
                </div>
              </div>
            </div>
            <!--            二级评论区-->
            <div class="subcomment-wrapper" @click="UnShowReplyBtnTwo(i)">
              <div v-for="(reply,j) in item.reply" :key="j" class="sub-comment-list">
                <!--          评论人头像-->
                <div class="popover-box">
                  <a href="#" class="user-link">
                    <img :src="reply.headImg" alt="" class="avatar">
                  </a>
                </div>
                <div class="content-box">
                  <div class="content-wrapper">
                    <div class="user-box">
<!--                      评论人名称-->
                      <div class="user-popover">
                        <a href="#" class="username">
                          <span class="name">{{reply.from}}</span>
                        </a>
                      </div>
<!--                      被回复评论人名称-->
                      <div class="rely-box">
                        <span>回复</span>
                        <div class="user-popover">
                          <a href="#" class="repliedname">{{reply.to}}</a>
                        </div>
                      </div>
<!--                      评论发布时间-->
                      <time class="time">{{reply.time}}</time>
                    </div>
<!--                    评论内容-->
                    <div class="content">{{reply.comment}}</div>
                    <div class="action-box">
<!--                      点赞-->
                      <div class="item dig-item" @click="addThumbsUp(reply)">
                        <i class="iconfont icon-dianzan_kuai"><span>{{reply.like}}</span></i>
                      </div>
<!--                      回复-->
                      <div class="item " @click="showReplyInput(i,reply.from,reply.id)" @click.stop="">
                        <i class="iconfont icon-duihuaxinxitianchong"><span>回复</span></i>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!--            回复评论区-->
            <div class="comment-form" v-show="_inputShow(i)" >
              <div class="content">
                <div class="form-box">
<!--                  回复评论输入框-->
                  <div class="auth-card">
                    <div class="input-box input-box-comment">
                      <div
                          class="rich-comment-input"
                          contenteditable="true"
                          placeholder="请输入内容"
                          spellcheck="false"
                          disabled="disabled"
                          @input="onDivInput($event)"
                      ></div>
                    </div>
                  </div>
<!--                  发布评论按钮及表情,图片图标-->
                  <div class="action-box">
<!--                    表情-->
                    <div class="emoji-container emoji-btn">
                      <div class="emoji-box">
                        <i class="iconfont icon-shoucang icon"><span>表情</span></i>
                      </div>
                    </div>
<!--                    图片-->
                    <div class="image-btn">
                      <i class="iconfont icon-duihuaxinxitianchong icon"><span>图片</span></i>
                    </div>
<!--                    评论-->
                    <div class="submit">
                      <span>Ctrl + Enter</span>
                      <button class="submit-btn submit-btn-new" @click="sendCommentReply(i)">评论</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/font/iconfont.css'
export default {
  name: 'NewComment',
  props:{
    commentse: {
      required: true,
    },
  },
  data(){
    return{
      commentThis: this.commentse
    }
  },
  methods: {
    //  1.评论区域输入框获取焦点后,显示发布按钮等
    showReplyBtn(){
      this.commentThis.btnShow = true
      var richInput = document.getElementsByClassName('input-box')[0];
      richInput.style.border = "2px solid blue"
    },
    // 2. 在评论区输入框为空且点击全部评论区域时,隐藏发布按钮
    UnShowReplyBtnOne(){
      if (!this.commentThis.replyComment && this.commentThis.btnShow){
        this.commentThis.btnShow = false
        var richInput = document.getElementsByClassName('input-box')[0];
        richInput.style.border = "2px solid #fff"
      }
    },
    //3.在回复区输入框为空且点击全部评论区域时,隐藏发布按钮
    UnShowReplyBtnTwo(i){
      if (!this.commentThis.replyComment && this.commentThis.comments[i].inputShow){
        this.commentThis.comments[i].inputShow = false
      }
    },

    //4. 评论区域输入框输入内容时,改变发布按钮的颜色,获取输入框中的内容
    onDivInput: function (e) {
      this.commentThis.replyComment = e.target.innerText;
    },
    //5. 点击发布按钮,创建一个新的一级评论
    sendComment(){
      if (!this.commentThis.replyComment) {
        this.commentThis.$message({
          showClose: true,
          type: 'warning',
          message: '评论不能为空'
        })
      }
      else {
        let a = {}
        let input = document.getElementsByClassName('rich-input')[0]
        let timeNow = new Date().getTime();
        let time = this.dateStr(timeNow);
        a.name = this.commentThis.myName
        a.comment = this.commentThis.replyComment
        a.headImg = this.commentThis.myHeader
        a.time = time
        a.commentNum = 0
        a.like = 0
        //添加,解决新发布的评论,无法回复
        a.reply = []
        this.commentThis.comments.push(a)
        this.commentThis.replyComment = ''
        input.innerHTML = '';
      }
    },
    //6.返回是否显示输入框
    _inputShow(i) {
      return this.commentThis.comments[i].inputShow
    },
    //7. 点击发布按钮,创建一个新的二级评论
    sendCommentReply(i) {
      if (!this.commentThis.replyComment) {
        this.commentThis.$message({
          showClose: true,
          type: 'warning',
          message: '评论不能为空'
        })
      } else {
        let a = {}
        let timeNow = new Date().getTime();
        let time = this.dateStr(timeNow);
        a.from = this.commentThis.myName
        a.to = this.commentThis.to
        a. headImg = this.commentThis.myHeader
        a.comment = this.commentThis.replyComment
        a.time = time
        a.commentNum = 0
        a.like = 0
        this.commentThis.comments[i].reply.push(a)
        this.commentThis.replyComment = ''
        document.getElementsByClassName("rich-comment-input")[i].innerHTML = ""
      }
    },
    //8. 获取回复人的信息,并修改是否显示输入框
    showReplyInput(i, name, id) {
      //关闭上一个评论的输入框
      this.commentThis.comments[this.commentThis.index].inputShow = false
      //将当前边框返回
      this.commentThis.index = i
      this.commentThis.comments[i].inputShow = true
      this.commentThis.to = name
      this.commentThis.toId = id
    },
    //9. 点赞增加
    addThumbsUp(item){
      item.like++
    },
    //根据发布时间修改显示的时间
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
  }
}
</script>

<style scoped lang="css">
/*布局采用伸缩布局,使用时可以直接更改 .reply 中的宽高百分比,或直接设置父组件的宽高*/
.reply{
  width: 100%;
  height: 100%;
  margin: 0 auto;
}
.reply .my-reply{
  width: 100%;
  background-color: #fff;
  padding: 30px;
  padding-bottom: 0px;
  box-sizing: border-box;
}
.my-reply .header{
  margin-bottom: 30px;
}
.header .header-title{
  font-size: 18px;
  line-height: 30px;
  font-weight: 600;
  color: #252933;
}
.my-reply .content{
  /*伸缩布局*/
  display: flex;
  /*元素位于容器的开头。 弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。*/
  align-items: flex-start;
}
.content .avatar-box{
  flex: 0 0 auto;
}
.avatar-box .avatar{
  margin-right: 16px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.content .form-box{
  flex: 1 1 auto;
  position: relative;
}
.form-box .auth-card{
  position: relative;
}
.auth-card .input-box{
  font-size: 0;
  transition: all .3s;
  background: #F0F1F4FF;
  border: 1px solid #fff;
  border-radius: 4px;
}
.input-box .rich-input{
  position: relative;
  padding: 8px 12px;
  color: #252933;
  outline: none;
  min-height: 64px;
  box-sizing: border-box;
  line-height: 22px;
  font-size: 14px;
  resize: both;
}
/*没有获得焦点时*/
/*:empty CSS 伪类 代表没有子元素的元素*/
/*:before 创建一个伪元素，其将成为匹配选中的元素的第一个子元素。常通过 content 属性来为一个元素添加修饰性的内容。此元素默认为行内元素。*/
.rich-input:empty:before{
  content: attr(placeholder);/*显示placeholder中的内容*/
  /*content: 'this.commentThis is content';*/
  color: #252933;
}
/*获得焦点时*/
/*:focus表示获得焦点的元素（如表单输入）。当用户点击或触摸元素或通过键盘的 “tab” 键选择它时会被触发。*/
.rich-input:focus{
  content:none;
}
.form-box .action-box{
  display: flex;
  align-items: center;
  margin-top: 8px;
}
.action-box .emoji-btn{
  user-select: none;
}
.emoji-container{
  position: relative;
  z-index: 1;
}
.emoji-container .emoji-box{
  display: flex;
  align-items: center;
  position: relative;
  /*设置光标的类型*/
  cursor: pointer;
  color: #515767;
}
.emoji-box .icon{
  background-repeat: no-repeat;
  background-size: cover;
}
.emoji-box span{
  color: #515767;
  font-size: 13px;
  line-height: 22px;
  padding-left: 4px;
}
.emoji-box :hover{
  color: #1e80ff;
}
.action-box .image-btn{
  display: flex;
  align-items: center;
  position: relative;
  cursor: pointer;
  margin-left: 24px;
  color: #515767;
}
.image-btn .icon{
  background-repeat: no-repeat;
  background-size: cover;

}
.image-btn span{
  color: #515767;
  font-size: 14px;
  line-height: 22px;
  padding-left: 4px;
}
.image-btn :hover{
  color: #1e80ff;
}
.action-box .submit{
  flex: 0 0 auto;
  margin-left: auto;
}
.submit span{
  font-size: 14px;
  line-height: 22px;
  /*用于设置文本字符的间距表现。*/
  letter-spacing: 2px;
  color: #86909c;
  margin-right: 16px;
}
.submit .submit-btn{
  flex: 0 0 auto;
  margin-left: auto;
  width: 92px;
  text-align: center;
  font-size: 14px;
  line-height: 36px;
  background: #1e80ff;
  border: 0 solid #fff;
  border-radius: 4px;
  box-sizing: border-box;
  color: #fff;
  padding: 0;
  cursor: pointer;
}

.reply .others-reply{
  width: 100%;
  height: 100%;
  position: relative;
  margin: 0 auto;
  padding: 30px;
  margin-top: 0;
  box-sizing: border-box;
}
.others-reply .title{
  position: relative;
  padding-bottom: 8px;
  font-weight: 600;
  font-size: 18px;
  color: #252933;
  width: 100%;
  display: flex;
  align-items: center;
  line-height: 30px;
}
.others-reply .list .comment{
  display: flex;
  padding: 16px 0;
}
.comment .popover-box{
  display: inline;
  height: 33px;
}
.popover-box .user-link{
  /*flex:设置了弹性项目如何增大或缩小以适应其弹性容器中可用的空间*/
  flex: 0 0 auto;
}
.user-link .avatar{
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.comment .content-box{
  flex: 1 1 auto;
  margin-left: 16px;
}
.content-box .comment-main{
  position: relative;
}
.comment-main .user-box{
  display: flex;
  align-items: center;
  position: relative;
}
.user-box .user-popover{
  display: inline;
}
.user-popover .username{
  font-size: 16px;
  font-weight: 600;
  color: #2e3135;
  text-decoration: none;
  display: flex;
  align-items: center;
}
.username .name{
  font-weight: 500;
  font-size: 15px;
  color: #252933;
  max-width: 90px;
  line-height: 26px;
  display: inline-block;
  /*vertical-align 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。*/
  vertical-align: top;
  overflow: hidden;
  /*text-overflow CSS 属性用于确定如何提示用户存在隐藏的溢出内容。其形式可以是裁剪、显示一个省略号（“…”）或显示一个自定义字符串。*/
  text-overflow: ellipsis;
  /*white-space CSS 属性是用来设置如何处理元素中的 空白 (en-US)。*/
  white-space: nowrap;
}

.user-box .time{
  font-size: 14px;
  color: #8a919f;
  line-height: 22px;
  margin-left: auto;
}
.comment-main .content{
  font-weight: 400;
  font-size: 14px;
  line-height: 2rem;
  color: #515767;
  margin-top: 8px;
  -webkit-line-clamp: 6;
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-box-orient: vertical;
}
.comment-main .action-box{
  display: flex;
  align-items: center;
  margin-top: 8px;
  /*user-select 控制用户能否选中文本。除了文本框内，*/
  user-select: none;
}
.action-box .item{
  margin-right: 16px;
  line-height: 22px;
  font-size: 14px;
  cursor: pointer;
  color: #8a919f;
  display: flex;
  align-items: center;
}
.action-box .item:hover{
  color: #1e80ff;
}
.action-box .item i{
  fill: #8a919f;
  margin-right: 4px;
  overflow: hidden;
}
.action-box .item i span{
  margin-left: 5px;
}

.content-box .subcomment-wrapper{
  padding: 16px;
  background: rgba(247,248,250,.7);
  border-radius: 4px;
  margin-top: 16px;
}
.subcomment-wrapper .sub-comment-list{
  display: flex;
}
.sub-comment-list:not(:first-child){
  margin-top: 20px;
}
.sub-comment-list .popover-box{
  display: inline;
}
.popover-box .user-link{
  background-color: transparent;
  margin: initial;
  text-decoration: none;
  cursor: pointer;
  color: #909090;
  flex: 0 0 auto;
}
.sub-comment-list .content-box{
  flex: 1 1 auto;
  margin-left: 12px;
}
.content-box .content-wrapper{
  position: relative;
}

.content-wrapper .user-box{
  display: flex;
  align-items: center;
  position: relative;
}
.user-box .rely-box{
  display: flex;
  align-items: center;
}
.rely-box span{
  padding: 0 12px;
  font-size: 14px;
  line-height: 22px;
  color: #8a919f;
}
.rely-box .user-popover a{
  font-weight: 500;
  font-size: 15px;
  color: #252933;
  line-height: 26px;
  display: block;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
  text-decoration: none;
  cursor: pointer;
}
.content-wrapper .content{
  font-weight: 400;
  font-size: 14px;
  line-height: 2rem;
  color: #515767;
  margin-top: 8px;
  -webkit-line-clamp: 6;
  display: -webkit-box;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-box-orient: vertical;
}

.content-wrapper .action-box{
  display: flex;
  align-items: center;
  margin-top: 8px;
  user-select: none;
}

.content-box .comment-form{
  margin-top: 12px;
  background: rgba(247,248,250,.7);
  padding-left: 52px;
}
.comment-form .content .input-box{
  font-size: 0;
  transition: all .3s;
  border-radius: 4px;
  border-color: #1e80ff;
  background: #fff;

}
.input-box .rich-comment-input{
  position: relative;
  padding: 8px 12px;
  color: #252933;
  outline: none;
  min-height: 64px;
  box-sizing: border-box;
  line-height: 22px;
  font-size: 14px;
  resize: both;
}
.comment-form .action-box{
  display: flex;
  align-items: center;
  margin-top: 8px;
  margin-bottom: 0;
}

</style>
