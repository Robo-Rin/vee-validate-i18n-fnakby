<script setup lang="ts">
import { useField, useForm } from 'vee-validate';
import { toTypedSchema } from '@vee-validate/zod';
import * as zod from 'zod';
import { localize, setLocale } from '@vee-validate/i18n';
import { configure } from 'vee-validate';
import en from '@vee-validate/i18n/dist/locale/en.json';
import de from '@vee-validate/i18n/dist/locale/de.json';

configure({
  generateMessage: localize({
    de,
    en,
  }),
});

setLocale('de');

const validationSchema = toTypedSchema(
  zod.object({
    email: zod.string().min(5),
    password: zod.string().min(8),
  })
);
const { handleSubmit, errors } = useForm({
  validationSchema,
});
const { value: email } = useField('email');
const { value: password } = useField('password');
const onSubmit = handleSubmit((values) => {
  alert(JSON.stringify(values, null, 2));
});

function changeLocale(locale: string) {
  setLocale(locale);
}
</script>

<template>
  <form @submit="onSubmit">
    <input name="email" v-model="email" type="email" />
    <span>{{ errors.email }}</span>
    <input name="password" v-model="password" type="password" />
    <span>{{ errors.password }}</span>
    <button>Submit</button>
  </form>
  <button type="button" @click="changeLocale('de')">DE</button>
  <button type="button" @click="changeLocale('en')">EN</button>
</template>
