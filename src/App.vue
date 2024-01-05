<script setup lang="ts">
import { Field, Form, ErrorMessage } from 'vee-validate'

/**
 * Simulates an API request
 */
const mockApiRequest = (value: string) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(value === 'test@example.com')
    }, 1000)
  })
}

function onSubmit(values: any) {
  alert(JSON.stringify(values, null, 2))
}

async function validateEmail(value: string) {
  const result = await mockApiRequest(value)

  return result ? true : 'This email is already taken'
}

// See vite.config.ts for details about automatic imports
const route = useRoute()

useHead({
  title: () => route.meta.title || 'Vite + Vue Template',
  meta: [
    {
      property: 'og:title',
      content: () => route.meta.title,
    },
    {
      name: 'twitter:title',
      content: () => route.meta.title,
    },
  ],
})

const VERSION = import.meta.env.VITE_APP_VERSION
const BUILD_DATE = import.meta.env.VITE_APP_BUILD_EPOCH
  ? new Date(Number(import.meta.env.VITE_APP_BUILD_EPOCH))
  : undefined
const thisYear = new Date().getFullYear()
</script>
<template>
  <div class="relative py-8">
    <div
      class="absolute inset-0 bg-[url(/img/grid.svg)] bg-top [mask-image:linear-gradient(180deg,white,rgba(255,255,255,0))]"
    ></div>
    <div
      class="container relative max-w-2xl mx-auto bg-white shadow-xl shadow-slate-700/10 ring-1 ring-gray-900/5"
    >
      <header class="px-4 pt-6 prose-sm md:px-6 md:prose">
        <h1>vee-validate TS issue</h1>
        <p class="pb-4 text-xl leading-relaxed tracking-wide text-gray-700"> Error: </p>
        <p
          >Type '(value: string) =&gt; Promise&lt;true | &quot;This email is already
          taken&quot;&gt;' is not assignable to type 'RuleExpression&lt;unknown&gt;'.ts(2322)</p
        >
      </header>
      <main>
        <Form @submit="onSubmit">
          <label for="email">Email</label>
          <Field id="email" name="email" :rules="validateEmail" type="email" />
          <ErrorMessage name="email" />

          <button type="submit">Submit</button>
        </Form>
      </main>
      <footer class="py-6 text-sm text-center text-gray-700">
        <p>
          Vite-ts-tailwind-starter by
          <a class="underline" href="https://twitter.com/uninen">@Uninen</a> &copy; 2020-{{
            thisYear
          }}.
          <template v-if="BUILD_DATE"> Site built {{ BUILD_DATE.toLocaleDateString() }}. </template>
          <template v-else> Development mode. </template>
        </p>
      </footer>
    </div>
  </div>
</template>
