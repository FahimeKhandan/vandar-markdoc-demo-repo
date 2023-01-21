<script  setup>
import Markdoc from "@markdoc/markdoc";
import render from "vue-markdoc";
// import Callout from './Callout.vue'
import a2aDoc from './a2a.md'


// const tags = {
//   callout: {
//     render: Callout,
//     attributes: {},
//   },
// }

// const doc = `
// # سرویس تسویه
// ## لیست تسویه ها
// #### در این بخش شما می‌توانید همه‌ی تسویه‌هایی را که تا به حال ثبت کرده‌اید، مشاهده نمایید.

// {% if true %}
// {% callout %}
// **نکته!**
// پارامتر deductible_amount نشان‌دهنده مبلغ قابل برداشت امروز و پارامتر blocked_amount نشان‌دهنده مبلغ مسدود‌شده در حساب است.
// {% /callout %}

// {% else /%}
// {% callout %}
// **نکته!**
// انواع حالت‌های ممکن تسویه (فیلد status مربوط به settlement) عبارتند از: INIT, PENDING, DONE, FAILED, CANCELED

// - INIT: وقتی که تسویه به بانک فرستاده می‌شود.
// - PENDING: وقتی که تسویه به بانک فرستاده شده و در انتظار رسیدن به سیکل مورد نظر است.
// - DONE: تسویه‌ای که با موفقیت انجام شده است.
// - FAILED: تسویه‌ای که با خطا مواجه شده است.
// - CANCELED: تسویه لغو شده است. لازم به ذکر است که امکان لغو تسویه در وضعیت تسویه‌ی آنی وجود ندارد. 
// {% /callout %}

// {% /if %}



// `

// const ast = Markdoc.parse(doc)
// const content = Markdoc.transform(ast, { tags })

// const ContentComponent = render(content)
import { defineComponent, ref, shallowRef } from 'vue'
  import { Codemirror } from 'vue-codemirror';
  import { markdown } from '@codemirror/lang-markdown';
  import { oneDark } from '@codemirror/theme-one-dark';




  components: {
    a2aDoc,
    Codemirror
  }



  const doc = `
# hello
{% table %}

- name {% width="50%" %}
- Type  {% colspan=2 %}
- Status  {% align="right" %}
---
- amount
- integer
- required
---
- iban
- string
- required
---
- track_id
- string 
- required
---
- payment_number
- integer
- optional
---
- notify_url
- string
- optional
---
- description
- string
- optional
---
- is_instant
- boolean
- optional
---
- type
- string
- optional
{% /table %}

`
      const code = ref(doc)
      const extensions = [markdown(), oneDark]

      // Codemirror EditorView instance ref
      const view = shallowRef()
      const handleReady = (payload) => {
        view.value = payload.view
      }

      // Status is available at all times via Codemirror EditorView
      const getCodemirrorStates = () => {
        const state = view.value.state
        const ranges = state.selection.ranges
        const selected = ranges.reduce((r, range) => r + range.to - range.from, 0)
        const cursor = ranges[0].anchor
        const length = state.doc.length
        const lines = state.doc.lines
        // more state info ...
        // return ...
      }


const increment = () => ++count.value


const ast = Markdoc.parse(code.value)
const content = Markdoc.transform(ast)

const ContentComponent = render(content)

</script>

<template>
  <!-- <ContentComponent /> -->
  <!-- <a2aDoc></a2aDoc> -->
  <codemirror
    v-model="code"
    placeholder="Code goes here..."
    :style="{ height: '400px', direction:ltr }"
    :autofocus="true"
    :indent-with-tab="true"
    :tab-size="2"
    :extensions="extensions"
    @ready="handleReady"
    @change="log('change', $event)"
    @focus="log('focus', $event)"
    @blur="log('blur', $event)"
  />

  <ContentComponent></ContentComponent>
</template>
