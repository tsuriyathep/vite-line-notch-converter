<script setup>
import { ref, computed, watch } from 'vue';

let reelText = ref(`1	1	1	1	1
2	2	2	2	2
0	0	0	0	0
3	3	3	3	3
0	1	2	1	0
3	2	1	2	3
2	1	0	1	2
1	2	3	2	1
0	1	0	1	0
3	2	3	2	3
2	1	2	1	2
1	2	1	2	1
1	1	0	1	1
2	2	3	2	2
0	1	1	1	0
3	2	2	2	3
1	0	0	0	1
2	3	3	3	2
0	0	1	0	0
3	3	2	3	3`);
let reelResult = ref('');
let reelRows = ref(0);
let reelCols = ref(0);
let reelLines = ref(0);

function process() {
  let result = '';

  // Clean input
  reelText.value = reelText.value.trim();

  // Get starting data row and count cols
  let rows = reelText.value.split('\n');
  let cols = 0;
  let maxRow = 0;
  let rowStart = 0;
  for (let i = 0; i < rows.length; i++) {
    let arr = rows[i].trim().split('\t');
    let n = arr.length;
    if (n == 0) break;
    arr.forEach((a) => {
      let notchRow = parseInt(a);
      if (notchRow > maxRow) maxRow = notchRow;
    });
    cols = n;
    rowStart = i;
    result += `"${i}": [`;
    let notchArray = [];
    for (let j = 0; j < arr.length; j++) {
      notchArray.push(`"${j}_${arr[j]}"`);
    }
    result += notchArray.join(',') + `],\n`;
  }

  // Save reel size
  reelRows.value = maxRow + 1;
  reelCols.value = cols;
  reelLines.value = rows.length;

  reelResult.value = result;
}

watch([reelText], (newValue, oldValue) => {
  process();
});

process();
</script>

<template>
  <b>INPUT (from Excel tab separated):</b>
  <textarea v-model="reelText" style="width: 100%" rows="20" />
  <b>OUTPUT ({{ reelCols }}x{{ reelRows }} {{ reelLines }} Lines):</b>
  <textarea v-model="reelResult" style="width: 100%" rows="15" />
</template>

<style scoped></style>
