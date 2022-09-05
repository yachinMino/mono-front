<script lang="ts" context="module">
	import * as api from '../../scripts/api_client.svelte';
	export async function load({ url }: { url: any }) {
		const queries = url.searchParams;
		const id = queries.has('id') ? queries.get('id') : '';
		const getUrl = `${api.BASE_HOST}/users/findById/${id}`;

		const response = await fetch(getUrl);

		return {
			status: response.status,
			props: {
				responseUserInfo: response.ok && (await response.json())
			}
		};
	}
</script>

<script lang="ts">
	export let responseUserInfo: [{ id: number | string; name: string }] = [{ id: '', name: '' }];
	let userInfo = responseUserInfo[0];
	let updateUserName: string = '';

	function putUserName() {
		userInfo.name = updateUserName;
		fetch(`${api.BASE_HOST}/users/`, {
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

	function deleteUserName() {
		fetch(`${api.BASE_HOST}/users/`, {
			method: 'DELETE',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify(userInfo)
		});
	}
</script>

<h1>ユーザー情報詳細</h1>

<div class="user-info-detail">
	<p>ユーザーID {userInfo.id}</p>
	<p>ユーザー名 {userInfo.name}</p>
</div>

<div class="user-info-update">
	<input
		type="text"
		placeholder="変更後のユーザー名"
		bind:value={updateUserName}
		name="modify-user-name"
	/>
</div>

<div class="action-button">
	<button on:click={() => putUserName()} name="do-modify-button">ユーザ情報更新</button>
	<a href="/SearchUser/"
		><button on:click={() => deleteUserName()} name="do-delete-button">ユーザ情報削除</button></a
	>
</div>
