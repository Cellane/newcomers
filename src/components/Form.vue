<template>
  <v-container>
    <v-row>
      <v-col cols="6">
        <v-row>
          <v-menu
            v-model="joinDateCalendarVisible"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="joinDate"
                label="入社日"
                prepend-icon="mdi-calendar"
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="joinDate"
              locale="ja"
              @input="joinDateCalendarVisible = false"
            ></v-date-picker>
          </v-menu>
        </v-row>
        <v-row>
          <v-select
            v-model="department"
            :items="departments"
            prepend-icon="mdi-briefcase"
            label="部門"
          >
          </v-select>
        </v-row>
        <v-row>
          <v-text-field
            v-model="nameKanji"
            label="氏名（漢字）"
            prepend-icon="mdi-account"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-text-field
            v-model="nameKana"
            label="氏名（カナ）"
            prepend-icon="mdi-account"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-text-field
            v-model="position"
            label="職種"
            prepend-icon="mdi-account"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-text-field
            v-model="seat"
            label="席"
            prepend-icon="mdi-seat"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-textarea
            v-model="career"
            label="① いままでのご経歴を簡略に"
            prepend-icon="mdi-worker"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="duties"
            label="② モンラボでの担当業務（職種）"
            prepend-icon="mdi-calendar-check"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="birthplace"
            label="③ 出身"
            prepend-icon="mdi-cake-variant"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="hobbies"
            label="④ 趣味"
            prepend-icon="mdi-controller-classic"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="favoriteFood"
            label="⑤ 好きな食べ物"
            prepend-icon="mdi-food-variant"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="wordToEveryone"
            label="⑥ みなさんへ一言"
            prepend-icon="mdi-thought-bubble"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
      </v-col>
      <v-col cols="6">
        <v-row> </v-row>
        <v-row>
          <v-textarea
            v-model="slackText"
            prepend-icon="mdi-slack"
            auto-grow
            no-resize
            readonly
            @focus="$event.target.select()"
          >
            <template v-slot:append>
              <v-btn small color="primary" @click="copy">Copy</v-btn>
            </template>
          </v-textarea>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import moment from "moment"

export default {
  data() {
    return {
      joinDate: moment().format("YYYY-MM-DD"),
      joinDateCalendarVisible: false,
      department: "",
      departments: ["DP事業部", "ゲーム事業部", "音楽事業部", "コーポレート"],
      nameKanji: "",
      nameKana: "",
      position: "",
      seat: "",
      career: "",
      duties: "",
      birthplace: "",
      hobbies: "",
      favoriteFood: "",
      wordToEveryone: ""
    }
  },

  methods: {
    copy() {
      const textarea = document.createElement("textarea")

      textarea.textContent = this.slackText
      textarea.style.position = "fixed"
      document.body.appendChild(textarea)
      textarea.select()

      try {
        return document.execCommand("copy")
      } catch (ex) {
        alert(
          "Sorry, failed to copy the text to clipboard. Please copy the text manually with Cmd-C!"
        )
      } finally {
        document.body.removeChild(textarea)
      }
    }
  },

  computed: {
    joinDayMonth() {
      return moment(this.joinDate)
        .locale("ja")
        .format("MMMDo（dd）")
    },
    /* eslint-disable no-irregular-whitespace */
    slackText() {
      return `----------------------------------------
:confetti_ball:　新入社員のご紹介　:confetti_ball:
----------------------------------------

みなさまお疲れ様です:smiley:
${this.joinDayMonth}入社の方をご紹介いたします:tada::sparkles:
これからよろしくお願いいたします！

:bouquet:　*${this.department}　${this.nameKanji}（${
        this.nameKana
      }）さん*　:bouquet:
職種：${this.position}
＊${this.seat}

*1)いままでのご経歴を簡略に：*
　${this.career.replace(/\n/g, "\n　")}

*2)モンラボでの担当業務（職種）：*
　${this.duties.replace(/\n/g, "\n　")}

*3)出身：*
　${this.birthplace.replace(/\n/g, "\n　")}

*4)趣味：*
　${this.hobbies.replace(/\n/g, "\n　")}

*5)好きな食べ物：*
　${this.favoriteFood.replace(/\n/g, "\n　")}

*6)みなさんへ一言：*
　${this.wordToEveryone.replace(/\n/g, "\n　")}`
    }
    /* eslint-enable no-irregular-whitespace */
  }
}
</script>

<style>
</style>
