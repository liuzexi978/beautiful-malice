
@echo off
cd /d "%~dp0"
start "" "index.html"

#!/bin/bash
# 上课偷吃 · 单人版 —— 一键开始
cd "$(dirname "$0")" || exit 1
URL="file://$(pwd)/index.html"
if [ -d "/Applications/Google Chrome.app" ]; then
  open -a "Google Chrome" "$URL"
elif [ -d "/Applications/Microsoft Edge.app" ]; then
  open -a "Microsoft Edge" "$URL"
elif [ -d "/Applications/Safari.app" ]; then
  open -a "Safari" "$URL"
else
  open "$URL"
fi

<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>上课偷吃 · 暗夜版 · 四川话 | 停电教室 · 同学举报 · 可反杀</title>
<link rel="stylesheet" href="css/style.css">
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ctext y='.9em' font-size='88'%3E%F0%9F%8D%9F%3C/text%3E%3C/svg%3E">
</head>
<body>
<div id="app">

  <!-- 3D 舞台 -->
  <div id="stage"></div>
  <div id="vignette"></div>
  <div id="dangerGlow"></div>
  <div id="flash"></div>
  <div id="grain"></div>

  <!-- ================= HUD ================= -->
  <div id="hud" class="hidden">
    <div class="topbar">
      <div class="hud-lesson">
        <span class="lesson-badge" id="hudLesson">第 1 节</span>
        <div class="lesson-meta">
          <div class="lesson-subject" id="hudSubject">数学课 · 王老师</div>
          <div class="clockbar"><div class="clockbar-fill" id="timeFill"></div></div>
        </div>
        <span class="clock" id="hudClock">08:00</span>
      </div>

      <div class="teacher-state" id="teacherState">
        <div class="ts-icon" id="tsIcon">🖊️</div>
        <div class="ts-text">
          <b id="tsTitle">板书</b>
          <span id="tsSub">老师背对着你 · 安全</span>
        </div>
        <div class="ts-eye" id="tsEye">…</div>
      </div>

      <div class="hud-right">
        <div class="hud-toprow">
          <div class="hearts" id="hearts"><i></i><i></i><i></i></div>
          <div class="score-chip"><span>分数</span><b id="hudScore">0</b></div>
        </div>
        <div class="suspicion" id="suspicionBox">
          <div class="susp-label"><span>老师的怀疑</span><b id="suspVal">0</b></div>
          <div class="susp-track"><div class="susp-fill" id="suspFill"></div><div class="susp-marks"></div></div>
        </div>
      </div>
    </div>

    <div class="center-layer">
      <div id="bigWarn" class="big-warn hidden"><span>老师转身了！</span><small>松开空格，快装乖！</small></div>
      <div id="cheek" class="cheek hidden">🐹 腮帮子鼓鼓的…… 快装乖！</div>
    </div>

    <div class="npc-bubble hidden" id="npcBubble"><span id="npcBubbleText"></span></div>

    <div class="snitch-alert hidden" id="snitchAlert">
      <div class="sa-head"><span class="sa-icon">🙋</span><span id="snitchText">同学要举报你！</span></div>
      <div class="sa-track"><div class="sa-fill" id="snitchFill"></div></div>
      <div class="sa-key">按 <kbd>E</kbd> 塞零食堵嘴 · 别让老师听见</div>
    </div>
    <div class="snitch-bubble hidden" id="snitchBubble">老师！他/她偷吃！</div>

    <div class="subtitle hidden" id="subtitle">
      <div class="speaker" id="subtitleSpeaker">王老师</div>
      <div class="line" id="subtitleText"></div>
    </div>

    <div id="begCard" class="beg-card hidden">
      <div class="beg-head"><span class="beg-who" id="begName">同桌 李小胖</span><span class="beg-tag">悄悄话</span></div>
      <div class="beg-text" id="begText">“兄弟，分我一口嘛……”</div>
      <div class="beg-hint"><span><kbd>Y</kbd> 给一口</span><span><kbd>N</kbd> 不给</span></div>
      <div class="beg-effect">给：饥饿 -12，之后帮你挡一次举报 · 不给：他会记仇</div>
      <div class="beg-bar"><div id="begFill"></div></div>
    </div>

    <div id="sneakCard" class="sneak-card hidden">
      <div class="sneak-head"><span>摸黑上讲台</span><span id="sneakRisk" class="sneak-risk">她还在写</span></div>
      <div class="sneak-track"><div id="sneakFill"></div></div>
      <div class="sneak-status" id="sneakStatus">按住 W 前进 · 松开停 · R 退回</div>
      <div class="sneak-keys"><kbd>W</kbd> 前进 · <kbd>R</kbd> 退回 · 到了按 <kbd>Q</kbd></div>
    </div>

    <div class="bottom">
      <div class="snack-panel">
        <div class="panel-title"><span>桌兜里的零食</span><span id="snackCount" class="count">3 份</span></div>
        <div class="hunger-row" id="hungerRow" title="饥饿值：吃零食可以恢复；低于 30 会肚子叫">
          <span class="h-label">🍜 饥饿</span>
          <div class="hunger-track"><div id="hungerFill"></div></div>
          <b id="hungerVal">62</b>
        </div>
        <div id="snackList" class="snack-list"></div>
        <div class="panel-tip">按 <kbd>1</kbd>~<kbd>5</kbd> 选择 · 按住 <kbd>空格</kbd> 开吃</div>
      </div>

      <div class="hints" id="hints">
        <div id="sneakHint" class="sneak-hint hidden">🗡️ <kbd>R</kbd> 摸黑上讲台刺杀老师</div>
        <div class="chew-meter hidden" id="chewMeter">
          <span id="chewText">咀嚼中…</span>
          <div class="chew-track"><div class="chew-fill" id="chewFill"></div></div>
        </div>
        <div class="hint-row"><kbd>按住空格</kbd><span>偷吃</span><i></i><kbd>松开</kbd><span>收手</span></div>
        <div class="hint-row small"><kbd>Shift</kbd> 装乖 · <kbd>F</kbd> 手机 · <kbd>E</kbd> 堵嘴 · <kbd>Q</kbd> 反杀 · <kbd>P</kbd> 暂停</div>
      </div>

      <div class="phone-wrap hidden" id="phoneWrap">
        <div class="phone-frame">
          <div class="phone-speaker"></div>
          <canvas id="phoneCanvas"></canvas>
          <div class="phone-home"></div>
          <div class="phone-glow"></div>
        </div>
        <div class="phone-tip">摸鱼中 · <kbd>F</kbd> 收起 · <kbd>↑↓←→</kbd> 操控</div>
      </div>
    </div>

    <div class="toasts" id="toasts"></div>
    <div class="punish-overlay hidden" id="punishOverlay">
      <div class="po-inner">
        <div class="po-stamp">罚 站</div>
        <p class="po-line">老师让你站到教室后面反省</p>
        <div class="po-timer"><b id="punishTime">10</b><span>秒</span></div>
        <p class="po-tip">站着也别偷吃，她真的会回头。</p>
      </div>
    </div>
    <div class="click-prompt hidden" id="clickPrompt">点击画面锁定鼠标 · 环顾教室</div>
    <div class="look-hint hidden" id="lookHint">🖱️ 移动鼠标环顾教室</div>
    <div class="act-innocent hidden" id="innocentFlag">📖 正在装乖</div>
  </div>

  <!-- 反杀机会 -->
  <div id="qte" class="qte hidden">
    <div class="qte-card">
      <div class="qte-eyebrow">反 杀 机 会</div>
      <div class="qte-title">桌兜里有一把美工刀</div>
      <div class="qte-key">Q</div>
      <div class="qte-bar"><div id="qteFill"></div></div>
      <div class="qte-hint">按 Q 刺她 · 不按就会被吃掉</div>
    </div>
  </div>
  <div id="slash" class="slash"></div>

  <!-- 被吃掉：电影黑边字幕 -->
  <div id="cinema" class="cinema hidden">
    <div class="cine-bar top"></div>
    <div class="cine-bar bottom"></div>
    <div class="cine-sub" id="cinemaSub"></div>
    <div class="cine-hint">你还能听见自己的心跳</div>
  </div>
  <div id="blackout" class="blackout"></div>

  <!-- ================= 标题页 ================= -->
  <div id="screenTitle" class="screen title-screen">
    <div class="title-wrap">
      <div class="title-card">
        <div class="tape tape-tl"></div>
        <div class="title-eyebrow">高三(2)班 · 最后一排靠窗 · 晚自习 19:40</div>
        <h1 class="game-title"><span>上</span><span>课</span><span class="accent">偷</span><span class="accent">吃</span></h1>
        <div class="title-sub">《我带过的最差一届》 · 单人版 · 暗夜篇</div>
        <div class="title-badges">
          <span>🕯️ 停电晚自习</span>
          <span>🙋 同学会举报</span>
          <span>👹 被看见，就会被吃掉</span>
          <span>🗡️ 被抓住按 Q 反杀</span>
          <span>🗣️ 四川话（川普）</span>
          <span>🍜 饥饿值</span>
          <span>🗡️ 停电可摸黑刺杀</span>
        </div>
        <p class="title-desc">
          灯突然灭了。整间教室只剩她的脚步声，和手电筒扫过课桌的光。<br>
          同学在黑暗里看着你。举报只要一秒钟。<br>
          这一次，被抓到不只是罚站——她会把你拎到讲台上，当着全班的面，吃掉。<br>
          除非，你先摸到桌兜里那把美工刀。<br>
          还有——她骂人，全程都是四川话。
        </p>
        <div class="title-actions">
          <button id="btnStart" class="btn primary big">开始上课 <span class="arrow">▶</span></button>
          <button id="btnHelp" class="btn ghost">玩法说明</button>
        </div>
        <div class="title-foot">
          <span>🖱️ 鼠标看四周</span>
          <span>⌨️ 按住空格偷吃</span>
          <span>🔦 躲开手电筒</span>
          <span>🙋 E 堵住举报的嘴</span>
          <span>🗡️ 被抓住按 Q 反杀</span>
          <span>🗣️ 四川话（川普）</span>
          <span>🍜 饥饿值</span>
          <span>🗡️ 停电可摸黑刺杀</span>
        </div>
      </div>

      <div class="title-note">
        <div class="tape tape-note"></div>
        <h3>暗夜版守则</h3>
        <ol>
          <li>停电后，手电筒扫到你时，连腮帮子都别动。</li>
          <li>同学会举报你。看到 🙋 就按 E 塞零食。</li>
          <li>红牌用完后，她会把你拎到讲台上——除非你先按 Q。</li>
          <li>这次没有“请家长”，只有“被吃掉”。</li>
        </ol>
        <div class="note-sign">—— 高三(2)班 值班室</div>
        <div class="note-doodle">🕯️ 🔦 🙋 👹</div>
      </div>
    </div>
  </div>

  <!-- ================= 课前说明 ================= -->
  <div id="screenBrief" class="screen modal-screen hidden">
    <div class="modal-card brief-card">
      <div class="brief-head">
        <div>
          <div class="brief-num">第 <b id="briefNum">1</b> 节课</div>
          <h2 id="briefSubject">数学课</h2>
        </div>
        <div class="brief-time" id="briefTime">上午 第二节</div>
      </div>

      <div class="teacher-row">
        <div class="teacher-avatar" id="briefAvatar">👩🏻‍🏫</div>
        <div class="teacher-say">
          <div class="teacher-name" id="briefTeacher">王老师</div>
          <div class="teacher-quote" id="briefQuote">“把书翻到第 58 页，这节课讲函数。”</div>
        </div>
      </div>

      <div id="briefStory" class="brief-story">晚自习前，你把三包零食塞进桌兜。</div>

      <div id="briefWarning" class="brief-warning hidden">⚠ 这节课会停电。手电筒扫到你时，别动嘴；同学举手时，按 E 堵嘴。全班只有一张红牌，用完再被抓，就不是罚站了。<br>等老师背对你写板书时，按 R 摸黑上讲台——按住 W 靠近，按 Q 刺杀，成功直接赢下这一局。</div>

      <div class="brief-grid">
        <div class="brief-box">
          <h4>🍽️ 本节课目标</h4>
          <p id="briefGoal">老师背过身时，吃完 <b>3</b> 份零食。</p>
        </div>
        <div class="brief-box">
          <h4>🎒 桌兜里的货</h4>
          <div id="briefSnacks" class="mini-snacks"></div>
        </div>
        <div class="brief-box">
          <h4>👀 老师的规矩</h4>
          <ul id="briefRules">
            <li>板书时尽情吃，转身时必须收手。</li>
            <li>被盯上别硬撑，按 <kbd>Shift</kbd> 装乖。</li>
            <li>红牌用完，她会把你吃掉。</li>
          </ul>
        </div>
      </div>

      <div class="brief-keys">
        <span><kbd>按住空格</kbd> 偷吃</span>
        <span><kbd>Shift</kbd> 装乖</span>
        <span><kbd>1</kbd>-<kbd>5</kbd> 换零食</span>
        <span><kbd>F</kbd> 手机</span>
        <span><kbd>E</kbd> 堵嘴</span>
        <span><kbd>P</kbd> 暂停</span>
      </div>

      <div class="modal-actions">
        <button id="btnBegin" class="btn primary big">起立！老师好！</button>
      </div>
    </div>
  </div>

  <!-- ================= 暂停 ================= -->
  <div id="screenPause" class="screen modal-screen hidden">
    <div class="modal-card pause-card">
      <div class="pause-stamp">课间休息</div>
      <h2>先别吃了</h2>
      <p class="muted">（老师去接了个电话，你可以喘口气）</p>
      <div class="pause-actions">
        <button id="btnResume" class="btn primary">继续上课</button>
        <button id="btnPauseMute" class="btn ghost">关闭声音</button>
        <button id="btnPauseRestart" class="btn ghost">重上这节课</button>
        <button id="btnPauseTitle" class="btn ghost">回主菜单</button>
      </div>
      <div class="pause-keys">
        <span><kbd>鼠标</kbd> / <kbd>方向键</kbd> 看四周</span>
        <span><kbd>空格</kbd> 吃</span>
        <span><kbd>Shift</kbd> 装乖</span>
        <span><kbd>F</kbd> 手机</span>
        <span><kbd>E</kbd> 堵嘴</span>
        <span><kbd>Q</kbd> 反杀</span>
        <span><kbd>R</kbd> 摸黑 / <kbd>W</kbd> 前进</span>
        <span>🗣️ 四川话</span>
      </div>
    </div>
  </div>

  <!-- ================= 下课结算 ================= -->
  <div id="screenResult" class="screen modal-screen hidden">
    <div class="modal-card result-card">
      <div class="result-head">
        <div class="result-bell">🔔</div>
        <div>
          <div class="result-eyebrow">下课铃响了</div>
          <h2 id="resultTitle">这节课，值了！</h2>
        </div>
        <div class="grade-stamp" id="resultGrade">A</div>
      </div>
      <p class="result-line" id="resultLine">你在一节课里完成了一场惊心动魄的进食。</p>
      <div class="result-stats">
        <div class="stat"><b id="resEaten">3/3</b><span>偷吃完成</span></div>
        <div class="stat"><b id="resCatches">0</b><span>被抓次数</span></div>
        <div class="stat"><b id="resPhone">0</b><span>手机摸鱼</span></div>
        <div class="stat highlight"><b id="resScore">0</b><span>本节得分</span></div>
      </div>
      <div class="res-snitch" id="resSnitch"></div>
      <div class="modal-actions">
        <button id="btnNextLesson" class="btn primary">下一节课</button>
        <button id="btnRetryLesson" class="btn ghost hidden">重上这节课</button>
        <button id="btnResultTitle" class="btn ghost">回主菜单</button>
      </div>
    </div>
  </div>

  <!-- ================= 请家长 ================= -->
  <div id="screenGameover" class="screen modal-screen hidden">
    <div class="modal-card gameover-card">
      <div class="parent-note">
        <div class="pn-head">请 家 长 通 知 单</div>
        <div class="pn-body">
          <p>贵子女 <b>高三(2)班 · 最后一排靠窗同学</b></p>
          <p>于本节课内多次在课堂上偷吃零食、玩手机，屡教不改，现通知家长于明日到校面谈。</p>
          <p class="pn-teacher">班主任：<span id="goTeacher">王老师</span></p>
        </div>
        <div class="pn-stamp">请家长</div>
      </div>
      <h2>被抓第三次了</h2>
      <p class="muted">老师已经拨通了你妈妈的电话。</p>
      <div class="modal-actions">
        <button id="btnGameoverRetry" class="btn primary">重上这节课</button>
        <button id="btnGameoverTitle" class="btn ghost">回主菜单</button>
      </div>
    </div>
  </div>

  <!-- ================= 被吃掉 ================= -->
  <div id="screenEaten" class="screen modal-screen eaten-screen hidden">
    <div class="eaten-card">
      <div class="missing-head">寻 人 启 事</div>
      <div class="missing-portrait">🎒</div>
      <p class="missing-name">高三(2)班 · 最后一排靠窗同学</p>
      <p class="missing-text">最后一次出现：晚自习停电时。<br>目击者称，讲台上传来了咀嚼声。</p>
      <div class="missing-meta"><span id="eatenLesson">第 4 节 · 停电晚自习</span><span>本场得分 <b id="eatenScore">0</b></span></div>
      <div class="missing-stamp">已 找 到</div>
      <h2>你被吃掉了</h2>
      <p class="muted">这届学生，少了一个。老师擦了擦嘴，继续上课。</p>
      <div class="modal-actions">
        <button id="btnEatenRetry" class="btn primary">重上这节课</button>
        <button id="btnEatenTitle" class="btn ghost">回主菜单</button>
      </div>
    </div>
  </div>

  <!-- ================= 刺杀成功 ================= -->
  <div id="screenAssassinate" class="screen modal-screen assassinate-screen hidden">
    <div class="assassin-card">
      <div class="assassin-stamp">已 除 名</div>
      <div class="assassin-eyebrow">值 班 室 · 结 案 报 告</div>
      <h2>刺杀成功</h2>
      <div class="assassin-photo">👩🏻‍🏫</div>
      <p class="assassin-name" id="assassinTeacher">值班老师</p>
      <p class="assassin-text">死因：被一名高三学生用美工刀……<br>最后一句话：“这届学生，真难带。”</p>
      <div class="assassin-meta"><span id="assassinLesson">第 1 节</span><span>本节得分 <b id="assassinScore">0</b></span></div>
      <p class="assassin-small">你擦干净刀，坐回最后一排。下课铃响了。</p>
      <div class="modal-actions">
        <button id="btnAssassinNext" class="btn primary">下一节课</button>
        <button id="btnAssassinRetry" class="btn ghost">重上这节课</button>
        <button id="btnAssassinTitle" class="btn ghost">回主菜单</button>
      </div>
    </div>
  </div>

  <!-- ================= 学期总评 ================= -->
  <div id="screenFinal" class="screen modal-screen hidden">
    <div class="modal-card final-card">
      <div class="final-eyebrow">学期结束 · 家长会现场</div>
      <h2 id="finalTitle">你活下来了</h2>
      <div class="final-grade" id="finalGrade">S</div>
      <div class="final-stats">
        <div><b id="finalScore">0</b><span>总得分</span></div>
        <div><b id="finalEaten">0</b><span>吃掉的零食</span></div>
        <div><b id="finalCatches">0</b><span>累计被抓</span></div>
        <div><b id="finalPhone">0</b><span>手机摸鱼分</span></div>
      </div>
      <p class="final-line" id="finalLine">老师看着你，欲言又止。这个班，确实是她带过的最差的一届。</p>
      <p class="final-snitch" id="finalSnitch"></p>
      <div class="modal-actions">
        <button id="btnPlayAgain" class="btn primary">再上一遍</button>
        <button id="btnFinalTitle" class="btn ghost">回主菜单</button>
      </div>
    </div>
  </div>

  <!-- ================= 玩法说明 ================= -->
  <div id="screenHelp" class="screen modal-screen hidden">
    <div class="modal-card help-card">
      <div class="help-head">
        <h2>暗夜版：怎么活过这节课？</h2>
        <button id="btnHelpClose" class="icon-btn" aria-label="关闭">✕</button>
      </div>
      <div class="help-grid">
        <div class="help-item">
          <div class="hi-icon">🖊️</div>
          <h4>看老师，别看食物</h4>
          <p>她背对黑板写板书时最安全。粉笔声一停、她肩膀一动，就是转身的信号。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">👄</div>
          <h4>按住空格，松开收手</h4>
          <p>每份零食要嚼几下才吃完。松手后腮帮子还会鼓一小会儿，别在她回头时咬着不放。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">😇</div>
          <h4>被盯上就装乖</h4>
          <p>怀疑度会随着她的注视上涨。按住 Shift 低头看书，能让她尽快失去兴趣。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">🍗</div>
          <h4>零食也有性格</h4>
          <p>辣条、薯片、干脆面很响，但快；奶茶、香蕉安静，但慢。按 1-5 换着吃。吃零食还能回饥饿值；低于 30 肚子会叫，把老师引过来。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">📱</div>
          <h4>手机是高风险摸鱼</h4>
          <p>按 F 玩贪吃蛇，每颗豆子都加分，但屏幕的光老远就能看见。被抓住，手机没收。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">🕯️</div>
          <h4>停电晚自习</h4>
          <p>灯灭了，她的手电筒会扫过每一排。光柱照到你时，咀嚼、手机都会立刻暴露。<br>等老师背对你写板书时，按 <kbd>R</kbd> 摸黑上讲台；按住 <kbd>W</kbd> 靠近，到了按 <kbd>Q</kbd> 刺杀——成功直接赢下这一局。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">🙋</div>
          <h4>同学会举报你</h4>
          <p>旁边的同学会举手。看到 🙋 和倒计时就按 <kbd>E</kbd> 塞零食堵嘴；没堵住，老师会立刻转向你，怀疑度大涨。<br>有时他会先讨零食：<kbd>Y</kbd> 给一口，之后帮你挡一次举报；<kbd>N</kbd> 不给，他会记仇。</p>
        </div>
        <div class="help-item">
          <div class="hi-icon">👹</div>
          <h4>被抓到最后一次</h4>
          <p>红牌用完之后，她会把你拎到讲台上。在被吃掉之前，<b>按 Q</b> 用桌兜里的美工刀反杀——成功会直接结束这节课，并进入“刺杀成功”结局。</p>
        </div>
      </div>
      <div class="help-foot">
        <span><kbd>空格</kbd> / 鼠标左键 偷吃</span>
        <span><kbd>Shift</kbd> 装乖</span>
        <span><kbd>1-5</kbd> 换零食</span>
        <span><kbd>F</kbd> 手机</span>
        <span><kbd>P</kbd> 暂停</span>
        <span><kbd>E</kbd> 堵嘴</span>
        <span><kbd>Y</kbd>/<kbd>N</kbd> 讨零食</span>
        <span><kbd>R</kbd> 摸黑 / <kbd>W</kbd> 前进</span>
        <span><kbd>Q</kbd> 反杀</span>
        <span><kbd>M</kbd> 静音</span>
        <span>🗣️ 四川话（川普）</span>
      </div>
      <div class="modal-actions">
        <button id="btnHelpStart" class="btn primary">我懂了，开始上课</button>
      </div>
    </div>
  </div>

  <div id="loading" class="loading">
    <div class="loading-spinner"></div>
    <p>正在搭建教室……</p>
  </div>
  <div id="fatal" class="fatal hidden"></div>
