<template>
  <div class="main-container" @keyup.enter="draftWonders">
    <div>
      <label for="player1">Player 1: </label>
      <input type="text" class="form-control" id="player1" v-model="playerNames[0]"><br>
      <label for="player2">Player 2: </label>
      <input type="text" class="form-control" id="player2" v-model="playerNames[1]"><br>
      <label for="player3">Player 3: </label>
      <input type="text" class="form-control" id="player3" v-model="playerNames[2]"><br>
    </div>
    <div class="div_button">
      <button @click="draftWonders" class="btn btn-primary btn-lg" type="button">Draft Wonders</button>
    </div>
    <div class="results">
      <div class="player-list" v-for="(playerObjects, playerIndex) in draftedObjects" :key="playerIndex">
        <h2 class="player-name">{{ playerNames[playerIndex] }}:</h2>
        <ul class="object-list">
          <li v-for="object in playerObjects"><span>{{ object }}</span></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      objectPool: ["Alexandria", "Babylon", "Ephesus", "Giza", "Halicarnassus", "Olympia", "Rhodos"],
      playerNames: ["Fredrik", "Herman", "Emil"],
      draftedObjects: []
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
      const indexArray = [...Array(7).keys()]; // Create an array of indexes [0, 1, 2, ..., 6]
      this.shuffleArray(indexArray); // Shuffle the index array

      // Clear the drafted objects from the previous draft
      this.draftedObjects = [];

      // Draft two objects for each player
      for (let i = 0; i < 3; i++) {
        const playerObjects = [this.objectPool[indexArray[i * 2]], this.objectPool[indexArray[i * 2 + 1]]];
        this.draftedObjects.push(playerObjects);
      }
    }
  }
};
</script>