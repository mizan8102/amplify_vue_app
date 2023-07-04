<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <v-img height="300" src="@/assets/logo.svg" />

      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

      <h1 class="text-h2 font-weight-bold">AWS Deploy Complete</h1>

      <div class="py-14" />

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <input type="file" @change="handleFileChange" />

        </v-col>

        <v-col cols="auto">
          <v-btn color="primary" href="https://vuetifyjs.com/introduction/why-vuetify/#feature-guides" min-width="228"
            rel="noopener noreferrer" size="x-large" target="_blank" variant="flat">
            <v-icon icon="mdi-speedometer" size="large" start />

            Get Started
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn href="https://community.vuetifyjs.com/" min-width="164" rel="noopener noreferrer" target="_blank"
            variant="text">
            <v-icon icon="mdi-account-group" size="large" start />

            Community
          </v-btn>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script lang="ts" setup>

interface Model {
  KP: number;
  X: number;
  Y: number;
  Z: number;
}

function handleFileChange(event: Event): void {
  const file = (event.target as HTMLInputElement).files?.[0];
  const reader = new FileReader();

  reader.onload = (e: ProgressEvent<FileReader>) => {
    const contents = e.target?.result as string;
    const data = parseCSV(contents);
    console.log(findMinMax(data)); // Process the data as needed
  };

  reader.onerror = (error: ProgressEvent<FileReader>) => {
    console.error(error);
  };

  if (file) {
    reader.readAsText(file);
  }
}

function parseCSV(contents: string): Model[] {
  const lines = contents.split('\n');
  const headers = lines[0].split(',');

  const data: Model[] = [];
  for (let i = 1; i < lines.length; i++) {
    const values = lines[i].split(',');
    if (values.length === headers.length) {
      const entry: Model = {
        KP: parseInt(values[0], 10),
        X: parseFloat(values[1]),
        Y: parseFloat(values[2]),
        Z: parseFloat(values[3]),
      };
      data.push(entry);
    }
  }
  return data;
}

function findMinMax(data: Model[]): { maxX: number, minX: number, maxY: number, minY: number, maxZ: number, minZ: number } {
  let maxX = -Infinity;
  let minX = Infinity;
  let maxY = -Infinity;
  let minY = Infinity;
  let maxZ = -Infinity;
  let minZ = Infinity;

  for (const entry of data) {
    maxX = Math.max(maxX, entry.X);
    minX = Math.min(minX, entry.X);
    maxY = Math.max(maxY, entry.Y);
    minY = Math.min(minY, entry.Y);
    maxZ = Math.max(maxZ, entry.Z);
    minZ = Math.min(minZ, entry.Z);
  }

  return { maxX, minX, maxY, minY, maxZ, minZ };
}


</script>
