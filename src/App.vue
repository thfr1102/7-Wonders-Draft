<template>
  <div class="main-container" @keyup.enter="draftWonders">
    <div>
      <div class="div_check_boxes">
        <div class="armada_check">
          <input
            class="form-check-input"
            type="checkbox"
            id="armada"
            name="armada"
            v-model="armadaCheck"
            @change="checkArmadaExpansion"
          />
          <label class="form-check-label" for="armada">Armada</label>
        </div>
      </div>
      <div class="div_button">
        <div class="btn-group" role="group">
          <button @click="addPlayer" class="btn btn-success" type="button">
            Add Player
          </button>
          <button @click="removePlayer" class="btn btn-danger" type="button">
            Remove Player
          </button>
        </div>
      </div>
      <div
        class="player-name"
        v-for="(playerName, playerIndex) in playerNames"
        :key="playerIndex"
      >
        <label v-text="'Player ' + Number(playerIndex + 1)"></label>
        <input
          type="text"
          class="form-control"
          v-model="playerNames[playerIndex]"
        />
      </div>
    </div>
    <div class="div_button">
      <button
        @click="draftWonders"
        class="btn btn-primary btn-lg"
        type="button"
      >
        Draft Wonders
      </button>
    </div>
    <div class="results">
      <div
        class="player-list"
        v-for="(playerObjects, playerIndex) in draftedObjects"
        :key="playerIndex"
      >
        <h2 class="player-name">{{ playerNames[playerIndex] }}:</h2>
        <ul class="object-list">
          <li v-for="object in playerObjects">
            <span>{{ object }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      objectPool: [
        "Alexandria",
        "Babylon",
        "Ephesus",
        "Giza",
        "Halicarnassus",
        "Olympia",
        "Rhodos",
      ],
      playerNames: ["Fredrik", "Herman", "Emil"],
      draftedObjects: [],
      armadaCheck: false,
      oneEach: false,
    };
  },
  methods: {
    shuffleArray(array) {
      // Fisher-Yates shuffle algorithm
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },
    draftWonders() {
      // Shuffle the object pool
      const indexArray = [...Array(this.objectPool.length).keys()]; // Create an array of indexes [0, 1, 2, ..., 6]
      this.shuffleArray(indexArray); // Shuffle the index array
      // Shuffle players
      this.shuffleArray(this.playerNames);

      // Clear the drafted objects from the previous draft
      this.draftedObjects = [];

      // Draft objects for each player
      for (let i = 0; i < this.playerNames.length; i++) {
        let playerObjects = [];
        playerObjects = [
          this.objectPool[indexArray[i]],
          this.objectPool[indexArray[i + this.playerNames.length]],
        ];
        this.draftedObjects.push(playerObjects);
      }
    },
    addPlayer() {
      if (this.playerNames.length < 7) {
        this.playerNames.push("");
      }
    },
    removePlayer() {
      if (this.playerNames.length > 3) {
        this.playerNames.pop("");
      }
    },
    // Adds Syracuse to the draft if playing Armada
    checkArmadaExpansion() {
      if (this.armadaCheck) {
        this.objectPool.push("Syracuse");
      } else {
        if (this.objectPool.includes("Syracuse")) {
          this.objectPool.splice(this.objectPool.indexOf("Syracuse"), 1);
        }
      }
    },
  },
};
</script>
