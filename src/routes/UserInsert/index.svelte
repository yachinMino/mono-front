<script lang="ts">
	import * as api from '../../scripts/api_client.svelte';

	let userId: number | string;
	let userName: string;

	$: userInfo = { id: userId, name: userName };
	function postUser() {
		fetch(`${api.BASE_HOST}/users/`, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify(userInfo)
		});

		userId = '';
		userName = '';
	}
</script>

<div class="user-insert-form">
	<p>ユーザーID</p>
	<input type="text" placeholder="ユーザーID" bind:value={userId} name="user-id" />
	<p>ユーザー名</p>
	<input type="text" placeholder="ユーザー名" bind:value={userName} name="user-name" />
</div>

<div class="action-button">
	<button on:click={() => postUser()} name="do-insert-button">登録</button>
</div>
