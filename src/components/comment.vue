<template>
  <div class="container">
<!--    Vue 2.2.0+的版本里，当在组件中使用v-for时，key是必须的-->
    <div class="comment" v-for="item in comments" :key="item.kf">
      <div class="info">
        <img class="avatar" :src="item.fromAvatar" width="36" height="36"/>
        <div class="right">
          <div class="name">{{item.fromName}}</div>
          <div class="date">{{item.date}}</div>
        </div>
      </div>
      <div class="content">{{item.content}}</div>
      <div class="control">
        <span class="like" :class="{active: item.isLike}" @click="likeClick(item)">
          <i class="iconfont icon-like"></i>
          <span class="like-num">{{item.likeNum > 0 ? item.likeNum + '人赞' : '赞'}}</span>
        </span>
        <span class="comment-reply" @click="showCommentInput(item)">
          <i class="iconfont icon-comment"></i>
          <span>回复</span>
        </span>
      </div>
      <div class="reply">
        <div class="item" v-for="reply in item.reply" :key="reply.kf">
          <div class="reply-content">
            <span class="from-name">{{reply.fromName}}</span><span>: </span>
            <span class="to-name">@{{reply.toName}}</span>
            <span>{{reply.content}}</span>
          </div>
          <div class="reply-bottom">
            <span>{{reply.date}}</span>
            <span class="reply-text" @click="showCommentInput(item, reply)">
              <i class="iconfont icon-comment"></i>
              <span>回复</span>
            </span>
          </div>
        </div>
        <div class="write-reply" v-if="item.reply.length > 0" @click="showCommentInput(item)">
          <i class="el-icon-edit"></i>
          <span class="add-comment">添加新评论</span>
        </div>
        <transition name="fade">
          <div class="input-wrapper" v-if="showItemId === item.id">
            <el-input class="gray-bg-input"
                      v-model="inputComment"
                      type="textarea"
                      :rows="3"
                      autofocus
                      placeholder="写下你的评论">
            </el-input>
            <div class="btn-control">
              <span class="cancel" @click="cancel">取消</span>
              <el-button class="btn" type="success" round @click="commitComment">确定</el-button>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>

import Vue from 'vue'

export default {
  name: "CommentNew",
  props: {
    comments: {
      type: Array,
      required: true
    }
  },
  components: {},
  data() {
    return {
      inputComment: '',
      showItemId: ''
    }
  },
  computed: {},
  methods: {
    /**
     * 点赞
     */
    likeClick(item) {
      if (item.isLike === null) {
        Vue.$set(item, "isLike", true);
        item.likeNum++
      } else {
        if (item.isLike) {
          item.likeNum--
        } else {
          item.likeNum++
        }
        item.isLike = !item.isLike;
      }
    },

    /**
     * 点击取消按钮
     */
    cancel() {
      this.showItemId = ''
    },

    /**
     * 提交评论
     */
    commitComment() {
      console.log(this.inputComment);
    },

    /**
     * 点击评论按钮显示输入框
     * item: 当前大评论
     * reply: 当前回复的评论
     */
    showCommentInput(item, reply) {
      if (reply) {
        this.inputComment = "@" + reply.fromName + " "
      } else {
        this.inputComment = ''
      }
      this.showItemId = item.id
    }
  },
  created() {
    console.log(this.comments)
  }
}
</script>

<style scoped lang="css">

.container{
  padding: 0 10px;
  box-sizing: border-box;
}
.container .comment{
  display: flex;
  flex-direction: column;
  padding: 10px;
  border-bottom: 1px solid #F2F6FC;
}
.comment .info{
  display: flex;
  align-items: center;
}
.comment .info .avatar {
  border-radius: 50%;
}
.comment .info .right{
  display: flex;
  flex-direction: column;
  margin-left: 10px;
}
.right .name {
  font-size: 16px;
  color: #303133;
  margin-bottom: 5px;
  font-weight: 500;
}
.right .date {
  font-size: 12px;
  color: #303133;
}
.container .content {
  font-size: 16px;
  color: #303133;
  line-height: 20px;
  padding: 10px 0;
}
.container .control{
  display: flex;
  align-items: center;
  font-size: 14px;
  color: #909399;
}
.control .like {
  display: flex;
  align-items: center;
  margin-right: 20px;
  cursor: pointer;
}
.like.active,.like:hover {
  color: #409EFF;
}
.like .iconfont {
  font-size: 14px;
  margin-right: 5px;
}
.control .comment-reply {
  display: flex;
  align-items: center;
  cursor: pointer;
}
.comment-reply:hover {
   color: #333;
 }
.comment-reply .iconfont {
  font-size: 16px;
  margin-right: 5px;
}
.container .reply {
  margin: 10px 0;
  border-left: 2px solid #DCDFE6;
}
.reply .item {
  margin: 0 10px;
  padding: 10px 0;
  border-bottom: 1px dashed #EBEEF5;
}
.item .reply-content {
  display: flex;
  align-items: center;
  font-size: 14px;
  color: #303133;
}
.reply-content .from-name {
  color: #409EFF;
}
.reply-content .to-name {
  color: #409EFF;
  margin-left: 5px;
  margin-right: 5px;
}
.item .reply-bottom {
  display: flex;
  align-items: center;
  margin-top: 6px;
  font-size: 12px;
  color: #909399;
}
.reply-bottom .reply-text{
  display: flex;
  align-items: center;
  margin-left: 10px;
  cursor: pointer;
}
.reply-text:hover {
   color: #333;
 }
.reply-text .icon-comment {
  margin-right: 5px;
}
.reply .write-reply {
  display: flex;
  align-items: center;
  font-size: 14px;
  color: #909399;
  padding: 10px;
  cursor: pointer;
}
.write-reply:hover {
   color: #303133;
 }
.write-reply .el-icon-edit {
  margin-right: 5px;
}
.reply .fade-enter-active, fade-leave-active {
  transition: opacity 0.5s;
}
.reply .fade-enter, .fade-leave-to{
  opacity: 0;
}

.input-wrapper {
  padding: 10px;
}
.gray-bg-input, .el-input__inner {
  background-color: #67C23A;
}
.input-wrapper .btn-control {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-top: 10px;
}
.btn-control .cancel {
  font-size: 16px;
  color: #606266;
  margin-right: 20px;
  cursor: pointer;
}
.cancel:hover {
   color: #333;
 }
.btn-control .confirm {
  font-size: 16px;
}
</style>