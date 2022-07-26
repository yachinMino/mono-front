<script lang="ts" context="module">
	//画面ロード時の処理はこちらに記述する。
	export async function load({ url }: { url: any }) {
		const queries = url.searchParams;
		const id = queries.has('id') ? queries.get('id') : '';
		const getUrl = `http://localhost:3000/users/${id}`;
		let initial;
		await fetch(getUrl)
			.then((response) => {
				return response.text(); //ここでBodyからJSONを返す
			})
			.then((result) => {
				initial = JSON.parse(result); //取得したJSONデータを渡す
			})
			.catch((e) => {
				console.log(e); //エラーをキャッチし表示
			});

		return {
			props: {
				userInfo: initial
			}
		};
	}
</script>

<script lang="ts">
	//一行しか返ってこないので配列にしなくてOK
	export let userInfo: { id: number | string; name: string } = { id: '', name: 'test' };
	let updateUserName: string = '';

	function putUserName() {
		userInfo.name = updateUserName;
		console.log(userInfo);
		fetch(`http://localhost:3000/users/${userInfo.id}`, {
			method: 'PUT',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify(userInfo)
		})
			.then((response) => response.json())
			.then((data) => {
				console.log('Success:', data);
			})
			.catch((error) => {
				console.error('Error:', error);
			});
	}
</script>

<h1>ユーザー情報詳細</h1>

<div class="user-info-detail">
	<p>ユーザーID {userInfo.id}</p>
	<p>ユーザー名 {userInfo.name}</p>
</div>

<div class="user-info-update">
	<input type="text" placeholder="変更後のユーザー名" bind:value={updateUserName} />
</div>

<div class="action-button">
	<button on:click={() => putUserName()}>ユーザ情報更新</button>
</div>
