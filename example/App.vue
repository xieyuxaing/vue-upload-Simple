<template>
  <uploader :options="options" :file-status-text="statusText" class="uploader-example" ref="uploader"
            @file-complete="fileComplete" @complete="complete"
            @files-added="filesAdd" @file-added="fileAdded"
  ></uploader>
</template>

<script>
  export default {
    data () {
      return {
        options: {
          target: 'localhost:80/api/model/uploadModelFloder?name=test', // '//jsonplaceholder.typicode.com/posts/',
          testChunks: false
        },
        attrs: {
          accept: 'image/*'
        },
        statusText: {
          success: '成功了',
          error: '出错了',
          uploading: '上传中',
          paused: '暂停中',
          waiting: '等待中'
        }
      }
    },
    methods: {
      complete () {
        console.log('complete', arguments)
      },
      fileComplete () {
        console.log('file complete', arguments)
      },
      filesAdd (fileList, newFileList, evt) {
        if (newFileList.length > 0) {
          if (newFileList[0].isFolder) {
            for (var n in fileList) {
              if (fileList[n].size > 200 * 1024 * 1024) {
                fileList[n].ignored = true
              }
            }
          }
        }
      },
      fileAdded (files) {
        if (files.relativePath.split('/').length === 1) {
          if (files.size > 200 * 1024 * 1024) {
            files.ignored = true
          }
        }
      }
    },
    mounted () {
      this.$nextTick(() => {
        window.uploader = this.$refs.uploader.uploader
      })
    }
  }
</script>

<style>
  .uploader-example {
    width: 880px;
    padding: 15px;
    margin: 40px auto 0;
    font-size: 12px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .4);
  }

  .uploader-example .uploader-btn {
    margin-right: 4px;
  }

  .uploader-example .uploader-list {
    max-height: 440px;
    overflow: auto;
    overflow-x: hidden;
    overflow-y: auto;
  }
</style>
