<script setup lang="ts">
import type { GroupedLikeNotifications } from '~/types'

const { group } = defineProps<{
  group: GroupedLikeNotifications
}>()

const reblogs = $computed(() => group.likes.filter(i => i.reblog))
const likes = $computed(() => group.likes.filter(i => i.favourite && !i.reblog))
</script>

<template>
  <article flex flex-col relative>
    <StatusLink :status="group.status!" pb2 pt3>
      <div flex flex-col gap-2>
        <div v-if="reblogs.length" flex="~ gap-1">
          <div i-ri:repeat-fill text-xl me-1 color-green />
          <template v-for="i, idx of reblogs" :key="idx">
            <AccountHoverWrapper :account="i.account">
              <NuxtLink :to="getAccountRoute(i.account)">
                <AccountAvatar text-primary font-bold :account="i.account" class="h-1.5em w-1.5em" />
              </NuxtLink>
            </AccountHoverWrapper>
          </template>
          <div ml1>
            {{ $t('notification.reblogged_post') }}
          </div>
        </div>
        <div v-if="likes.length" flex="~ gap-1">
          <div i-ri:heart-fill text-xl me-1 color-red />
          <template v-for="i, idx of likes" :key="idx">
            <AccountHoverWrapper :account="i.account">
              <NuxtLink :to="getAccountRoute(i.account)">
                <AccountAvatar text-primary font-bold :account="i.account" class="h-1.5em w-1.5em" />
              </NuxtLink>
            </AccountHoverWrapper>
          </template>
          <div ml1>
            {{ $t('notification.favourited_post') }}
          </div>
        </div>
      </div>
      <div pl8 mt-1>
        <StatusBody :status="group.status!" text-secondary />
        <!-- When no text content is presented, we show media instead -->
        <template v-if="!group.status!.content">
          <StatusMedia
            v-if="group.status!.mediaAttachments?.length"
            :status="group.status!"
            :is-preview="false"
            pointer-events-none
          />
          <StatusPoll
            v-else-if="group.status!.poll"
            :status="group.status!"
          />
        </template>
      </div>
    </StatusLink>
  </article>
</template>
