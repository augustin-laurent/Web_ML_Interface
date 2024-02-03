<template>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Save Model</span>
        </v-card-title>
        <v-card-text>
          <v-text-field v-model="modelName" label="Model Name" />
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="blue darken-1" text @click="dialog = false">Close</v-btn>
          <v-btn color="blue darken-1" text @click="saveModel">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>

<script>
</script>