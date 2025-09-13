<script>
    let name =$state('');
    let welcome =$derived(name ? 'hello, ${name}!':'welcome!');

    function randomize()
    {
        const msgs = ['웹 개발 연습!','Sveltekit 익히기!','vercel 이용해서 배포!','안녕하세요'];
        alert(msgs[Math.floor(Math.random()*msgs.length)]);
    }

</script>

<h1>Welcome to SvelteKit</h1>
<label>
    your name: <input placeholder="type your name" bind:value={name}/>
</label>
<p>미리보기: <strong>{name || '(입력 대기)'}</strong></p>
<button onclick={randomize}>랜덤 메시지</button>


