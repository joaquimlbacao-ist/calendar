<!--
  - @copyright Copyright (c) 2019 Georg Ehrke <oc.list@georgehrke.com>
  -
  - @author Georg Ehrke <oc.list@georgehrke.com>
  -
  - @license AGPL-3.0-or-later
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->
<template>
<div class="property-title">
	<emoji-picker @emoji="insert" :search="search">
		<div slot="emoji-invoker" slot-scope="{ events: { click: clickEvent } }" @click.stop="clickEvent">
			<button type="button" class="button">
				<Emoji />
			</button>
		</div>
		<div slot="emoji-picker" slot-scope="{ emojis, insert, display }">
			<div>
			<div>
				<input type="text" v-model="search">
			</div>
			<div>
				<div v-for="(emojiGroup, category) in emojis" :key="category">
				<h5>{{ category }}</h5>
				<div>
					<span
					v-for="(emoji, emojiName) in emojiGroup"
					:key="emojiName"
					@click="insert(emoji)"
					:title="emojiName"
					>{{ emoji }}</span>
				</div>
				</div>
			</div>
			</div>
		</div>
	</emoji-picker>

	<div class="property-title__input"
		:class="{ 'property-title__input--readonly': isReadOnly }">
		<input v-if="!isReadOnly"
			v-focus
			type="text"
			:placeholder="t('calendar', 'Event title')"
			:value="value"
			@input.prevent.stop="changeValue">
		<!-- eslint-disable-next-line vue/singleline-html-element-content-newline -->
		<div v-else>{{ value }}</div>
	</div>
</div>
</template>

<script>
import focus from '../../../directives/focus.js'
import Emoji from 'vue-material-design-icons/EmoticonOutline.vue'
import { EmojiPicker } from 'vue-emoji-picker'

export default {
	name: 'PropertyTitle',
	data() {
		return {
			input: '',
			search: '',
		}
	},
	directives: {
		focus,
	},
	components: {
		EmojiPicker,
		Emoji,
	},
	props: {
		isReadOnly: {
			type: Boolean,
			required: true,
		},
		value: {
			type: String,
			default: '',
		},
	},
	methods: {
		changeValue(event) {
			this.$emit('update:value', event.target.value)
		},
		insert(emoji) {
			this.value += emoji;
		},
	},
}
</script>

<style lang="scss" scoped>
.button {
background-color: white;
border-style: solid;
color: black;
padding: 6px 6px;
text-align: center;
text-decoration: none;
display: inline-block;
font-size: 16px;
border-radius: 12px;
}
</style>

