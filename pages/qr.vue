<template>
  <div class="app">
    <div class="section-intro">
      <div class="container">
        <div class="title en">Until next DODO</div>
        <div class="counter-container">
          <div class="counter counter-null en">Welcome</div>
        </div>
        <div class="hello en" v-if="email">안녕하세요!<br/><span class="highlight">{{ email }}님</span><br />환영합니다.</div>
        <div class="description">Do what you want to Do! 두두는 미루고 미루던 개인 프로젝트를 끝내기 위한 12시간 해커톤입니다. 두두는 디자이너와 개발자가 해야지 해야지 하다가 시작도 못한 개인 프로젝트를 끝내기 위해 밤을 샜던 3월의 어느 날 시작되었습니다. 개인 블로그, 사이드 프로젝트, 외주 작업, 스터디 등 바쁜 일상에 치여 미루던 일들 누구나 하나쯤은 있잖아요? 구체적이고 완벽한 결과물, 끝내야겠다는 강력한 의지는 필요없습니다. 한 달에 열두시간, 미루던 일 하나 끝내는 해커톤 두두와 함께해요-!</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({app: {$axios}}) {
    let adminAuth = false;

    try {
      const response = await $axios.get("/api/auth/user");
      adminAuth = response.data && response.data.success;
    } catch (e) {}

    return {
      adminAuth
    }
  },
  data() {
    return {
      email: '',
      code: ''
    }
  },
  async mounted() {
    this.email = this.$route.query.email || localStorage.getItem('email') || '';
    this.code = this.$route.query.code || '';

    localStorage.removeItem('email');

    if (this.email) {
      try {
        const response = await this.$axios.post("/api/qr/check", {
          email: this.email,
          code: this.code
        })

        if (response.data && response.data.success) {
          alert(`DODO에 오신것을 환영합니다!\n출석체크가 완료되었습니다.`);
          return 1;
        }
        throw new Error();
      } catch (e) {
        if (e.response) {
          if (e.response.status === 403) {
            alert(`12월 두두를 기대해주세요!`);
            return 0;
          } else if (e.response.status === 302 && e.response.data.url !== '') {
            localStorage.setItem('email', this.email);
            location.replace(e.response.data.url);
            return -1;
          } else if (e.response.status === 500) {
            console.log(e.response.data.message);
          }
        }
        alert(`출석체크에 실패했습니다 ㅠㅠ 다시 한 번 바코드를 찍어주세요.`);
        return -1;
      }
    }
  }
}
</script>
<style lang="scss" scoped>
@import "~assets/scss/mixins";

$container_width: 712px;
$gutter: 40px;

$white: #ffffff;
$blue: #7151f4;
$cyan: #a4d6ef;

.section-intro {
  background: $blue;
  background-repeat: repeat-y;
  background-size: calc(100% - 80px);
  background-position: center center;
  background-image: url(~static/background.png);

  position: fixed;
  left: 0;
  width: 100%;
  top: 0;
  bottom: 0;

  @include media-2x() {
    background-image: url(~static/background@2x.png);    
  }
  @include media-3x() {
    background-image: url(~static/background@3x.png);    
  }

  .container {
    padding: 57px 0;
    margin: auto;
    width: $container_width / 2 - $gutter;
  }

  .title {
    font-size: 1.5rem;
    margin-bottom: .8125rem;
    text-align: center;
    color: $white;
  }

  .counter-container {
    text-align: center;
    margin-bottom: 1.25rem;
  }

  .counter-null {
    text-align: center;
    font-size: 70px;
    line-height: 1.1;
    color: $white;
    @include media-md() {
      font-size: 80px;
    }
  }

  .hello {
    font-size: 1.3rem;
    margin-bottom: .75rem;
    letter-spacing: .03125rem;
    text-align: center;
    color: $white;
    .highlight {
      color: $cyan;
    }
  }

  .description {
    display: block;
    font-size: .9rem;
    line-height: 1.6;
    color: $white;
    margin-bottom: 1.25rem;
    word-break: keep-all;
  }
}


</style>
