<!doctype html>
<html lang="ko">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>귓밥 파기 — 면봉 끝 붙는 버전 (업그레이드 효과 약하게)</title>
<style>
  :root{--bg:#f6efe8;--muted:#666;--accent:#d9a84b}
  *{box-sizing:border-box}
  body{margin:0;padding:18px;background:linear-gradient(180deg,var(--bg),#efe7db);font-family:Inter, "Noto Sans KR",system-ui;color:#222}
  .scoreBigWrap{max-width:980px;margin:8px auto;display:flex;justify-content:center;align-items:center}
  .scoreBig{font-weight:900;font-size:36px;padding:6px 14px;border-radius:12px;background:linear-gradient(90deg,#fff,#fff6e6);box-shadow:0 8px 24px rgba(0,0,0,.07)}
  .wrap{max-width:980px;margin:8px auto;display:flex;flex-direction:column;gap:12px}
  .stage{width:100%;height:640px;background:linear-gradient(180deg,#fff,#fee9d9);border-radius:12px;display:flex;gap:16px;padding:12px;position:relative;overflow:hidden;border:6px solid #f2d8b9}
  .ear-area{flex:0 0 620px;height:100%;position:relative;display:flex;align-items:center;justify-content:center}
  .ear-img{width:520px;height:620px;object-fit:contain}
  .swabArea{position:absolute;right:18px;top:18px;width:72px;height:160px;border-radius:12px;display:flex;align-items:center;justify-content:center;flex-direction:column;pointer-events:auto;z-index:400}
  .swab{width:22px;height:130px;display:flex;flex-direction:column;align-items:center;pointer-events:auto;cursor:grab}
  .stick{width:6px;height:98px;background:#6b4f3a;border-radius:4px}
  .head{width:30px;height:30px;border-radius:50%;background:#fff;border:4px solid #efecec;margin-top:-8px;box-shadow:0 8px 20px rgba(0,0,0,.12);position:relative;overflow:visible}
  .tissue-zone{flex:0 0 240px;height:240px;border-radius:12px;background:linear-gradient(180deg,#fff,#fff6e6);display:flex;align-items:center;justify-content:center;flex-direction:column;box-shadow:0 10px 30px rgba(0,0,0,.06);position:relative}
  .tissue-label{position:absolute;top:10px;left:12px;font-weight:700;color:#a35f0f}
  .tissue-img{font-size:72px}
  .log{font-size:13px;color:var(--muted);height:56px;overflow:auto;padding:8px;border-radius:8px;background:#fbf7f4;margin-top:8px}

  .wax{position:absolute;border-radius:50%;box-shadow:0 10px 28px rgba(0,0,0,.14);user-select:none;touch-action:none;pointer-events:none;display:flex;align-items:center;justify-content:center}
  .wax.small{width:28px;height:20px;background:radial-gradient(circle at 30% 30%, #c99a4b, #8a5e28)}
  .wax.med{width:50px;height:32px;background:radial-gradient(circle at 30% 30%, #c79a4c, #7a4f22)}
  .wax.big{width:74px;height:52px;background:radial-gradient(circle at 30% 30%, #b8843b, #5a3416)}
  .wax.lotto{width:86px;height:62px;border-radius:50%;background:conic-gradient(from 200deg, #ffd94d, #ffef9c, #ffd94d 60%, #f6d08b);border:3px solid rgba(255,240,160,0.9);filter:drop-shadow(0 8px 28px rgba(255,190,40,0.28));}
  .wax.lotto::after{content:"LOTTO";position:absolute;bottom:6px;font-weight:900;color:#6b3c00;font-size:12px;letter-spacing:1px;text-shadow:0 2px 0 rgba(255,240,200,0.9)}
  .progressBadge{position:absolute;right:-14px;top:-12px;background:rgba(0,0,0,0.7);color:#fff;font-size:11px;padding:6px 8px;border-radius:10px;display:none}
  .sizeLabel{position:absolute;left:-14px;top:-12px;background:rgba(0,0,0,0.5);color:#fff;font-size:11px;padding:4px 8px;border-radius:10px}
  .attachedHint{position:absolute;left:14px;bottom:14px;font-size:13px;color:#a44;display:none;z-index:600}

  .particle{position:fixed;width:6px;height:6px;border-radius:50%;pointer-events:none;z-index:500;transform:translate(-50%,-50%) scale(1);animation:particleMove 700ms linear forwards}
  @keyframes particleMove{ to{transform:translate(-50%,-50%) translateY(-24px) translateX(18px) scale(0.2);opacity:0} }

  .blood{position:fixed;width:8px;height:8px;border-radius:50%;pointer-events:none;z-index:900;opacity:0.95;background:radial-gradient(circle at 30% 30%, #ff7a7a, #b30000);transform-origin:center;animation:blood-fall var(--dur,900ms) cubic-bezier(.2,.8,.2,1) forwards;box-shadow:0 6px 12px rgba(180,20,20,0.35)}
  @keyframes blood-fall{ to{ transform:translateY(180px) translateX(var(--dx,20px)) scale(0.4); opacity:0 } }

  .confetti{position:fixed;width:10px;height:16px;pointer-events:none;z-index:1100;border-radius:2px;opacity:0.95;transform-origin:center;animation:confetti-fall var(--dur,1200ms) linear forwards}
  @keyframes confetti-fall{ to{transform:translateY(380px) rotate(420deg);opacity:0} }

  .screen-flash{position:fixed;left:0;top:0;width:100%;height:100%;pointer-events:none;z-index:1500;background:rgba(200,20,20,0.08);opacity:0;transition:opacity 220ms ease-out}
  .screen-flash.show{opacity:1;transition:opacity 120ms ease-in}

  .shop{max-width:980px;margin:0 auto;background:#fff9f0;border-radius:12px;padding:10px 12px;box-shadow:0 8px 18px rgba(0,0,0,.05);display:flex;flex-direction:column;gap:8px;border:1px solid #f0dec3}
  .shop-header{display:flex;justify-content:space-between;align-items:center}
  .shop-title{font-weight:800;font-size:15px;color:#a35f0f}
  .shop-points{font-size:13px;color:#77552a}
  .shop-items{display:flex;flex-wrap:wrap;gap:8px}
  .shop-btn{flex:1 1 200px;border:none;border-radius:999px;padding:7px 12px;font-size:12px;cursor:pointer;background:linear-gradient(90deg,#ffe9c4,#ffd9aa);box-shadow:0 4px 10px rgba(0,0,0,.08);display:flex;flex-direction:column;align-items:flex-start;gap:2px}
  .shop-btn span{font-size:11px;color:#6b4f3a}

  @media(max-width:900px){
    .stage{flex-direction:column;height:auto}
    .ear-area{width:100%;height:420px}
    .tissue-zone{margin-top:12px}
  }
</style>
</head>
<body>
  <div class="screen-flash" id="screenFlash" aria-hidden="true"></div>

  <div class="scoreBigWrap">
    <div class="scoreBig" id="scoreBig">POINTS: 0</div>
  </div>

  <div class="wrap">
    <div class="stage" id="stage">
      <div class="ear-area" id="earArea">
        <img class="ear-img" id="earImg" src="https://raw.githubusercontent.com/hansae777/earsss/main/ear-9663602_1920.00_00_00_00.%EC%8A%A4%ED%8B%B8%20001.png" alt="귀">

        <div class="swabArea" id="swabArea">
          <div class="swab" id="swab" title="면봉을 꾹 눌러서 문지르세요">
            <div class="stick"></div>
            <div class="head" id="swabHead"></div>
          </div>
        </div>

        <div class="attachedHint" id="attachedHint">면봉 끝에 붙음 — 휴지로 드래그하세요</div>
      </div>

      <div class="tissue-zone" id="tissueZone">
        <div class="tissue-label">휴지 (같은 칸)</div>
        <div class="tissue-img" id="tissueImg">🧻</div>
      </div>
    </div>

    <div class="log" id="log">업그레이드로 퍼센트가 진짜 아주 조금씩만 더 잘 차도록 조정했습니다.</div>

    <div class="shop" id="shop">
      <div class="shop-header">
        <div class="shop-title">상점</div>
        <div class="shop-points">현재 포인트: <span id="shopPoints">0</span></div>
      </div>
      <div class="shop-items">
        <button class="shop-btn" data-upgrade="spawn" data-cost="40">
          귓밥 생성 속도 업
          <span>새 귓밥 생성 간격이 0.3초씩 줄어듭니다. (40점, 여러 번 구매 가능)</span>
        </button>
        <button class="shop-btn" data-upgrade="progress" data-cost="40">
          퍼센트 차오름 업
          <span>문지를 때 퍼센트가 거의 안 느껴질 정도로 아주 조금 더 잘 찹니다. (40점, 여러 번 구매 가능)</span>
        </button>
      </div>
    </div>
  </div>

<script>
(() => {
  const DAMAGE_DELTA_THRESHOLD = 2.2;
  const DAMAGE_POINTS = 1;
  const DAMAGE_COOLDOWN_MS = 1800;
  const DAMAGE_FACTOR = 1.0;

  const earArea     = document.getElementById('earArea')
  const swab        = document.getElementById('swab')
  const swabHead    = document.getElementById('swabHead')
  const tissueZone  = document.getElementById('tissueZone')
  const log         = document.getElementById('log')
  const scoreBig    = document.getElementById('scoreBig')
  const attachedHint= document.getElementById('attachedHint')
  const screenFlash = document.getElementById('screenFlash')
  const shopPointsEl = document.getElementById('shopPoints')
  const shopEl      = document.getElementById('shop')

  let score = 0
  let waxList = []
  let nextId = 1

  let holding = false
  let attachedWax = null
  let pointerX = 0, pointerY = 0

  const SIZE_SCORE = { small:1, med:3, big:7, lotto:500 }
  const RADIUS_THRESH = { small:18, med:28, big:40, lotto:40 }
  const ATTACH_RADIUS_FACTOR = 0.55
  const MIN_TURNS = 1, MAX_TURNS = 20
  const LOTTO_PROB = 1.23e-7
  const MAX_WAX_ON_EAR = 5

  let spawnInterval = null
  let spawnIntervalMs = 15000
  let spawnLevel = 0
  let progressLevel = 0

  const BASE_PROGRESS_MULT   = 0.55
  const PROGRESS_PER_LEVEL   = 0.01

  let lastDamageTs = 0

  function nowTs(){ return performance.now() }
  function logAdd(s){ log.textContent = `[${new Date().toLocaleTimeString()}] ${s}` }
  function refreshShopScore(){
    if (shopPointsEl) shopPointsEl.textContent = String(score)
  }
  function updateScoreUI(){
    scoreBig.textContent = `POINTS: ${score}`;
    scoreBig.animate(
      [{ transform: 'scale(1)' }, { transform: 'scale(1.06)' }, { transform: 'scale(1)' }],
      { duration: 300, easing: 'ease-out' }
    )
    refreshShopScore()
  }

  function getSpawnBoundsFromArea(area){
    const r = earArea.getBoundingClientRect()
    const cx = r.left + area.cx * r.width
    const cy = r.top + area.cy * r.height
    const radius = area.r * Math.min(r.width, r.height)
    return {
      left: cx - radius + 6,
      top: cy - radius + 6,
      width: radius*2 - 12,
      height: radius*2 - 12,
      right: cx + radius - 6,
      bottom: cy + radius - 6
    }
  }

  function confettiBurst(x,y,count=18){
    const colors = ['#ffd84d','#ff9f9f','#ffe3b3','#ffd1f0','#c6ffd8','#ffd6a8']
    for (let i=0;i<count;i++){
      const c = document.createElement('div')
      c.className = 'confetti'
      c.style.left = (x + (Math.random()*120-60)) + 'px'
      c.style.top = (y + (Math.random()*40-20)) + 'px'
      c.style.background = colors[Math.floor(Math.random()*colors.length)]
      c.style.setProperty('--dur', `${900 + Math.random()*600}ms`)
      c.style.transform = `translateY(0) rotate(${Math.random()*360}deg)`
      document.body.appendChild(c)
      setTimeout(()=>{ try{ c.remove() }catch(e){} }, 1600)
    }
  }

  function emitParticlesAt(x,y,amount=6,gold=false){
    const maxParticles = 200
    for (let i=0;i<amount;i++){
      if (document.querySelectorAll('.particle').length > maxParticles) break
      const p = document.createElement('div'); p.className='particle'
      const ox = (Math.random()*26 - 13), oy = (Math.random()*20 - 8)
      p.style.left = (x + ox) + 'px'; p.style.top = (y + oy) + 'px'
      if (gold) p.style.background = `rgba(255,220,90,${0.9 - Math.random()*0.25})`
      else {
        const shade = 140 + Math.floor(Math.random()*80)
        p.style.background = `rgba(${shade-60},${shade-30},${shade/2},${0.9 - Math.random()*0.3})`
      }
      document.body.appendChild(p)
      setTimeout(()=> { try{ p.remove() } catch(e){} }, 900)
    }
  }

  function bloodBurst(x,y,count=10){
    for (let i=0;i<count;i++){
      const b = document.createElement('div')
      b.className = 'blood'
      const dx = (Math.random()*120 - 60).toFixed(1)
      b.style.left = (x + (Math.random()*24 - 12)) + 'px'
      b.style.top = (y + (Math.random()*8 - 4)) + 'px'
      b.style.setProperty('--dx', `${dx}px`)
      b.style.setProperty('--dur', `${700 + Math.random()*600}ms`)
      b.style.transform = `translateY(0) rotate(${Math.random()*360}deg)`
      document.body.appendChild(b)
      setTimeout(()=>{ try{ b.remove() }catch(e){} }, 1400)
    }
    screenFlash.classList.add('show')
    setTimeout(()=> screenFlash.classList.remove('show'), 160)
  }

  function createWaxElement(type, x, y){
    const el = document.createElement('div')
    const cls = type === 'lotto' ? 'wax lotto' :
                (type==='big' ? 'wax big':
                (type==='med' ? 'wax med':'wax small'))
    el.className = cls
    el.style.left = x + 'px'; el.style.top = y + 'px'
    el.style.transform = `translate(-50%,-50%) rotate(${Math.random()*40-20}deg)`
    el.style.pointerEvents = 'none'
    const badge = document.createElement('div'); badge.className='progressBadge'; badge.textContent='0%'; badge.style.display='none'
    const sLabel = document.createElement('div'); sLabel.className='sizeLabel'
    if (type === 'lotto') {
      sLabel.textContent = 'LOTTO'
      sLabel.style.background = 'linear-gradient(90deg,#ffd87a,#fff2c7)'
      sLabel.style.color = '#7a3f00'
      sLabel.style.fontWeight = '900'
      sLabel.style.fontSize = '12px'
    } else {
      sLabel.textContent = type.toUpperCase()
    }
    el.appendChild(badge); el.appendChild(sLabel)
    document.body.appendChild(el)
    return {el, badge}
  }

  const spawnAreas = [
    {cx: 0.53, cy: 0.14, r: 0.06},
    {cx: 0.66, cy: 0.26, r: 0.08},
    {cx: 0.41, cy: 0.38, r: 0.09},
    {cx: 0.45, cy: 0.48, r: 0.08},
    {cx: 0.60, cy: 0.46, r: 0.065},
    {cx: 0.48, cy: 0.62, r: 0.12}
  ]

  function spawnWax(size=null){
    if (waxList.length >= MAX_WAX_ON_EAR) return
    const area = spawnAreas[Math.floor(Math.random()*spawnAreas.length)]
    const b = getSpawnBoundsFromArea(area)
    const rx = b.left + Math.random()*Math.max(1,b.width)
    const ry = b.top + Math.random()*Math.max(1,b.height)

    const isLotto = (size === 'lotto') || (size === null && Math.random() < LOTTO_PROB)
    if (isLotto){
      const {el, badge} = createWaxElement('lotto', rx, ry)
      const requiredTurns = Math.floor(MIN_TURNS + Math.random()*(MAX_TURNS-MIN_TURNS+1))
      const w = {
        id:String(nextId++), el,
        size:'lotto',
        attached:false,
        angleAccum:0,
        lastAngle:null,
        requiredTurns,
        badge,
        loosened:false
      }
      waxList.push(w)
      confettiBurst(rx, ry, 26); emitParticlesAt(rx, ry, 24, true)
      logAdd(`★ LOTTO 등장! 필요회전=${requiredTurns}`)
      return
    }

    const type = size || (Math.random() < 0.12 ? 'big' : (Math.random() < 0.45 ? 'med' : 'small'))
    const {el, badge} = createWaxElement(type, rx, ry)
    const requiredTurns = Math.floor(MIN_TURNS + Math.random()*(MAX_TURNS-MIN_TURNS+1))
    waxList.push({
      id:String(nextId++),
      el,
      size:type,
      attached:false,
      angleAccum:0,
      lastAngle:null,
      requiredTurns,
      badge,
      loosened:false
    })
    logAdd(`귓밥 생성: size=${type}, 필요회전=${requiredTurns} (현재 ${waxList.length}/${MAX_WAX_ON_EAR})`)
  }

  function startSpawnLoop(){
    if (spawnInterval) clearInterval(spawnInterval)
    spawnInterval = setInterval(()=> spawnWax(), spawnIntervalMs)
  }

  function waxCenter(w){
    const r = w.el.getBoundingClientRect()
    return {x: r.left + r.width/2, y: r.top + r.height/2}
  }
  function headPos(){
    const r = swabHead.getBoundingClientRect()
    return {x: r.left + r.width/2, y: r.top + r.height/2}
  }

  function attachWaxToHead(w){
    swabHead.appendChild(w.el)
    w.el.style.position = 'absolute'
    w.el.style.left = '50%'
    w.el.style.top  = '50%'
    w.el.style.transform = 'translate(-50%,-50%)'
  }

  function detachWaxToWorld(w){
    const r = w.el.getBoundingClientRect()
    document.body.appendChild(w.el)
    w.el.style.position = 'absolute'
    w.el.style.left = (r.left + r.width/2) + 'px'
    w.el.style.top  = (r.top  + r.height/2) + 'px'
    w.el.style.transform = 'translate(-50%,-50%)'
  }

  function headNearWax(w){
    const head = headPos()
    const c = waxCenter(w)
    const d = Math.hypot(head.x - c.x, head.y - c.y)
    return d <= (RADIUS_THRESH[w.size] || 30)
  }

  function updateRubbing(){
    if (!holding) return
    const head = headPos()
    for (let w of waxList){
      if (w.attached) continue

      if (w.loosened && headNearWax(w)){
        attachedWax = w
        w.attached = true
        w.badge.style.display = 'none'
        w.el.style.zIndex = 9999
        attachWaxToHead(w)
        attachedHint.style.display = 'block'
        logAdd(`다시 집음: 귓밥#${w.id} (${w.size})`)
        break
      }

      if (!headNearWax(w)){
        if (w.angleAccum === 0) w.badge.style.display='none'
        w.lastAngle = null
        continue
      }

      w.badge.style.display = 'block'
      const ang = (()=>{ const c= waxCenter(w); return Math.atan2(head.y - c.y, head.x - c.x); })()
      if (w.lastAngle === null){
        w.lastAngle = ang
        continue
      }

      let delta = ang - w.lastAngle
      while (delta <= -Math.PI) delta += 2*Math.PI
      while (delta >  Math.PI)  delta -= 2*Math.PI
      const absDelta = Math.abs(delta)
      const ts = nowTs()

      const baseDamageThreshold = DAMAGE_DELTA_THRESHOLD * DAMAGE_FACTOR
      let damageThreshold = baseDamageThreshold
      if (w.size === 'small') {
        damageThreshold = baseDamageThreshold * 1.3
      } else if (w.size === 'big') {
        damageThreshold = baseDamageThreshold * 0.85
      }

      if (absDelta >= damageThreshold && (ts - lastDamageTs) > DAMAGE_COOLDOWN_MS){
        lastDamageTs = ts
        score = Math.max(0, score - DAMAGE_POINTS)
        updateScoreUI()
        const h2 = headPos()
        bloodBurst(h2.x, h2.y, 8)
        logAdd(`상처 발생! -${DAMAGE_POINTS}점 (${w.size} — 크기별 판정 적용)`)
      }

      const progressMult = BASE_PROGRESS_MULT + progressLevel * PROGRESS_PER_LEVEL
      w.angleAccum += absDelta * progressMult
      w.lastAngle = ang

      const particleCount = Math.min(10, 1 + Math.round(absDelta*12))
      emitParticlesAt(head.x, head.y, particleCount, w.size==='lotto')

      const requiredRad = w.requiredTurns * 2 * Math.PI
      const pct = Math.min(100, Math.round((w.angleAccum / requiredRad) * 100))
      w.badge.textContent = pct + '%'

      const attachRadius = (RADIUS_THRESH[w.size] || 30) * ATTACH_RADIUS_FACTOR
      const headToWax = Math.hypot(head.x - waxCenter(w).x, head.y - waxCenter(w).y)
      if (w.angleAccum >= requiredRad && headToWax <= attachRadius){
        w.loosened = true
        w.attached = true
        attachedWax = w
        w.badge.style.display = 'none'
        w.el.style.zIndex = 9999
        attachWaxToHead(w)
        attachedHint.style.display = 'block'
        if (w.size === 'lotto'){
          const wc = waxCenter(w)
          confettiBurst(wc.x, wc.y, 22)
          emitParticlesAt(wc.x, wc.y, 20, true)
        }
        logAdd(`붙음: 귓밥#${w.id} (${w.size}) — 완전히 파냄`)
        break
      }
    }
  }

  function setSwab(pageX, pageY){
    pointerX = pageX; pointerY = pageY
    const earRect = earArea.getBoundingClientRect()
    const x = Math.max(earRect.left + 8, Math.min(earRect.right - 8, pageX))
    const y = Math.max(earRect.top + 8, Math.min(earRect.bottom - 8, pageY))
    swab.style.position = 'fixed'
    swab.style.left = (x - 11) + 'px'
    swab.style.top = (y - 65) + 'px'
    if (!attachedWax){
      updateRubbing()
    }
  }

  function releaseAt(pageX, pageY){
    holding = false
    if (!attachedWax){
      swab.style.position = 'static'
      waxList.forEach(w=>{ if (w.angleAccum===0) w.badge.style.display='none' })
      return
    }
    const tz = tissueZone.getBoundingClientRect()

    detachWaxToWorld(attachedWax)

    if (pageX >= tz.left && pageX <= tz.right && pageY >= tz.top && pageY <= tz.bottom){
      const pts = SIZE_SCORE[attachedWax.size] || 1
      score += pts
      updateScoreUI()
      if (attachedWax.size === 'lotto'){
        const tc = {x: tz.left + tz.width/2, y: tz.top + tz.height/2}
        confettiBurst(tc.x, tc.y, 48)
        emitParticlesAt(tc.x, tc.y, 30, true)
      }
      logAdd(`휴지에 넣음 +${pts}점 (${attachedWax.size})`)
      try{ attachedWax.el.remove() } catch(e){}
      waxList = waxList.filter(w=> w.id !== attachedWax.id)
      attachedWax = null
      attachedHint.style.display = 'none'
    } else {
      if (attachedWax){
        attachedWax.el.style.zIndex = ''
        if (attachedWax.loosened){
          attachedWax.attached = false
          attachedWax.lastAngle = null
          attachedWax.badge.style.display = 'none'
          logAdd('떨어짐 — 다시 집어서 바로 넣을 수 있습니다')
        } else {
          attache
