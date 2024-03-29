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
            v-model="seat"
            label="席（任意）"
            prepend-icon="mdi-seat"
          ></v-text-field>
        </v-row>
        <v-row>
          <v-textarea
            v-model="career"
            label="① いままでのご経歴を簡略に"
            prepend-icon="mdi-account-hard-hat"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="duties"
            label="② 担当業務（職種）"
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
            v-model="star"
            label="⑥ あなたのスターは？"
            prepend-icon="mdi-star"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-textarea
            v-model="wordToEveryone"
            label="⑦ みなさんへ一言"
            prepend-icon="mdi-thought-bubble"
            auto-grow
            rows="1"
          ></v-textarea>
        </v-row>
        <v-row>
          <v-text-field
            v-model="kaonaviUrl"
            label="カオナビ自己紹介（任意）"
            prepend-icon="mdi-account-circle-outline"
          ></v-text-field>
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
              <v-btn small color="primary" @click="copy">コピー</v-btn>
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
      departments: [
        "DC事業部",
        "音楽事業部",
        "コーポレート企画本部",
        "コーポレートコミュニケーション室",
        "ブランディング室",
        "ファイナンス本部",
        "CEO室",
        "A.C.O. / Board部",
        "A.C.O. / Design部",
        "A.C.O. / Office Management部",
        "A.C.O. / UX/IA部",
        "A.C.O. / Project Direction部"
      ],
      nameKanji: "",
      nameKana: "",
      seat: "",
      career: "",
      duties: "",
      birthplace: "",
      hobbies: "",
      favoriteFood: "",
      star: "",
      wordToEveryone: "",
      kaonaviUrl: ""
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

    aco() {
      return this.department.startsWith("A.C.O.")
    },

    /* eslint-disable no-irregular-whitespace */
    headerText() {
      if (this.aco) {
        return `みなさまお疲れ様です:smiley:\n${this.joinDayMonth}入社しました:tada::sparkles:\nこれからよろしくお願いいたします！`
      }

      return `みなさまお疲れ様です:smiley:\n${this.joinDayMonth}入社の方をご紹介いたします:tada::sparkles:\nこれからよろしくお願いいたします！`
    },

    nameText() {
      if (this.aco) {
        return `:bouquet:　*${this.department}　${this.nameKanji}（${this.nameKana}）*　:bouquet:`
      }

      return `:bouquet:　*${this.department}　${this.nameKanji}（${this.nameKana}）さん*　:bouquet:`
    },

    dutiesQuestionText() {
      if (this.aco) {
        return "A.C.O.での担当業務（職種） ／ Duties at A.C.O.:"
      }

      return "モンラボでの担当業務（職種） ／ Duties at Monstar Lab:"
    },

    slackText() {
      return `
----------------------------------------
:confetti_ball:　新入社員のご紹介　:confetti_ball:
----------------------------------------

${this.headerText}

${this.nameText}
${this.seat.length > 0 ? "＊" + this.seat + "\n" : ""}
*1)いままでのご経歴を簡略に ／ Your career until now:*
　${this.career.replace(/\n/g, "\n　")}

*2)${this.dutiesQuestionText}*
　${this.duties.replace(/\n/g, "\n　")}

*3)出身 ／ Birth place:*
　${this.birthplace.replace(/\n/g, "\n　")}

*4)趣味 ／ Hobbies:*
　${this.hobbies.replace(/\n/g, "\n　")}

*5)好きな食べ物 ／ Favourite food:*
　${this.favoriteFood.replace(/\n/g, "\n　")}

*6)あなたのスターは？ ／ Who is your Monstar? (= who is your “my star”):*
  ${this.star.replace(/\n/g, "\n　")}

*7)みなさんへ一言 ／ A word to everyone:*
　${this.wordToEveryone.replace(/\n/g, "\n　")}${
        this.kaonaviUrl.length > 0
          ? "\n\nーーーー\n▼カオナビ 自己紹介（更新中）\n" + this.kaonaviUrl
          : ""
      }`
    }
    /* eslint-enable no-irregular-whitespace */
  }
}
</script>

<style></style>
