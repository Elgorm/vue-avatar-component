<template>
  <div class="avatar" v-bind:style="styler">
    <table><tr><td>{{initials}}</td></tr></table>
  </div>
</template>

<script>
  export default {
    name: 'avatar',
    props: {
      fullname: { type: String, default: '##' },
      size: { type: Number, default: 48 }
    },
    computed: {
      initials () {
        var words = this.fullname.split(/[\s-]+/)
        var intls = ''
        for (var i = 0; i < words.length; i++) {
          intls += words[i].substr(0, 1).toUpperCase()
        }
        if (intls.length > 3) {
          intls = intls.substr(0, 3)
        }
        return intls
      },
      styler () {
        var fontSize = this.initials.length > 2 ? this.size / 3 : this.size / 2
        return {
          'width': this.size + 'px',
          'height': this.size + 'px',
          'font-size': fontSize + 'px',
          'background-color': this.toColor(this.fullname)
        }
      }
    },
    methods: {
      toColor (str) {
        var hash = 0
        var len = str.length
        if (len === 0) return hash
        for (var i = 0; i < len; i++) {
          hash = ((hash << 8) - hash) + str.charCodeAt(i)
          hash |= 0
        }
        hash = Math.abs(hash)
        return '#' + hash.toString(16).substr(0, 6)
      }
    }
  }
</script>

<style scoped>
  .avatar {
    display: inline-block;
    background-color: black;
    color: white;
    width: 48px;
    height: 48px;
    font-size: 12px;
    border-radius: 50%;
  }
  .avatar table {
    width: 100%;
    height: 100%;
    text-align: center;
    vertical-align: middle;
  }
</style>
