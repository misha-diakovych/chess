<template>
  <section class="chess-board">
    <div class="chess-board__left-board">
      <div class="chess-board__user-info">
        <div>
          <img src="../assets/user.png" alt="User" width="25" height="25">
          <span class="chess-board__text">1 Player</span>
          <input type="color" v-model="colorBoardFirstPlayer" />
        </div>
        <BaseTimer :selectedTypeTime="selectedTypeTime" ref="startTime"/>
      </div>
      <div class='chess-board__board-outer'>
        <div class="chess-board__number-container">
          <div class="chess-board__label" v-for="(number, index) in numbers" :key="index">{{number}}</div>
        </div>
        <div class="chess-board__letter-container">
          <div class="chess-board__label" v-for="(letter, index) in letters" :key="index">{{letter}}</div>
        </div>
        <div class='chess-board__boar-inner' id="flip-board">
          <div
            class="chess-board__boar-item"
            v-for="(el, index) in 8"
            :key="index"
            :style="{'flexDirection': index % 2 === 0 ? '' : 'row-reverse'}">
            <div v-for="(el, index) in 8" :key="index" style="height: 50px;">
              <span class="chess-board__square"
                  :style="{'backgroundColor': index % 2 === 0 ? colorBoardFirstPlayer : colorBoardSecondPlayer}"></span>
            </div>
          </div>
        </div>
      </div>
      <div class="chess-board__user-info">
        <div>
          <img src="../assets/user.png" alt="User" width="25" height="25">
          <span class="chess-board__text">2 Player</span>
          <input type="color" v-model="colorBoardSecondPlayer" />
        </div>
        <BaseTimer :selectedTypeTime="selectedTypeTime" ref="startTime"/>
      </div>
    </div>
    <div class="chess-board__right-board">
      <div>
        <p class="chess-board__title-play-chess">Play chess</p>
        <div class="chess-board__button-start-game" @click="flipBoard('flip-board', 'animation')">Flip Board</div>
        <ActionSelect v-model="selectedTypeTime"
                      :options="timeOptions">
        </ActionSelect>
      </div>
      <div class="chess-board__button-start-game" @click="countDownTimer">Start Game</div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ChessBoard',
  data: () => ({
    letters: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'],
    numbers: ['1', '2', '3', '4', '5', '6', '7', '8'],
    colorBoardFirstPlayer: '#ffffff',
    colorBoardSecondPlayer: '#000000',
    selectedTypeTime: '10 minutes',
    timeOptions: ['30 minutes', '10 minutes', '20 second']
  }),
  methods: {
    countDownTimer() {
      this.$refs.startTime.startTimer()
    },
    flipBoard (id, className) {
      const el = document.getElementById(id)
      el.classList.toggle(className)
    }
  },
}
</script>

<style lang="scss">
  .chess-board {
    display: flex;
    align-items: center;
    width: 100%;
    max-width: 900px;
    justify-content: space-between;
    margin: 75px auto 0;
    height: 594px;
    &__number-container{
      width: 500px;
      height: 50px;
      margin-left:24px;
    }
    &__letter-container {
      width: 50px;
      display: inline-block;
      height: 400px;
      margin-top:1px;
    }
    &__boar-inner{
      border: solid 1px black;
      width: 400px;
      height: 400px;
      display: inline-flex;
      flex-wrap: wrap;
      &.animation {
        transition: 1s linear;
        transform: rotateX(180deg)
      }
    }
    &__board-outer {
      height: 452px;
      width: 452px;
      display: flex;
      flex-wrap: wrap;
      margin: auto;
      perspective: 500px;
    }
    &__label {
      width: 50px;
      height: 50px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
    }
    &__boar-item {
      display: flex;
      align-items: center;
    }
    &__user-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-left: 50px;
      margin-top: 25px;
      div {
        display: flex;
        align-items: center;
      }
      img {
        margin-right: 5px;
      }
      input {
        margin-left: 5px;
      }
    }
    &__text {
      color: #000000;
      font-size: 16px;
      font-weight: 600;
    }
    &__right-board {
      display: flex;
      flex-direction: column;
      height: 250px;
      justify-content: space-between;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19);
      padding: 30px;
    }
    &__button-start-game {
      cursor: pointer;
      border-radius: .5rem;
      width: 240px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #000000;
      color: #ffffff;
    }
    &__square {
      width: 50px;
      height: 50px;
      display: inline-block;
      margin: 0;
      animation: fadeIn 1s;
    }
    &__title-play-chess {
      margin: 0;
      padding: 0 0 15px;
      font-size: 20px;
      font-weight: 600;
    }
  }

  @media (max-width: 767px) {
    .chess-board {
      height: 100%;
      max-width: 100%;
      flex-direction: column-reverse;
    }
  }

  @keyframes fadeIn {
    0%{
      transform: rotateY(180deg);
    }
    100% {
      transform: rotateY(0deg);
    }
  }
</style>
