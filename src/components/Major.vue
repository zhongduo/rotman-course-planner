<template>
  <v-card class="mx-auto" max-width="600" outlined>
    <v-list-item>
      <v-list-item-avatar v-if="!completed" color="grey"></v-list-item-avatar>
      <v-list-item-avatar v-if="completed" color="red"></v-list-item-avatar>
      <v-list-item-content>
        <v-list-item-title class="headline" v-text="requirement.name"></v-list-item-title>
      </v-list-item-content>
    </v-list-item>

    <v-list subheader flat>
      <v-subheader>Core</v-subheader>
      <v-list-item-group multiple v-for="(item, i) in requirement.core" :key="i">
        <v-list-item>
          <v-list-item-action>
            <v-checkbox :input-value="selected.includes(item)" color="primary" readonly></v-checkbox>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title v-text="item"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>

    <v-list subheader flat>
      <v-subheader>Optional</v-subheader>
      <v-list-item-group multiple v-for="(item, i) in requirement.optional" :key="i">
        <v-list-item>
          <v-list-item-action>
            <v-checkbox :input-value="selected.includes(item)" color="primary" readonly></v-checkbox>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title v-text="item"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>

    <v-list subheader flat>
      <v-subheader>Summary</v-subheader>
      <v-list-item-group multiple>
        <v-list-item>
          <v-checkbox :input-value="requirement.coreRequired <= numCore" color="primary" readonly></v-checkbox>
          <v-list-item-content>Core: required {{requirement.coreRequired}}, get {{numCore}}</v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-checkbox :input-value="requirement.optionalRequired <= numOption" color="primary" readonly></v-checkbox>
          <v-list-item-content>Optional: required {{requirement.optionalRequired}}, get {{numOption}}</v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-checkbox
            :input-value="requirement.totalRequired <= (numOption + numCore)"
            color="primary"
            readonly
          ></v-checkbox>
          <v-list-item-content>Total: required {{requirement.totalRequired}}, get {{numOption + numCore}}</v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-card>
</template>

<script>
export default {
  props: {
    selected: {
      type: Array,
      required: true
    },
    requirement: {
      type: Object,
      required: true
    }
  },
  computed: {
    completed: function() {
      return (
        this.requirement.coreRequired <= this.numCore &&
        this.requirement.optionalRequired <= this.numOption &&
        this.requirement.totalRequired <= this.numOption + this.numCore
      );
    },
    numCore: function() {
      var n = 0;
      for (var c of this.selected) {
        if (this.requirement.core.includes(c)) {
          n++;
        }
      }
      return n;
    },
    numOption: function() {
      var n = 0;
      for (var c of this.selected) {
        if (this.requirement.optional.includes(c)) {
          n++;
        }
      }
      return n;
    }
  },

  methods: {
    isSelected: function(course) {
      for (var c of this.selected) {
        if (c.name == course) {
          return true;
        }
      }
      return false;
    }
  },
  beforeMount() {
    //console.log(this.requirement);
  }
};
</script>