</div>

<script src="js/vendor/three.min.js"></script>
<script src="js/audio.js"></script>
<script src="js/phone.js"></script>
<script src="js/world.js"></script>
<script src="js/game.js"></script>
</body>
</html>

# 上课偷吃 · 暗夜版

> 在《我带过的最差一届》的“课堂偷吃”单人版基础上，加入更紧张的事：
> **停电晚自习、同学举报、被老师吃掉、被抓住时反杀她，以及——老师全程用四川话骂你。**

![暗夜版预览](preview.png)

## 🗣️ 四川话语音

老师的所有台词都重写成了四川话：

> “逮到你了。黑灯瞎火的，哪个都看不到——这回，轮到我吃你咯。”
> “你娃咋个回事嘛，我的课上就这么饿嗦？”
> “再吃一口试哈，粉笔头认得到你哈。”

游戏会优先寻找系统里的**四川话发音人**。如果系统没有（macOS 自带一般只有普通话/粤语/台湾腔），会自动做两件事：

1. 把朗读文本做川话发音替换（**啥子→撒子、是→四、吃→呲、说→梭、去→气、的→哩、了→咯、这→则**），字幕仍保留正字；
2. 压低音高、放慢语速，用女声读出“川普版”。

> 想要真正地道的成都口音，需要接讯飞 / 火山 / 豆包等带四川话发音人的在线 TTS（需要 key 或联网）。这一版先把台词、语气、发音替换和字幕做成川话；原声普通话版仍保存在 `../上课偷吃_暗夜版_普通话备份.zip`。

