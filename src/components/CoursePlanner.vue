
<template>
  <div>
    <v-data-table
      v-model="selected"
      :headers="headers"
      :items="courses"
      :single-select="singleSelect"
      item-key="name"
      show-select
      class="elevation-1"
      disable-pagination
    >
      <template v-slot:top></template>
    </v-data-table>

  <v-flex v-for="(item, i) in requirements" :key="i">
    <Major :selected="selectedList" :requirement="item"></Major>
  </v-flex>

  </div>
</template>

<script>
import Major from "./Major"

export default {
  data() {
    return {
      allCourses: [],
      allMajors: [],

      singleSelect: false,
      selected: [],
      headers: [/*
        { text: "course", value: "name" },
        { text: "major1", value: "major1" },
        { text: "major2", value: "major2" }*/
      ],
      courses: [
        /*{
          name: "course1",
          major1: "R"
        },
        {
          name: "course2",
          major1: "R",
          major2: "O"
        }*/
      ],

      requirements: [
        {
          name: "Brand Management",
          core: [
            "Marketing Strategy",
            "Consumer Behaviour",
            "Strategic Marketing Communication",
            "Marketing Research",
            "Sales Management",
            "Branding",
            "Pricing",
            "One to One Marketing",
            "Digital Marketing",
            "Marketing and Behavioural Economics",
            "Creativity and Business Innovation"
          ],
          optional: ["Corporate Strategy", "Financial Management"],
          coreRequired: 4,
          optionalRequired: 0,
          totalRequired: 4
        },
        {
          name: "Business Design",
          core: [
            "Business Design Fundamentals",
            "Business Design Practicum",
            "Creativity for Business Innovation OR Global Practicum: Design-Led Innovation",
            "Design Research and Data Storytelling OR Service Design: Innovating Service-Based Organizations"
          ],
          optional: [],
          coreRequired: 4,
          optionalRequired: 0,
          totalRequired: 4
        },
        {
          name: "Consulting",
          core: [
            "Management Consulting",
            "Healthcare Consulting",
            "Management Consulting Practicum",
            "Strategic Change and Implementation"
          ],
          optional: [
            "Corporation 360°",
            "Communications Strategy",
            "Financial Statement Analysis",
            "Business Analysis and Valuation",
            "Corporate Strategy",
            "Supply Chain Management",
            "Operations Management Strategy",
            "Analytics and Operations Consulting",
            "Organization Design",
            "Aligning People and Strategy",
            "Catastrophic Failure in Organizations",
            "Strategic Networks",
            "Business Problem Solving: A Model-Based Approach"
          ],
          coreRequired: 1,
          optionalRequired: 2,
          totalRequired: 4
        },
        {
          name: "Financial Reporting and Analysis",
          core: [
            "Taxation and Decision Making",
            "Financial Statement Analysis",
            "Financial Distress and Insolvency",
            "Business Analysis and Valuation"
          ],
          optional: [
            "Corporate Strategy",
            "Corporate Financing",
            "Financial Management",
            "Financial Institutions and Capital Markets"
          ],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        }
      ]
    };
  },
  components: {
    Major
  },
computed: {
selectedList: function() {
  var l = []
  for (var c of this.selected) {
    l.push(c.name)
  }
  return l
}
},
  methods: {
    genTable: function() {
      var m, i, c, req
      // gen header
      this.headers.push({ text: "course", value: "name" });
      i = 0;
      for (m of this.allMajors) {
        this.headers.push({ text: m, value: "m" + i });
        i++;
      }
      for (c of this.allCourses) {
        var courseDef = {
          name: c
        };
        i = 0
        for (m of this.allMajors) {
          req = this.requirements.find((element) => (element.name == m))
          if (req.core.includes(c)) {
            courseDef["m"+i] = "R" 
          } else if (req.optional.includes(c)) {
            courseDef["m"+i] = "O"
          }
          i++
        }
        this.courses.push(courseDef);
      }
    },
    genData: function() {
      var req, c;
      for (req of this.requirements) {
        this.allMajors.push(req.name);
        for (c of req.core) {
          if (!this.allCourses.includes(c)) {
            this.allCourses.push(c);
          }
        }
        for (c of req.optional) {
          if (!this.allCourses.includes(c)) {
            this.allCourses.push(c);
          }
        }
      }
      this.allMajors.sort();
      this.allCourses.sort();
      this.genTable()
    }
  },
  beforeMount() {
    this.genData();
  }
};
</script>