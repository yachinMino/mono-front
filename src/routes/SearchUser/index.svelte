<script lang="ts" context="module">
	import * as api from '../../scripts/api_client.svelte';
	//画面ロード時の処理はこちらに記述する。
	/** @type {import('./__types/SearchUser').Load} */
	export async function load() {
		const url = `${api.BASE_HOST}/users/`;
		const response = await fetch(url);

		return {
			status: response.status,
			props: {
				users: response.ok && (await response.json())
			}
		};
	}
</script>

<script lang="ts">
	export let users: [{ id: number | string; name: string }] = [{ id: '', name: '' }];
	let inputUserId: number | string = '';
	let inputUserName: string = '';
	let searchOption = '';

	async function getUserInfo() {
		//検索条件
		if (inputUserId !== '' && inputUserName !== '') {
			searchOption = `/${inputUserId}/${inputUserName}`;
		} else {
			if (inputUserId !== '') {
				searchOption = searchOption + `/findById/${inputUserId}`;
			}
			if (inputUserName !== '') {
				searchOption = searchOption + `/findByName/${inputUserName}`;
			}
		}

		const url = `${api.BASE_HOST}/users${searchOption}`;
		console.log(url);
		const response = await fetch(url);
		searchOption = '';
		if (response.ok) {
			users = await response.json();
		}
	}
</script>

<div class="searching-option-header">
	<p>検索条件</p>
</div>

<div class="searching-option">
	<input type="text" placeholder="ユーザーID" bind:value={inputUserId} name="search-user-id" />
	<input type="text" placeholder="ユーザー名" bind:value={inputUserName} name="search-user-name" />
	<button on:click={() => getUserInfo()} name="do-search-button">検索</button>
</div>

<div class="searching-result-header">
	<p>検索結果</p>
</div>

<div class="searching-result">
	{#if users}
		{#each users as user}
			<a href={`/UserDetail/?id=${user.id}`} class="user-info" name={`user-info-${user.id}`}>
				<p>
					ユーザーID {user.id}
				</p>
				<p>
					ユーザー名 {user.name}
				</p>
			</a>
		{/each}
	{:else}
		<p>loading</p>
	{/if}
</div>

<style>
	.searching-result {
		display: flex;
		flex-wrap: wrap;
	}

	.user-info {
		margin: 8px;
		padding: 8px;
		border-radius: 8px;
		background-color: aqua;
		text-decoration: none;
		color: black;
	}
</style>