## 直接开玩

- **macOS**：双击 `开始游戏.command`（优先用 Chrome 打开）
- **Windows**：双击 `开始游戏.bat`
- 或者直接双击 `index.html`

推荐 Chrome / Edge，桌面端，**建议戴耳机**。点击「开始上课」后鼠标会被锁定，`Esc` 暂停。
本作含 jumpscare、闪烁和恐怖音效，胆子小的话可以把声音关小一点。

## 暗夜版新增

### 🕯️ 停电晚自习（第 4 节课）
灯突然灭了，整栋楼都是黑的。值班老师拿着一支手电筒，光柱会在教室里扫来扫去。
**光柱照到你时，别嚼、别动手机、屏住呼吸。** 在黑暗里吃零食也会被听见，但真正致命的是那束光。

### 🍜 饥饿值
每节课从 62 开始，随着课堂时间下降。每咬一口零食都能回一点饥饿值，吃得越久回得越多。

- 低于 **30**：肚子开始咕噜叫，怀疑度上涨；旁边的同学会听见。
- 低于 **12**：叫得特别响，老师很可能直接转身。
- 饥饿值到 0 不会立刻死，但会不停叫、不停涨怀疑——离被抓只差一步。

所以别只顾着吃分数：该吃的时候得吃，但吃的时候又最容易被抓。

