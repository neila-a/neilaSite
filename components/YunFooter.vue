<script lang="ts" setup>
import { capitalize, computed } from 'vue'
import { useConfig } from 'valaxy'
import { useI18n } from 'vue-i18n'
import pkg from 'valaxy/package.json'
import { useThemeConfig } from 'valaxy-theme-yun/composables'

const { t } = useI18n()
const config = useConfig()
const themeConfig = useThemeConfig()
const year = new Date().getFullYear()

const isThisYear = computed(() => {
    return year === themeConfig.value.footer.since
})

const poweredHtml = computed(() => t('footer.powered', [`<a href="${pkg.repository}" target="_blank" rel="noopener">Valaxy</a> v${pkg.version}`]))
const footerIcon = computed(() => themeConfig.value.footer.icon || {
    url: pkg.repository,
    name: 'i-ri-cloud-line',
    title: pkg.name,
})
</script>

<template>
    <footer class="va-footer p-4 text-$va-c-text-light" text="center sm">
        <a href="https://icp.gov.moe/?keyword=20222036" target="_blank" rel="noopener">
            萌ICP备20222036号
        </a>

        <div class="copyright flex justify-center items-center" p="1">
            <span>
                &copy;
                <template v-if="!isThisYear">
                    {{ themeConfig.footer.since }} -
                </template>
                {{ year }}
            </span>

            <a v-if="themeConfig.footer.icon" m="x-2" class="inline-flex animate-pulse" :href="footerIcon.url"
                target="_blank" :title="footerIcon.title">
                <div :class="footerIcon.name" />
            </a>
            <span>{{ config.author.name }}</span>
        </div>

        <div v-if="themeConfig.footer.powered" class="powered" m="2">
            <span v-html="poweredHtml" /> | <span>{{ t('footer.theme') }} - <a :href="themeConfig.pkg.homepage"
                    :title="themeConfig.pkg.name" target="_blank">{{ capitalize(config.theme) }}</a> v{{
                            themeConfig.pkg.version
                    }}</span>
        </div>

        <slot />
    </footer>
</template>
