<script lang="ts">
	let userId: number | string;
	let userName: string;

	$: userInfo = { id: userId, name: userName };
	function postUser() {
		// const userInfo:[{id:Number,name:String}] = [];
		fetch(`http://localhost:3000/users/`, {
			method: 'POST',
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

		userId = '';
		userName = '';
	}
</script>

<div class="user-insert-form">
	<p>ユーザーID</p>
	<input type="text" placeholder="ユーザーID" bind:value={userId} />
	<p>ユーザー名</p>
	<input type="text" placeholder="ユーザー名" bind:value={userName} />
</div>

<div class="action-button">
	<button on:click={() => postUser()}>登録</button>
</div>