### 🙋 同学讨零食

老师背过身写板书的时候，同桌有时会扯你袖子：

- **Y 给一口**：饥饿值 **-12**，但他会欠你一个人情——下一次有人要举报你时，他会帮你打掩护，把举报压下去一次。
- **N 不给**：他会记仇。下一次举报大概率就是他，而且倒计时更短。

![同桌讨零食](preview-classmate.png)

### 🙋 同学会举报你
你旁边的同学会注意到你在偷吃，举手打小报告。
**看到顶部 ⚠ 提示，立刻按 `E` 塞零食堵嘴。** 没堵住，老师会立刻转向你，怀疑度大涨；红牌只有一张，被举报多了就没了。

### 👹 被老师吃掉
红牌用完的那一刻，她不会让你去罚站——她会把你从座位上拎起来，带到讲台，举过头顶，然后张开嘴。
这是一段约 6 秒的第一人称处刑镜头：你被举到半空、看到她的牙、然后世界变黑。之后会看到一张“寻人启事”，可以重上这节课。

> 现在每节课只有 **1 张红牌**。第一次被抓是罚站，红牌用完之后再被发现，就直接进入“被吃掉”流程（或者按 Q 反杀）。

### 🗡️ 停电摸黑刺杀（主动赢法）

第 4 节停电晚自习，等值班老师背对你写板书时：

