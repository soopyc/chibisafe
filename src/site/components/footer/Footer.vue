<template>
	<footer>
		<div class="container">
			<div class="columns">
				<div class="column is-narrow">
					<h4>chibisafe</h4>
					<span>© 2017-{{ getYear }}
						<a
							href="https://github.com/pitu"
							class="no-block">Pitu</a>
					</span><br>
					<span>(<a href="//github.com/soopyc/chibisafe" class="no-block">edited</a> by <a href="//soopy.moe" class="no-block">soopyc</a>)</span><br>
					<span>{{ version }}</span>
				</div>
				<div class="column is-narrow bottom-up">
					<a href="https://github.com/weebdev/chibisafe">GitHub</a>
					<a href="https://patreon.com/pitu">Patreon</a>
					<a href="https://discord.gg/5g6vgwn">Discord</a>
				</div>
				<div class="column is-narrow bottom-up">
					<a v-if="loggedIn" @click="createShareXThing">ShareX Config</a>

					<router-link to="/canary" exact>Transparency</router-link>
					<router-link to="/faq" exact>FAQ</router-link>
					<!--<a href="https://chrome.google.com/webstore/detail/lolisafe-uploader/enkkmplljfjppcdaancckgilmgoiofnj">Chrome Extension</a>-->
				</div>
			</div>
		</div>
	</footer>
</template>

<script>
/* eslint-disable no-restricted-globals */

import { mapState, mapGetters } from 'vuex';
import { saveAs } from 'file-saver';

export default {
	computed: {
		...mapGetters({
			loggedIn: 'auth/isLoggedIn',
			apiKey: 'auth/getApiKey'
		}),
		...mapState({
			version: state => state.config.version
		}),
		getYear() {
			return new Date().getFullYear();
		}
	},
	methods: {
		createShareXThing() {
			const sharexFile = `{
				"Name": "${this.$store.state.config.serviceName}",
				"DestinationType": "ImageUploader, FileUploader",
				"RequestType": "POST",
				"RequestURL": "${location.origin}/api/upload",
				"FileFormName": "files[]",
				"Headers": {
					"token": "${this.apiKey}",
					"accept": "application/vnd.chibisafe.json"
				},
				"ResponseType": "Text",
				"URL": "$json:url$",
				"ThumbnailURL": "$json:thumb$"
			}`;
			const sharexBlob = new Blob([sharexFile], { type: 'application/octet-binary' });
			saveAs(sharexBlob, `${location.hostname}.sxcu`);
		}
	}
};
</script>

<style lang="scss" scoped>
	@import '~/assets/styles/_colors.scss';
	footer {
		pointer-events: none;
		touch-action: none;

		@media screen and (min-width: 1025px) {
			position: fixed;
			bottom: 0;
			width: 100%;
			> div {
				padding: 1rem 1rem !important;
				max-width: unset !important;
			}
		}

		.container {
			.column {
				pointer-events: auto;
				touch-action: auto;

				text-align: center;
				@media screen and (min-width: 1025px) {
					margin-right: 2rem;
					&.bottom-up {
						display: flex;
						flex-direction: column;
						justify-content: flex-end;
						margin-right: 0;
					}
				}

				a {
					display: block;
					color: $textColor;
					&:hover {
						color: white
					}
					&.no-block {
						display: inherit;
					}
				}
			}
		}
	}
</style>
