<script lang="ts" context="module">
	// リクエスト送信先
	export const BASE_HOST = 'http://localhost:8080';

	// // リクエストを送信する
	// function send({ method, path, data }: { method: string; path: string; data?: string }) {
	// 	const opts: any = { method, headers: {} };

	// 	opts.headers['Content-Type'] = 'application/json';
	// 	if (data) {
	// 		opts.body = JSON.stringify(data);
	// 	}

	// 	return fetch(`${BASE_HOST}${path}`, opts)
	// 		.then((r) => r.text())
	// 		.then((json) => {
	// 			try {
	// 				return JSON.parse(json);
	// 			} catch (err) {
	// 				return json;
	// 			}
	// 		});
	// }

	// // GETリクエストを送信する
	// export function get(path: string) {
	// 	return send({ method: 'GET', path });
	// }

	// // POSTリクエストを送信する
	// export function post(path: string, data: string) {
	// 	return send({ method: 'POST', path, data });
	// }

	// // PUTリクエストを送信する
	// export function put(path: string, data: string) {
	// 	return send({ method: 'PUT', path, data });
	// }
</script>
