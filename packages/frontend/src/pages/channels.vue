<template>
<MkStickyContainer>
	<template #header><MkPageHeader v-model:tab="tab" :actions="headerActions" :tabs="headerTabs"/></template>
	<MkSpacer :content-max="700">
		<div v-if="tab === 'featured'">
			<MkPagination v-slot="{items}" :pagination="featuredPagination">
				<MkChannelPreview v-for="channel in items" :key="channel.id" class="_margin" :channel="channel"/>
			</MkPagination>
		</div>
		<div v-else-if="tab === 'favorites'">
			<MkPagination v-slot="{items}" :pagination="favoritesPagination">
				<MkChannelPreview v-for="channel in items" :key="channel.id" class="_margin" :channel="channel"/>
			</MkPagination>
		</div>
		<div v-else-if="tab === 'following'">
			<MkPagination v-slot="{items}" :pagination="followingPagination">
				<MkChannelPreview v-for="channel in items" :key="channel.id" class="_margin" :channel="channel"/>
			</MkPagination>
		</div>
		<div v-else-if="tab === 'owned'">
			<MkButton class="new" @click="create()"><i class="ti ti-plus"></i></MkButton>
			<MkPagination v-slot="{items}" :pagination="ownedPagination">
				<MkChannelPreview v-for="channel in items" :key="channel.id" class="_margin" :channel="channel"/>
			</MkPagination>
		</div>
	</MkSpacer>
</MkStickyContainer>
</template>

<script lang="ts" setup>
import { computed } from 'vue';
import MkChannelPreview from '@/components/MkChannelPreview.vue';
import MkPagination from '@/components/MkPagination.vue';
import MkButton from '@/components/MkButton.vue';
import { useRouter } from '@/router';
import { definePageMetadata } from '@/scripts/page-metadata';
import { i18n } from '@/i18n';

const router = useRouter();

let tab = $ref('featured');

const featuredPagination = {
	endpoint: 'channels/featured' as const,
	noPaging: true,
};
const favoritesPagination = {
	endpoint: 'channels/my-favorites' as const,
	limit: 100,
};
const followingPagination = {
	endpoint: 'channels/followed' as const,
	limit: 10,
};
const ownedPagination = {
	endpoint: 'channels/owned' as const,
	limit: 10,
};

function create() {
	router.push('/channels/new');
}

const headerActions = $computed(() => [{
	icon: 'ti ti-plus',
	text: i18n.ts.create,
	handler: create,
}]);

const headerTabs = $computed(() => [{
	key: 'featured',
	title: i18n.ts._channel.featured,
	icon: 'ti ti-comet',
}, {
	key: 'favorites',
	title: i18n.ts.favorites,
	icon: 'ti ti-star',
}, {
	key: 'following',
	title: i18n.ts._channel.following,
	icon: 'ti ti-eye',
}, {
	key: 'owned',
	title: i18n.ts._channel.owned,
	icon: 'ti ti-edit',
}]);

definePageMetadata(computed(() => ({
	title: i18n.ts.channel,
	icon: 'ti ti-device-tv',
})));
</script>
