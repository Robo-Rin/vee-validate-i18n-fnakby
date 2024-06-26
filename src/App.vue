<script setup lang="ts">
import { toTypedSchema } from "@vee-validate/zod";
import i18next from "i18next";
import { useField, useForm } from "vee-validate";
import { z } from "zod";
import { zodI18nMap } from "zod-i18n-map";
import de from "zod-i18n-map/locales/de/zod.json";
import en from "zod-i18n-map/locales/en/zod.json";

z.setErrorMap(zodI18nMap);
const translations: Record<string, object> = { de, en }; // This is a short syntax for { de: de, en: en }

function changeLocale(locale: string) {
  i18next.init({
    lng: locale,
    resources: {
      //    ↓      The square bracket syntax is used to create a dynamically named property
      [`${locale}`]: { zod: translations[locale] },
    },
  });
}

changeLocale("de");

const validationSchema = toTypedSchema(
  z.object({
    email: z.string().min(5),
    password: z.string().min(8),
  })
);
const { handleSubmit, errors } = useForm({
  validationSchema,
});
const { value: email } = useField("email");
const { value: password } = useField("password");
const onSubmit = handleSubmit((values) => {
  alert(JSON.stringify(values, null, 2));
});
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
