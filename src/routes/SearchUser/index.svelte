<script lang="ts" context="module">
	//画面ロード時の処理はこちらに記述する。
	/** @type {import('./__types/[slug]').Load} */
	export async function load() {
		let url = 'http://localhost:3000/users/';
		let initial;
		await fetch(url)
			.then((response) => {
				return response.text(); //ここでBodyからJSONを返す
			})
			.then((result) => {
				initial = JSON.parse(result); //取得したJSONデータを渡す（配列で返ってくるので配列にしなくてOK）
				console.log(initial);
			})
			.catch((e) => {
				console.log(e); //エラーをキャッチし表示
			});

		return {
			props: {
				users: initial
			}
		};
	}
</script>

<script lang="ts">
	import { escape } from 'svelte/internal';

	export let users: [{ id: number | string; name: string }] = [{ id: '', name: '' }];
	let inputUserId: number | string = '';
	let inputUserName: string = '';

	//検索時に必要なだけなのでreactiveしない
	let url = 'http://localhost:3000/users/?';

	async function getUserInfo() {
		//検索条件
		if (inputUserId !== '') {
			url = url + `id=${inputUserId}`;
		}
		if (inputUserName !== '') {
			url = url + `&name=${inputUserName}`;
		}

		fetch(url)
			.then((response) => {
				return response.text(); //ここでBodyからJSONを返す
			})
			.then((result) => {
				users = JSON.parse(result); //取得したJSONデータを渡す（配列で返ってくるので配列にしなくてOK）
			})
			.catch((e) => {
				console.log(e); //エラーをキャッチし表示
			});

		url = 'http://localhost:3000/users/?';
	}
</script>

<div class="searching-option-header">
	<p>検索条件</p>
</div>

<div class="searching-option">
	<input type="text" placeholder="ユーザーID" bind:value={inputUserId} />
	<input type="text" placeholder="ユーザー名" bind:value={inputUserName} />
	<button on:click={() => getUserInfo()}>検索</button>
</div>

<div class="searching-result-header">
	<p>検索結果</p>
</div>

<div class="searching-result">
	{#if users}
		{#each users as user}
			<a href="/UserDetail" class="user-info">
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
	}
</style>
