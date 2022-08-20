<template>
<!--  <comment :comments="commentData"></comment>-->
<!--  <comments></comments>-->
<!--  <CommentNew :commentse="commentData"></CommentNew>-->
<!--  <commentsNew :commentse="commentData"></commentsNew>-->
  <NewComment :commentse="commentData"></NewComment>
</template>

<script>

// import comment from "@/components/comment";
// import comments from "@/components/comments";
/*import CommentNew from "@/components/CommentNew";*/
import imageone from "@/assets/images/1.jpg";
import imagetwo from "@/assets/images/2.jpg";
import imagethree from "@/assets/images/3.jpg";
import imagefour from "@/assets/images/4.jpg";
// import commentsNew from "@/components/commentsNew";
// import * as CommentData from '@/mockdata'
import NewComment from "@/components/NewComment";
export default {
  components: {
    // comment,
    // comments
    // CommentNew,
    // commentsNew
    NewComment
  },
 /* data() {
    return {
      commentData: []
    }
  },*/
 /* data() {
    return {
      commentData: [
        {
          kf:'1',
          id: 'comment0001', //主键id
          date: '2018-07-05 08:30',  //评论时间
          ownerId: 'talents100020', //文章的id
          fromId: 'errhefe232213',  //评论者id
          fromName: '犀利的评论家',   //评论者昵称
        /!*  fromAvatar: 'http://ww4.sinaimg.cn/bmiddle/006DLFVFgy1ft0j2pddjuj30v90uvagf.jpg', //评论者头像*!/
          likeNum: 3, //点赞人数
          content: '非常靠谱的程序员',  //评论内容
          reply: [  //回复，或子评论
            {
              kf:'11',
              id: '34523244545',  //主键id
              commentId: 'comment0001',  //父评论id，即父亲的id
              fromId: 'observer223432',  //评论者id
              fromName: '夕阳红',  //评论者昵称
              // fromAvatar: 'https://wx4.sinaimg.cn/mw690/69e273f8gy1ft1541dmb7j215o0qv7wh.jpg', //评论者头像
              toId: 'errhefe232213',  //被评论者id
              toName: '犀利的评论家',  //被评论者昵称
              // toAvatar: 'http://ww4.sinaimg.cn/bmiddle/006DLFVFgy1ft0j2pddjuj30v90uvagf.jpg',  //被评论者头像
              content: '赞同，很靠谱，水平很高',  //评论内容
              date: '2018-07-05 08:35'   //评论时间
            },
            {
              kf:'12',
              id: '34523244545',
              commentId: 'comment0001',
              fromId: 'observer567422',
              fromName: '清晨一缕阳光',
              // fromAvatar: 'http://imgsrc.baidu.com/imgad/pic/item/c2fdfc039245d688fcba1b80aec27d1ed21b245d.jpg',
              toId: 'observer223432',
              toName: '夕阳红',
              // toAvatar: 'https://wx4.sinaimg.cn/mw690/69e273f8gy1ft1541dmb7j215o0qv7wh.jpg',
              content: '大神一个！',
              date: '2018-07-05 08:50'
            }
          ]
        },
        {
          kf:'2',
          id: 'comment0002',
          date: '2018-07-05 08:30',
          ownerId: 'talents100020',
          fromId: 'errhefe232213',
          fromName: '毒蛇郭德纲',
          // fromAvatar: 'http://ww1.sinaimg.cn/bmiddle/006DLFVFgy1ft0j2q2p8pj30v90uzmzz.jpg',
          likeNum: 0,
          content: '从没见过这么优秀的人',
          reply: []
        }
      ]
    }
  },*/
  /*created() {
    this.commentData = CommentData.comment.data;
  },*/
  data() {
    return {
      commentData: {
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
    }
  },
}
</script>