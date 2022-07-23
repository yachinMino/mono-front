<script lang="ts">
	let users = [
		{ id: '1', name: 'test-user1' },
		{ id: '2', name: 'test-user2' }
	];

	let inputUserId: number | string = '';
	let inputUserName: string = '';

	//bindした検索条件が更新されるたびにURLも併せてリアクティブになる。（便利すぎ）
	$: url = `http://localhost:3000/users?id=${inputUserId}`;

	async function getUserInfo() {
		console.log(url);
		fetch(url)
			.then((response) => {
				return response.json(); //ここでBodyからJSONを返す
			})
			.then((result) => {
				users = result; //取得したJSONデータを渡す（配列で返ってくるので配列にしなくてOK）
			})
			.catch((e) => {
				console.log(e); //エラーをキャッチし表示
				users = [];
			});
	}
</script>

<div class="searching-option-header">
	<p>検索条件</p>
</div>

<div class="searching-option">
	<input type="text" placeholder="ユーザーID" bind:value={inputUserId} />
	<input type="text" placeholder="ユーザー名（あいまい検索）" bind:value={inputUserName} />
	<button on:click={() => getUserInfo()}>検索</button>
</div>

<div class="searching-result-header">
	<p>検索結果</p>
</div>

<div class="searching-result">
	{#if users.length === 0}
		<p>検索結果が0件です。</p>
	{:else}
		{#each users as user}
			<div class="user-info">
				<p>
					ユーザーID {user.id}
				</p>
				<p>
					ユーザー名 {user.name}
				</p>
			</div>
		{/each}
	{/if}
</div>

<style>
	.searching-result {
		display: flex;
		flex-wrap: wrap;
	}

	.user-info {
		margin: 8px;
		border-style: solid;
		border: 1px;
		border-color: black;
		background-color: aqua;
	}
</style>
