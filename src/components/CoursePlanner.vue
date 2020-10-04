
<template>
  <v-data-table
    v-model="selected"
    :headers="headers"
    :items="courses"
    :single-select="singleSelect"
    item-key="name"
    show-select
    class="elevation-1"
  >
    <template v-slot:top>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data() {
    return {
      allCourses: ["course1", "course2"],
      allMajors: ["major1", "major2"],

      singleSelect: false,
      selected: [],
      headers: [
        { text: "course", value: "name"},
        { text: "major1", value: "major1"},
        { text: "major2", value: "major2"},
      ],
      courses: [
        {
          name: "course1",
          major1: "R",
        },
        {
          name: "course2",
          major1: "R",
          major2: "O",
        },
      ]
    };
  }
};
</script>