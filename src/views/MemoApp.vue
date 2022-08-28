<template>
  <h1>Vue メモ</h1>
  <div class="memo-list">
    <ul class="memo-list__container">
      <li class="memo" v-for="(memo, index) in memos" v-bind:key="index">
        <div class="memo__checkbox">
          <input type="checkbox" v-model="memo.isDone" />
        </div>
        <div
          v-if="memo.isDone && !memo.isEdit"
          class="memo__text memo__text--done"
        >
          {{ index }}:{{ memo.text }}
        </div>
        <div v-else-if="!memo.isDone && !memo.isEdit" class="memo__text">
          {{ index }}:{{ memo.text }}
        </div>
        <div v-else>
          <input
            type="text"
            class="input__edit"
            v-model="editedText"
            v-on:keydown.enter="setNewText(index)"
          />
        </div>
        <div class="memo__buttons">
          <button
            class="memo__edit"
            v-if="!memo.isEdit"
            @click="editMemo(index)"
          >
            編集
          </button>
          <button
            class="memo__delete"
            v-if="!memo.isEdit"
            @click="deleteMemo(index)"
          >
            削除
          </button>
          <button class="memo__done" v-else @click="setNewText(index)">
            完了
          </button>
        </div>
      </li>
    </ul>
    <div class="add-memo-field">
      <input
        class="add-memo-field__input"
        type="text"
        v-model="inputText"
        v-on:keydown.enter="addMemo"
        placeholder="新しいメモ"
      />
      <button
        class="add-memo-field__button"
        @click="addMemo"
        v-bind:disabled="isInputEmpty"
      >
        追加
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputText: "",
      memos: [
        {
          text: "ひき肉を300g買う",
          isDone: false,
          isEdit: false,
        },
        {
          text: "ホウレンソウを1束買う",
          isDone: false,
          isEdit: false,
        },
        {
          text: "ピーマンを2個買う",
          isDone: false,
          isEdit: false,
        },
      ],
      editedText: "",
    }
  },
  methods: {
    /**
     * メモを追加する
     */
    addMemo() {
      if (!this.isInputEmpty) {
        this.memos.push({ text: this.inputText, isDone: false, isEdit: false })
        this.inputText = ""
      }
    },
    /**
     * メモを削除する
     */
    deleteMemo(idx) {
      this.memos.splice(idx, 1)
    },
    editMemo(idx) {
      this.memos[idx].isEdit = true
      this.editedText = this.memos[idx].text
    },
    setNewText(idx) {
      if (this.editedText === "") {
        this.deleteMemo(idx)
      } else {
        this.memos[idx].text = this.editedText
        this.memos[idx].isEdit = false
      }
    },
  },
  computed: {
    /**
     * 入力欄が空白ならTrue, それ以外はFalse
     */
    isInputEmpty() {
      return this.inputText === ""
    },
  },
}
</script>

<style scoped>
.memo-list {
  padding-left: 5rem;
  padding-right: 5rem;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  max-width: 512px;
  margin-left: auto;
  margin-right: auto;
}

.memo-list__container {
  padding: 0;
}

.memo {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  border-radius: 5px;
}

.memo:hover {
  color: white;
  background-color: #b23b61;
}

.memo__text {
  margin-left: 2rem;
  text-align: left;
}

.memo__text--done {
  text-decoration-line: line-through;
}

.memo__delete {
  margin-left: 1rem;
  padding: 0.5rem 0.5rem;
  border: solid 1px black;
  border-radius: 5px;
  background-color: white;
}

.memo__delete:hover {
  background-color: #ff0000;
  border-radius: 5px;
}

.memo__edit {
  margin-left: 1rem;
  padding: 0.5rem 0.5rem;
  border: solid 1px black;
  border-radius: 5px;
  background-color: white;
}

.memo__edit:hover {
  background-color: #b2ae3b;
  border-radius: 5px;
}

.input__edit {
  padding: 8px;
}

.memo__done {
  margin-left: 1rem;
  padding: 0.5rem 0.5rem;
  border: solid 1px black;
  border-radius: 5px;
  background-color: white;
}

.memo__done:hover {
  background-color: #00aeef;
  border-radius: 5px;
}

.add-memo-field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.add-memo-field__input {
  padding: 10px;
}
.add-memo-field__button {
  padding: 0.5rem 0.5rem;
  border: solid 1px black;
  border-radius: 5px;
  background-color: white;
}

.add-memo-field__button:hover {
  background-color: #b2ae3b;
  border-radius: 5px;
}
</style>
