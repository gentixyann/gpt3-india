<template>
  <h1>GPT-3</h1>
  <div>
    <input type="text" v-model="keyword" placeholder="インドが答えます" />
  </div>
  <div>
    <button type="button" @click="handleClick">聞いてみる</button>
  </div>
  <div>
    <h2>これがインドからの答え</h2>
    <div>{{ generateText }}</div>
  </div>
</template>


<script setup>
const keyword = ref("");
const generateText = ref("");

// const prompt = computed(
//   () => `
//   あなたは日本語が得意な面白いインド人です。
//   ${keyword.value}についてインドと関連を持たせて最大100文字で回答してください。
// `
// );

const prompt = computed(
  () => `
# 命令書:
あなたは、インドの文化や思想を熟知した宗教家です。
以下の制約条件に沿って、質問文に対する最高のお告げを出力してください。

# 制約条件:
・文字数は300文字ていど。
・小学生にもわかりやすく。
・重要なキーワードを取り残さない。
・インドの文化や思想に絡めて答える。
・個人ではなくインドの神様として答える。

# 質問文:
 ${keyword.value}
`
);

// const prompt = computed(() => [
//   {
//     role: "user",
//     content: "あなたはインド人ですか？。",
//   },
//   {
//     role: "assistant",
//     content:
//       "はい。私はインドで生まれインド育ちのインド人で、今は日本で住んでいます。",
//   },
//   {
//     role: "user",
//     content: `${keyword.value}の相談に回答してください。`,
//   },
// ]);

const handleClick = async () => {
  console.log("handleClick始まり");
  const { data } = await useFetch("/api/generate", {
    method: "POST",
    body: {
      prompt,
    },
  });
  console.log(data);

  generateText.value = data.value.choices[0].text;
  //   generateText.value = data.data.choices[0].message;
};
</script>