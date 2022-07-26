<script lang="ts">
    let userId;
    let userName;

    function postUser(){
        const userInfo:[{id:Number,name:String}] = [];
        fetch(`http://localhost:3000/users/1`, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify(userInfo[0])
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

<div class="user-insert-form">
<p>ユーザーID</p><input type="text" placeholder="ユーザーID">
<p>ユーザー名</p><input type="text" placeholder="ユーザー名">

</div>

<div  class="action-button">
    <button>登録</button>
</div>