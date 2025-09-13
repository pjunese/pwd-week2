<!-- src/routes/projects/[slug]/+page.svelte -->
<script>
    let { data } = $props();      // { item, slug }
    let memo = $state('');
  
    // memo 상세에서만 로컬스토리지 동기화
    $effect(() => {
      if (data.slug === 'memo') {
        const saved = localStorage.getItem('memo');
        if (saved) memo = saved;
      }

      // ✅ [추가] timetable 상세에서 로컬스토리지 동기화
      if (data.slug === 'timetable') {
        const saved = localStorage.getItem('timetable');
        if (saved) timetable = JSON.parse(saved);
      }
    });
  
    function save() {
      localStorage.setItem('memo', memo);
      alert('저장 완료!');
    }


    // ✅ [추가] timetable 관련 상태/함수
    const DAYS = ['월', '화', '수', '목', '금'];
    const PERIODS = 8; // 교시 수 (원하면 늘리세요)
    
    let timetable = $state(
      Array.from({ length: PERIODS }, () => Array(DAYS.length).fill(''))
    );

    function saveTimetable() {
      localStorage.setItem('timetable', JSON.stringify(timetable));
      alert('시간표 저장 완료!');
    }
    
      // ✅ 교시 시간 계산
    function getPeriodTime(periodIndex) {
        const startHour = 9;
        const duration = 75; // 수업 75분
        const breakTime = 15; // 쉬는 시간 15분

        const offset = periodIndex * (duration + breakTime);
        const start = new Date(0, 0, 0, startHour, offset);
        const end = new Date(start.getTime() + duration * 60000);

        const fmt = d =>
        String(d.getHours()).padStart(2, '0') + ':' +
        String(d.getMinutes()).padStart(2, '0');

        return `${fmt(start)} ~ ${fmt(end)}`;
    }


  </script>
  
  <h2>{data.item.title}</h2>
  <p>{data.item.body}</p>
  
  {#if data.slug === 'memo'}
    <textarea rows="6" bind:value={memo} class="card" style="width:100%"></textarea>
    <button onclick={save}>메모 저장</button>
    <p style="opacity:.6">브라우저 로컬에만 저장됩니다.</p>
  {/if}


<!-- ✅ [추가] timetable 화면 -->
    {#if data.slug === 'timetable'}
    <table class="tt">
    <thead>
        <tr>
        <th>교시</th>
        {#each DAYS as d}<th>{d}</th>{/each}
        </tr>
    </thead>
    <tbody>
        {#each Array(PERIODS) as _, r}
        <tr>
            <!-- ✅ 교시 칸: 숫자 + 시간 두 줄 -->
            <td>
            <div>{r + 1}교시</div>
            <div style="font-size: 12px; color: #666;">
                ({getPeriodTime(r)})
            </div>
            </td>

            {#each DAYS as _, c}
            <td>
                <input type="text" bind:value={timetable[r][c]} />
            </td>
            {/each}
        </tr>
        {/each}
    </tbody>
    </table>
    <button onclick={saveTimetable}>시간표 저장</button>
    {/if}

<style>
    /* 테이블 전체 */
    table.tt {
      border-collapse: collapse;
      width: 100%;
      background: #fff;
      table-layout: fixed;              /* 셀 균등 분배 */
    }
    table.tt th, table.tt td {
      border: 1px solid #e9e9e9;
      padding: 0.6rem;                  /* 셀 패딩(칸 키우기) */
      text-align: center;
    }
    table.tt thead th {
      background: #f7f5ff;
      font-weight: 600;
    }
  
    /* 칸 크기 키우기 */
    table.tt td {
      min-width: 120px;                 /* 가로 폭 */
      height: 56px;                     /* 세로 높이 */
    }
  
    /* 입력창 크게 & 중앙 정렬 */
    table.tt input {
      width: 100%;
      height: 44px;
      font-size: 16px;
      text-align: center;
      border: none;
      outline: none;
      background: transparent;
    }
  
    /* 모바일에서 좌우 스크롤 가능하도록 (선택) */
    @media (max-width: 640px) {
      table.tt td { min-width: 90px; }
      table.tt input { font-size: 14px; height: 40px; }
    }
  </style>