1. 按 **R** 从座位溜下来，摸黑上讲台；
2. 按住 **W** 前进，松开停；听到粉笔声停、她肩膀一动，立刻按 **R** 退回；
3. 摸到讲台边，按 **Q** 刺杀——**成功直接赢下这一局**。

被转身发现就是普通被抓（红牌/被吃），所以时机比手速重要。

![摸黑上讲台](preview-sneak.png)

### 🗡️ 反杀选项（被吃掉之前）
就在她把你拎起来、牙齿凑过来的时候，屏幕会出现 **反杀机会**：桌兜里还有一把美工刀。

- 在倒计时结束前按 **`Q`**：一刀刺过去，她会松手、倒下。你会进入一个全新的 **“刺杀成功”结局**，这节课直接判定通过，并额外获得 **500 分**。
- 不按 / 按慢了：继续原来的被吃掉流程。
- 反杀后可以点“下一节课”继续，或者重上这节课。

如果最终四节课都通过，且至少反杀过一位老师，学期总评会变成特殊的 **S** 结局：
> “这届学生，有点东西。”

![反杀机会与刺杀结局](preview-assassin.png)

川话台词实机：

![川话台词](preview-sichuan.png)

## 四节课

| 节次 | 老师 | 特点 |
|---|---|---|
| 数学课 | 王老师 | 节奏慢，适合上手 |
| 英语课 | 李老师 | 转身更快，开始巡视 |
| 班主任的课 | 张主任 | 5 份零食，转身极快 |
| **停电晚自习** | **值班老师** | **手电筒、举报、1 张红牌、被吃掉** |

