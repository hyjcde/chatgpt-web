<script setup lang="ts">
import {onMounted, ref} from 'vue'
import { NConfigProvider } from 'naive-ui'
import { NaiveProvider } from '@/components/common'
import ActivationCode from '@/components/common/ActivationCode/index.vue'
import { useTheme } from '@/hooks/useTheme'
import { useLanguage } from '@/hooks/useLanguage'
const hasValidCode = ref(false)
const { theme, themeOverrides } = useTheme()
const { language } = useLanguage()
const checkActivationCodeCookie = () => {
	const cookieString = document.cookie
	const cookies: { [key: string]: string } = cookieString.split('; ').reduce((acc, cookie) => {
		const [key, value] = cookie.split('=')
		acc[key] = value
		return acc
	}, {})

	if (cookies.activationCode && validateActivationCode(cookies.activationCode)) {
		hasValidCode.value = true
	} else {
		hasValidCode.value = false
	}
}


const validateActivationCode = (code: string) => {
	return code === '13121602'
}

onMounted(() => {
	checkActivationCodeCookie()
})

</script>

<template>
  <NConfigProvider
    class="h-full"
    :theme="theme"
    :theme-overrides="themeOverrides"
    :locale="language"
  >
    <NaiveProvider>
				<ActivationCode @validCode="hasValidCode = true" v-if="!hasValidCode"/>
				<RouterView v-else/>
		</NaiveProvider>
  </NConfigProvider>
</template>
