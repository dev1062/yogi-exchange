<template>
    <div class="header">
        <div class="header-left">
            <a
                class="brand"
                href="https://yogi.fi"
                target="_blank"
            >
                <Icon
                    class="logo"
                    :title="'brand'"
                />
                <span class="title">Yogi</span>
            </a>
            <a
                v-if="isDev"
                :href="commitLink"
                target="_blank"
                class="commit-label"
            >
                <div>
                    #{{ commitLabel }}
                </div>
            </a>
            <div class="page-links">
                <div
                    class="link active"
                >
                    Swap
                </div>
                <a
                    class="link"
                    :href="poolsLink"
                    target="_blank"
                >
                    Pools
                </a>
                <a
                    class="link"
                    :href="farmsLink"
                    target="_blank"
                >
                    Farms
                </a>
                <a
                    class="link"
                    href="https://snapshot.org/#/yogi-fi.eth"
                    target="_blank"
                >
                    Vote
                </a>
                <a
                    class="link"
                    href="https://docs.yogi.fi"
                    target="_blank"
                >
                    Docs
                </a>
            </div>
        </div>
        <div class="header-right">
            <Icon
                class="mode-icon"
                :title="modeLogo"
                @click="toggleMode"
            />
            <Account class="account" />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';

import Storage from '@/utils/storage';

import Account from '@/components/Account.vue';
import Icon from '@/components/Icon.vue';

export default defineComponent({
    components: {
        Account,
        Icon,
    },
    setup() {
        
        // eslint-disable-next-line no-undef
        const isDev = ref(process.env.APP_ENV === 'dev');
        // eslint-disable-next-line no-undef
        const commit = ref(APP_COMMIT || '');
        const commitLabel = computed(() => commit.value.substr(0, 6));
        const commitLink = computed(() => 
            `https://github.com/yogi-fi/yogi-exchange/commit/${commit.value}`,
        );

        const mode = ref(Storage.isDarkmode());
        const modeLogo = computed(() => getLogo(mode.value));

        const poolsLink = process.env.APP_POOLS || "https://pools.yogi.fi";
        const farmsLink = process.env.APP_FARMS || "https://farms.yogi.fi";

        function toggleMode(): void {
            mode.value = Storage.toggleMode();
            if (mode.value) {
                document.documentElement.setAttribute('data-theme', 'dark');
            } else {
                document.documentElement.removeAttribute('data-theme');
            }
        }

        function getLogo(isDarkmode: boolean): string {
            return isDarkmode ? 'moon' : 'sun';
        }

        return {
            isDev,
            commitLabel,
            commitLink,
            poolsLink,
            farmsLink,
            modeLogo,
            toggleMode,
        };
    },
});
</script>

<style scoped>
.header {
    height: 80px;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: var(--background-primary);
}

.header-left {
    display: flex;
    align-items: flex-end;
}

.header-right {
    display: flex;
    align-items: center;
}

a {
    text-decoration: none;
    color: var(--text-primary);
}

.brand {
    margin-left: 20px;
    display: flex;
    align-items: flex-end;
}

.logo {
    height: 22px;
    width: 27px;
}

.title {
    margin-left: 12px;
    font-size: var(--font-size-large);
}

.commit-label {
    margin-left: 8px;
    font-size: var(--font-size-small);
    color: var(--text-secondary);
}

.page-links {
    display: flex;
    align-items: flex-end;
    margin-left: 48px;
}

.link {
    margin-right: 20px;
    color: var(--text-secondary);
    cursor: pointer;
}

.link.active {
    color: var(--text-primary);
}

.mode-icon {
    height: 24px;
    width: 24px;
    cursor: pointer;
}

.account {
    margin: 0 16px;
}

@media only screen and (max-width: 768px) {
    .brand {
        margin-left: 16px;
    }

    .title,
    .commit-label,
    .link {
        display: none;
    }
}
</style>
