
<template>
  <div>
    <v-container fluid>
      <v-row>
        <v-divider></v-divider>
        <v-col cols="12">
          <v-card class="ma-3">
            <v-card-title>
              Courses Selected: {{selected.length}}
              <v-spacer></v-spacer>
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Search"
                single-line
                hide-details
              ></v-text-field>
            </v-card-title>
            <v-data-table
              v-model="selected"
              :headers="headers"
              :items="courses"
              :single-select="singleSelect"
              item-key="name"
              show-select
              class="elevation-1"
              :search="search"
            >
              <template v-slot:top></template>
            </v-data-table>
          </v-card>
        </v-col>
        <v-divider></v-divider>
        <v-col cols="12">
          <v-row>
            <v-card class="ma-3 pa-6" outlined v-for="(item, i) in requirements" :key="i" tile>
              <Major :selected="selectedList" :requirement="item"></Major>
            </v-card>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import Major from "./Major";

export default {
  data() {
    return {
      allCourses: [],
      allMajors: [],

      singleSelect: false,
      selected: [],
      headers: [],
      courses: [],
      search: "",

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
        },
        {
          name: "Funds Management",
          core: [
            "Security Analysis and Portfolio Management",
            "Analysis and Management of Fixed Income Securities",
            "Value Investing",
            "Sustainable Finance"
          ],
          optional: [
            "Risk Modelling and Financial Trading Strategies",
            "Options & Futures Markets",
            "Machine Learning and Financial Innovation",
            "Financial Statement Analysis",
            "Financial Institutions and Capital Markets"
          ],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Global Management",
          core: [
            "International Strategy",
            "International Entrepreneurship",
            "Strategy in Emerging Markets",
            "Supply Chain Management",
            "Economic Environment of International Business",
            "Global Practicum (including Global Innovation, Global Classroom, Design-Led Innovation)",
            "International Financial Management"
          ],
          optional: [
            "Global Practicum (including Global Innovation, Global Classroom, Design-Led Innovation)",
            "Global Consulting Projects",
            "Doing Business Internationally",
            "International Exchange Program"
          ],
          coreRequired: 3,
          optionalRequired: 1,
          totalRequired: 3
        },
        {
          name: "Health Sector Management",
          core: [
            "Health Sector Strategy and Organization",
            "Pharmaceutical Strategy",
            "Health Systems Consulting",
            "Healthcare Innovation"
          ],
          optional: [],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Innovation and Entrepreneurship",
          core: [
            "Entrepreneurship",
            "International Entrepreneurship",
            "Creative Destruction Lab Advanced course",
            "Sustainability Strategy",
            "Strategy in Emerging Markets",
            "Technology Strategy",
            "Venture Capital Strategy",
            "Strategic Networks",
            "Social Entrepreneurship",
            "Healthcare Innovation",
            "Private Equity and Entrepreneurial Finance",
            "Digital Marketing",
            "Business Design Practicum",
            "Creativity and Business Innovation",
            "Silicon Valley Innovation & Entrepreneurship Study Tour",
            "Business Problem Solving – An Integrated Approach"
          ],
          optional: [],
          coreRequired: 4,
          optionalRequired: 0,
          totalRequired: 4
        },
        {
          name: "Investment Banking",
          core: [
            "Corporate Financing",
            "Financial Management",
            "Mergers and Acquisitions",
            "Private Equity and Entrepreneurial Finance"
          ],
          optional: [],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Leadership and Change Management",
          core: [
            "Strategic Change and Implementation",
            "Strategic Networks",
            "Organizational Design",
            "Managerial Negotiations (pre-requisite for Advanced Negotiations & Conflict Management)",
            "Advanced Negotiations and Conflict Management",
            "Aligning People and Strategy",
            "Managing Talent for Global Operations",
            "Inclusive Global Development: Economic Prosperity through Human Development",
            "Leading Social Innovation",
            "The Socially Intelligent Manager",
            "Power and Influence in Organizations",
            "Leading Teams",
            "Effective Leadership",
            "Business Problem Solving: A Model-Based Approach",
            "Top Manager’s Perspective",
            "OnBoard Fellowship",
            "C-Suite: Living Out Leadership Day to Day for Organizational Impact"
          ],
          optional: [],
          coreRequired: 5,
          optionalRequired: 0,
          totalRequired: 5
        },
        {
          name: "Management Analytics",
          core: [
            "Modelling and Optimization for Decision Making",
            "Management Analytics",
            "Machine Learning and Financial Innovation",
            "Forecasting Models & Econometric Methods",
            "Any Fall term course in the MMA program"
          ],
          optional: [
            "Game Theory and Applications to Management",
            "Real Estate Investment",
            "Financial Statement Analysis",
            "Business Analysis and Valuation",
            "Risk Modelling and Financial Trading Strategies",
            "Supply Chain Management",
            "Marketing Research",
            "Fintech Marketing ",
            "Pricing",
            "One to One Marketing",
            "Socially Intelligent Manager elective",
            "Introduction to Data Science and Analytics",
            "Most courses in the MMA program"
          ],
          coreRequired: 2,
          optionalRequired: 1,
          totalRequired: 4
        },
        {
          name: "Process and Supply Chain Management",
          core: [
            "Supply Chain Management",
            "Operations Management Strategy",
            "Services Operations Management",
            "Modeling and Optimization for Decision Making",
            "Quality Management with Lean Six Sigma",
            "Management Analytics",
            "Game Theory and Applications to Management"
          ],
          optional: [],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Real Estate",
          core: [
            "Real Estate Development",
            "Real Estate Economics",
            "Real Estate Investment"
          ],
          optional: [],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Risk Management and Financial Engineering",
          core: [
            "Risk Modelling & Financial Trading Strategies",
            "Options and Futures Markets",
            "Advanced Derivatives",
            "Financial Risk Management",
            "Machine Learning and Financial Innovation"
          ],
          optional: [],
          coreRequired: 3,
          optionalRequired: 0,
          totalRequired: 3
        },
        {
          name: "Social Impact and Sustainability",
          core: [
            "Sustainability Strategy",
            "Sustainable Finance",
            "Leading Social Innovation",
            "Social Entrepreneurship",
            "Clean Energy: Policy Context and Business Opportunities",
            "Social Value and Impact Investing",
            "Designing for Equality",
            "Catastrophic Failure in Organizations"
          ],
          optional: [
            "Marketing and Behavioural Economics",
            "Business Design Practicum",
            "Top Manager’s Perspective",
            "Environmental Finance and Sustainable Investing"
          ],
          coreRequired: 2,
          optionalRequired: 1,
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
      var l = [];
      for (var c of this.selected) {
        l.push(c.name);
      }
      return l;
    }
  },
  methods: {
    genTable: function() {
      var m, i, c, req;
      // gen header
      this.headers.push({ text: "Courses", value: "name" });
      i = 0;
      for (m of this.allMajors) {
        this.headers.push({ text: m, value: "m" + i });
        i++;
      }
      for (c of this.allCourses) {
        var courseDef = {
          name: c
        };
        i = 0;
        for (m of this.allMajors) {
          req = this.requirements.find(element => element.name == m);
          if (req.core.includes(c)) {
            courseDef["m" + i] = "R";
          } else if (req.optional.includes(c)) {
            courseDef["m" + i] = "O";
          }
          i++;
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
      this.genTable();
    }
  },
  beforeMount() {
    this.genData();
  }
};
</script>