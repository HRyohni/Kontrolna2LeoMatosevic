<template>
  <v-container fill-height fluid class="background">
    <v-row>
      <v-col cols="12">
        <v-card class="pa-3" outlined width="600px">
          <v-card-title></v-card-title>
          <v-card-text>
            <v-form></v-form>
          </v-card-text>
          <v-card-actions class="d-inline">
            <v-form ref="form">
              <v-text-field
                v-model="pod0"
                :counter="10"
                label="Ime"
                :rules="nameRules"
                outlined
              ></v-text-field>
              <v-text-field
                v-model="pod1"
                :rules="nameRules"
                :counter="10"
                label="Prezime"
                required
                outlined
              ></v-text-field>
              <v-text-field
                v-model="pod2"
                :rules="max15"
                :counter="10"
                label="Broj dolazaka (max 15)"
                required
                outlined
              ></v-text-field>
              <v-text-field
                v-model="pod3"
                :rules="max40"
                :counter="10"
                label="Rezultat prvog kolokvija (max 40 bodova)"
                required
                outlined
              ></v-text-field>
              <v-text-field
                v-model="pod4"
                :rules="max40"
                :counter="10"
                label="Rezultat drugog kolokvija (max 40 bodova)"
                required
                outlined
              ></v-text-field>
              <v-text-field
                v-model="pod5"
                :rules="max20"
                :counter="10"
                label="Kontinuirano pracenje (max 20 bodova)"
                required
                outlined
              ></v-text-field>

              <v-row class="d-flex">
                <v-col>
                  <v-btn
                    color="black"
                    class="white--text"
                    elevation="0"
                    @click="obrisiSveUnesenePodatke"
                  >
                    BRISI PODATKE
                  </v-btn>
                </v-col>
                <v-spacer></v-spacer>
                <v-col>
                  <v-btn
                    color="red"
                    class="white--text"
                    outlined
                    elevation="0"
                    @click="cleanUp()"
                  >
                    Očisti
                  </v-btn>
                </v-col>

                <v-col>
                  <v-btn
                    color="green"
                    class="black--text"
                    elevation="0"
                    outlined
					:disabled="isDisable()"
                    @click="dodajStudenta()"
                  >
                    OK
                  </v-btn>
                </v-col>
              </v-row>
              <v-btn type="submit" block class="mt-2">Submit</v-btn>
            </v-form>

            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "prvi-zadatak",
  data() {
    return {
      pod0: "",
      pod1: "",
      pod2: "",
      pod3: "",
      pod4: "",
      pod5: "",

      rules: {
        required: (value) => !!value || "Required.",
        min: (v) => v?.length >= 6 || "Min 6 characters",
        email: (v) =>
          !v ||
          /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
          "E-mail must be valid",
      },

      nameRules: [
        (v) => !!v || "Name is required",
        (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
      ],
      max15: [
        (v) => !!v || "Name is required",
        (v) => (v > 0 && v <= 15) || "must be less than 15 characters",
      ],
      max40: [
        (v) => !!v || "Name is required",
        (v) => (v > 0 && v <= 40) || "must be less than 40 characters",
      ],
      max20: [
        (v) => !!v || "Name is required",
        (v) => (v > 0 && v <= 20) || "must be less than 20 characters",
      ],
    };
  },

  watch: {},
  methods: {
	isDisable() {

		if (this.pod0 == "" && this.pod1 == "" && this.pod2 == "" && this.pod3 == "" && this.pod4 == "" && this.pod5 == "" )
		{return true;}
    	
  },
    cleanUp() {
      this.pod0 = "";
      this.pod1 = "";
      this.pod2 = "";
      this.pod3 = "";
      this.pod4 = "";
      this.pod5 = "";
    },

    async validate() {
      const { valid } = await this.$refs.form.validate();

      if (valid) alert("Form is valid");
    },
    ocistiFormu() {
      this.$refs.form.resetValidation();
    },
    async dodajStudenta() {
     
     
        let noviStudent = {
          ime: this.pod0,
          prezime: this.pod2,
          brojDolazaka: this.pod3,
          prviKolokvij: this.pod4,
          drugiKolokvij: this.pod5,
          kontinuiranoPracenje: this.pod6,
        };
		
        //ovo ne diraj
        let studenti = JSON.parse(localStorage.getItem("studenti"));
        if (!studenti) {
          studenti = [];
        }
        studenti.push(noviStudent);
        localStorage.setItem("studenti", JSON.stringify(studenti));
      
    },
    obrisiSveUnesenePodatke() {
      localStorage.clear();
    },
  },
};
</script>