## 操作

| 操作 | 按键 |
|---|---|
| 环顾教室 | 鼠标（指针锁定）/ 方向键 |
| 偷吃 | 按住 `空格` / 鼠标左键，松开收手 |
| 装乖 | 按住 `Shift` |
| 换零食 | `1` ~ `5` |
| 手机贪吃蛇 | `F` 开/关，`↑↓←→` 控制 |
| 饥饿值 | 自动下降；吃零食恢复；低于 30 会肚子叫 |
| 同学讨零食 | `Y` 给一口 / `N` 不给 |
| 停电摸黑刺杀 | `R` 起身 / 退回，按住 `W` 前进，到讲台按 `Q` |
| 堵住举报的同学 | `E` |
| 被抓住后反杀 | `Q`（出现“反杀机会”时） |
| 暂停 / 静音 | `Esc`、`P` / `M` |
| 老师口音 | 四川话（川普）；优先系统川话发音人 |

## 存档说明

- 旧版原样保存在：`../上课偷吃_单人版/`，压缩包：`../上课偷吃_单人版.zip`
- 这一版是独立文件夹：`上课偷吃_暗夜版/`，压缩包：`../上课偷吃_暗夜版.zip`
- 原声（普通话）暗夜版备份：`../上课偷吃_暗夜版_普通话备份.zip`
- 两版互不影响，可以分别打开玩。
- 刺杀成功的那节课也计入四节课的总评；四节全过 + 至少反杀一次 = S 结局。

## 技术

- 纯前端原生 JavaScript + **three.js r149**（本地 `js/vendor/three.min.js`）。
- 3D 教室、老师、学生、零食全部由代码和 Canvas 纹理生成，没有外部图片/模型。
- 音效由 WebAudio 实时合成，老师台词用系统 `speechSynthesis`；没有中文语音时字幕照常显示。
- 无构建、无网络请求，双击 `index.html` 就能运行。
- 用 VS Code 打开本文件夹：任务面板里有「直接在浏览器打开游戏」「本地预览服务器」「检查 JS 语法」；`F5` 有 Chrome 调试配置。

> 粉丝自制单人版，非官方作品；角色、台词、场景为原创程序化内容，仅借鉴了“上课偷吃 + 老师转身”的玩法概念。
