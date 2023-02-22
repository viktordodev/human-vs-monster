<template>
  <div class="container">
    <div v-if="gameStatus != 'progress'" class="gamestatus">
      <h2>Game Over</h2>
      <h3 v-if="gameStatus == 'humanwin'">You Won</h3>
      <h3 v-else-if="gameStatus == 'monsterwin'">Monster Won</h3>
      <h3 v-else>It's a Draw!!</h3>
      <hr />
      <button :style="sgame" @click="startOver()">Start New Game</button>
    </div>
    <div v-if="gameStatus == 'progress'">
      <div class="monster_health flex items-center">
        <h2>Monster Health</h2>
        <h3>{{ monsterHealth }}</h3>

        <meter v-bind:value="monsterHealth" min="0" max="100"></meter>
      </div>
      <div class="human_health">
        <h2>Human Health</h2>
        <h3>{{ humanHealth }}</h3>
        <meter v-bind:value="humanHealth" min="0" max="100"></meter>
      </div>

      <div class="container btns-f">
        <button @click="attackMonster()">Attack</button>
        <button
          :disabled="maxSpecial == 0"
          @click="specialAttack()"
          :style="specialStyle"
        >
          Special Attack
        </button>
        <button :style="healStyle" @click="healHuman()">Heal</button>
        <button @click="surrender()">Surrender</button>
      </div>
    </div>
    <div class="b-log">
      <h2>Battle Log</h2>
      <ul>
        <li v-for="(item, index) in msgs" :key="index + 'msglog'">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msgs: [],
      monsterHealth: 100,
      humanHealth: 100,
      maxHeal: 3,
      maxSpecial: 2,
      gameStatus: "progress",
    };
  },
  watch: {
    monsterHealth(value) {
      if (value <= 0 && this.humanHealth <= 0) {
        this.gameStatus = "draw";
      } else if (value <= 0) {
        this.gameStatus = "humanwin";
      }
    },
    humanHealth(value) {
      if (value <= 0 && this.monsterHealth <= 0) {
        this.gameStatus = "draw";
      } else if (value <= 0) {
        this.gameStatus = "monsterwin";
      }
    },
  },
  methods: {
    addLogMsg(who, what, value) {
      let msg = who + what + value;
      this.msgs.push(msg);
    },

    surrender() {
      this.humanHealth = 0;
    },
    startOver() {
      (this.msgs = []), (this.monsterHealth = 100);
      this.humanHealth = 100;
      this.maxHeal = 3;
      this.maxSpecial = 2;
      this.gameStatus = "progress";
    },
    healHuman() {
      if (this.maxHeal > 0) {
        let healValue = this.getRandomFormula(20, 10);
        if (healValue + this.humanHealth >= 100) {
          this.addLogMsg("Human ", "restored ", "full" + " health");
          this.humanHealth = 100;
        } else {
          this.addLogMsg(
            "Human ",
            "Healed himself with ",
            healValue + " points"
          );
          this.humanHealth += healValue;
        }
        this.maxHeal--;
      }
    },
    specialAttack() {
      if (this.maxSpecial > 0) {
        let specValue = this.getRandomFormula(18, 7);
        this.monsterHealth -= specValue;
        this.addLogMsg(
          "Human ",
          "Hit monster with Special Attack and took ",
          specValue + " points from his health"
        );
        this.attackHuman();
        this.maxSpecial--;
      }
    },
    attackMonster() {
      let hitValue = this.getRandomFormula(5, 12);
      this.monsterHealth -= hitValue;
      this.addLogMsg(
        "Human ",
        "Hit monster and took ",
        hitValue + " points from his health"
      );
      this.attackHuman();
    },
    attackHuman() {
      let Hitvalue = this.getRandomFormula(8, 15);
      this.humanHealth -= Hitvalue;
      this.addLogMsg(
        "Monster ",
        "Hit human and took ",
        Hitvalue + " points from his health"
      );
    },

    getRandomFormula(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
  },
  computed: {
    healStyle() {
      if (this.maxHeal > 0) {
        return { background: "peru" };
      } else {
        return {
          background: "red",
          cursor: "no-drop",
        };
      }
    },
    sgame() {
      return {
        background: "darkred",
        padding: "10px 20px",
        fontSize: "1.5rem",
        color: "white",
        border: "none",
        cursor: "pointer",
      };
    },
    specialStyle() {
      if (this.maxSpecial > 0) {
        return { background: "peru" };
      } else {
        return {
          background: "red",
          cursor: "no-drop",
        };
      }
    },
  },
};
</script>
<style lang="scss">
.container {
  padding: 50px 0;
  max-width: 800px;
  margin: auto;
  &.btns-grid {
    display: grid;
    grid-column: 1/2;
  }
}
.b-log {
  margin-top: 3%;
  border: 1px solid black;
  border-radius: 5px;
  padding: 10px;
  h2 {
    text-align: center;
  }
  li {
    font-weight: bold;
  }
}
.gamestatus {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.monster_health,
.human_health {
  margin-bottom: 3%;
  background-color: whitesmoke;
  padding: 20px;
  box-shadow: 0px 0px 14px 5px #0000000f;
  border: 1px solid gray;
  border-radius: 5px;
  h2 {
    text-align: center;
    font-weight: bolder;
    font-size: 2rem;
  }
  h3 {
    text-align: center;
  }
  meter {
    min-height: 50px;
    width: 100%;
  }
}
.btns-f {
  display: flex;
  justify-content: space-between;
  button {
    background: peru;
    padding: 10px 20px;
    border: none;
    font-size: 1.5rem;
    font-weight: bold;
    cursor: pointer;
    &:hover {
      background: rebeccapurple;
      color: white;
    }
  }
}
</style>
