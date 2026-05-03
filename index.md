---
layout: default
title: JLSS Eligibility Checker
---

<style>
  .checker-wrap { max-width: 780px; margin: 0 auto; padding: 0 0 3rem; }

  .notice {
    background: #fffbea;
    border-left: 4px solid #e6a817;
    border-radius: 4px;
    padding: 0.9rem 1.1rem;
    font-size: 0.92rem;
    margin-bottom: 1.6rem;
    line-height: 1.55;
  }
  .notice a { color: #1a6bbf; }

  .form-card {
    border: 1px solid #dde3eb;
    border-radius: 6px;
    margin-bottom: 1.4rem;
    overflow: hidden;
  }
  .form-card-header {
    background: #f4f6f9;
    border-bottom: 1px solid #dde3eb;
    padding: 0.65rem 1rem;
    font-weight: 700;
    font-size: 0.95rem;
    color: #222;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .form-card-body { padding: 1rem 1.1rem; }

  .program-toggle {
    display: flex;
    border: 2px solid #2879d0;
    border-radius: 6px;
    overflow: hidden;
    width: fit-content;
    margin: 0.4rem 0 0.2rem;
  }
  .program-toggle input[type="radio"] { display: none; }
  .program-toggle label {
    padding: 0.55rem 1.4rem;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.92rem;
    background: #fff;
    color: #2879d0;
    transition: background 0.15s, color 0.15s;
    user-select: none;
  }
  .program-toggle label:not(:last-of-type) { border-right: 2px solid #2879d0; }
  .program-toggle input[type="radio"]:checked + label { background: #2879d0; color: #fff; }

  .semester-block { margin-bottom: 1.2rem; }
  .semester-title {
    font-size: 0.8rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: #555;
    margin: 0 0 0.5rem;
    padding-bottom: 0.3rem;
    border-bottom: 1px dashed #ccc;
  }

  .subject-row {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.35rem 0;
    border-bottom: 1px solid #f2f2f2;
  }
  .subject-row:last-child { border-bottom: none; }
  .subject-label { flex: 1; min-width: 0; font-size: 0.88rem; line-height: 1.3; }
  .subject-code { font-size: 0.74rem; color: #777; display: block; }

  .grade-input {
    width: 72px;
    flex-shrink: 0;
    padding: 0.28rem 0.4rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 0.88rem;
    text-align: center;
    font-weight: 600;
    transition: border-color 0.15s;
  }
  .grade-input:focus { outline: none; border-color: #2879d0; }
  .grade-input.ok  { border-color: #5cb85c; color: #2d7a2d; background: #f5fff5; }
  .grade-input.bad { border-color: #d9534f; color: #b03030; background: #fff5f5; }

  .mark-select {
    font-size: 0.82rem;
    padding: 0.28rem 0.4rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    background: #fff;
    color: #333;
    flex-shrink: 0;
    cursor: pointer;
  }
  .mark-select.has-issue { border-color: #d9534f; background: #fff5f5; color: #b03030; }

  .gwa-display {
    display: none;
    align-items: center;
    gap: 1rem;
    background: #f7f9fc;
    border: 1px solid #dde3eb;
    border-radius: 6px;
    padding: 0.75rem 1rem;
    margin-bottom: 1.4rem;
    flex-wrap: wrap;
  }
  .gwa-label { font-weight: 700; font-size: 0.92rem; white-space: nowrap; }
  .gwa-value {
    font-size: 1.5rem;
    font-weight: 700;
    font-variant-numeric: tabular-nums;
    color: #aaa;
    min-width: 90px;
  }
  .gwa-value.ok  { color: #27ae60; }
  .gwa-value.bad { color: #d9534f; }
  .gwa-bar-wrap { flex: 1; min-width: 160px; }
  .gwa-bar-bg {
    height: 8px;
    background: #e2e8f0;
    border-radius: 99px;
    overflow: hidden;
  }
  .gwa-bar-fill {
    height: 100%;
    width: 0%;
    border-radius: 99px;
    background: #aaa;
    transition: width 0.25s, background 0.25s;
  }
  .gwa-bar-fill.ok  { background: #27ae60; }
  .gwa-bar-fill.bad { background: #d9534f; }
  .gwa-threshold-note { font-size: 0.78rem; color: #666; margin-top: 0.25rem; }

  .yn-group { display: flex; gap: 1rem; flex-wrap: wrap; margin-top: 0.4rem; }
  .yn-opt { display: flex; align-items: center; gap: 0.35rem; cursor: pointer; font-size: 0.92rem; }
  .yn-opt input[type="radio"] { accent-color: #2879d0; width: 16px; height: 16px; cursor: pointer; }

  .sub-question {
    margin-top: 0.8rem;
    padding: 0.8rem 1rem;
    background: #f7f9fc;
    border-left: 3px solid #2879d0;
    border-radius: 0 4px 4px 0;
    display: none;
  }
  .sub-question.visible { display: block; }
  .sub-question > label { font-weight: 600; font-size: 0.9rem; }

  .check-btn {
    display: block;
    width: 100%;
    padding: 0.85rem;
    background: #2879d0;
    color: #fff;
    border: none;
    border-radius: 6px;
    font-size: 1.05rem;
    font-weight: 700;
    cursor: pointer;
    letter-spacing: 0.02em;
    transition: background 0.15s;
    margin-top: 0.5rem;
  }
  .check-btn:hover { background: #1d5fa8; }

  .result-box {
    margin-top: 1.6rem;
    border-radius: 6px;
    padding: 1.1rem 1.3rem;
    display: none;
    animation: fadeIn 0.25s ease;
  }
  @keyframes fadeIn { from { opacity:0; transform:translateY(6px); } to { opacity:1; transform:none; } }
  .result-box.eligible   { background: #eafaf1; border: 2px solid #27ae60; }
  .result-box.ineligible { background: #fdf2f2; border: 2px solid #d9534f; }
  .result-box.visible    { display: block; }
  .result-title { font-size: 1.1rem; font-weight: 700; margin: 0 0 0.5rem; }
  .result-box.eligible   .result-title { color: #1a7a45; }
  .result-box.ineligible .result-title { color: #b03030; }
  .result-reasons { margin: 0; padding-left: 1.3rem; font-size: 0.9rem; line-height: 1.7; }
  .result-reasons li { margin-bottom: 0.2rem; }
  .result-note { font-size: 0.8rem; color: #666; margin-top: 0.7rem; border-top: 1px solid rgba(0,0,0,0.08); padding-top: 0.7rem; }

  .error-msg { color: #d9534f; font-size: 0.8rem; margin-top: 0.35rem; display: none; }
  .error-msg.visible { display: block; }

  @media (max-width: 560px) {
    .subject-row { flex-wrap: wrap; }
    .subject-label { width: 100%; }
    .grade-input, .mark-select { flex: 1; }
    .gwa-value { font-size: 1.2rem; }
  }
</style>

<div class="checker-wrap">

  <div class="notice">
    <strong>ℹ️ Note:</strong> This checker assumes you are a <strong>Regular 2nd Year BSIT or BSCS student</strong> enrolled in a <strong>State University or College (SUC)</strong>. If you study at a <strong>Private HEI</strong> or <strong>Local University and College (LUC)</strong>, verify eligibility directly at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a>.
  </div>

  <!-- ① Program -->
  <div class="form-card">
    <div class="form-card-header">① Your Program</div>
    <div class="form-card-body">
      <p style="margin:0 0 0.6rem;font-size:0.9rem;">Select your degree program:</p>
      <div class="program-toggle">
        <input type="radio" id="prog-cs" name="program" value="CS">
        <label for="prog-cs">BS Computer Science</label>
        <input type="radio" id="prog-it" name="program" value="IT">
        <label for="prog-it">BS Information Technology</label>
      </div>
      <div class="error-msg" id="err-program">Please select your program.</div>
    </div>
  </div>

  <!-- ② Grades -->
  <div class="form-card">
    <div class="form-card-header">② Grades (1st Year to Current Semester)</div>
    <div class="form-card-body">
      <p style="margin:0 0 0.8rem;font-size:0.88rem;color:#444;">
        Enter your percentage grade (0–100) for each subject. Use the dropdown to mark a subject
        as <strong>INC</strong> or <strong>W</strong> instead of entering a number.
        A grade below <strong>75%</strong>, or a mark of <em>INC</em> or <em>W</em>, is treated as a failing or conditional mark.
      </p>
      <div id="subject-list">
        <p style="color:#888;font-size:0.9rem;font-style:italic;">← Select your program above to load subjects.</p>
      </div>
      <div class="error-msg" id="err-subjects">Please fill in a grade for every subject.</div>
    </div>
  </div>

  <!-- Live GWA -->
  <div class="gwa-display" id="gwa-display">
    <div class="gwa-label">Computed GWA:</div>
    <div class="gwa-value" id="gwa-value">—</div>
    <div class="gwa-bar-wrap">
      <div class="gwa-bar-bg"><div class="gwa-bar-fill" id="gwa-bar"></div></div>
      <div class="gwa-threshold-note">Minimum required: <strong>83.00%</strong></div>
    </div>
  </div>

  <!-- ③ Citizenship -->
  <div class="form-card">
    <div class="form-card-header">③ Citizenship</div>
    <div class="form-card-body">
      <label style="font-weight:600;font-size:0.92rem;">Are you a Filipino citizen?</label>
      <div class="yn-group">
        <label class="yn-opt"><input type="radio" name="citizen" value="yes"> Yes</label>
        <label class="yn-opt"><input type="radio" name="citizen" value="no">  No</label>
      </div>
      <div class="error-msg" id="err-citizen">Please answer this question.</div>
    </div>
  </div>

  <!-- ④ DOST-SEI -->
  <div class="form-card">
    <div class="form-card-header">④ DOST-SEI Application</div>
    <div class="form-card-body">
      <label style="font-weight:600;font-size:0.92rem;">Did you previously apply for a DOST-SEI scholarship?</label>
      <div class="yn-group">
        <label class="yn-opt"><input type="radio" name="dost-applied" value="yes"> Yes</label>
        <label class="yn-opt"><input type="radio" name="dost-applied" value="no">  No</label>
      </div>
      <div class="error-msg" id="err-dost">Please answer this question.</div>

      <div class="sub-question" id="dost-qualify-block">
        <label style="font-weight:600;font-size:0.9rem;">Did you <strong>qualify</strong> for that DOST-SEI scholarship?</label>
        <div class="yn-group" style="margin-top:0.4rem;">
          <label class="yn-opt"><input type="radio" name="dost-qualified" value="yes"> Yes, I qualified</label>
          <label class="yn-opt"><input type="radio" name="dost-qualified" value="no">  No, I did not qualify</label>
        </div>
        <div class="error-msg" id="err-dost-qualify">Please answer this question.</div>
      </div>
    </div>
  </div>

  <button class="check-btn" onclick="checkEligibility()">Check My Eligibility →</button>

  <div class="result-box" id="result-box">
    <div class="result-title" id="result-title"></div>
    <ul class="result-reasons" id="result-reasons"></ul>
    <div class="result-note" id="result-note"></div>
  </div>

</div>

<script>
const SUBJECTS = {
  CS: [
    {
      label: '1st Year, 1st Semester',
      courses: [
        { code: 'COMP 001', name: 'Introduction to Computing' },
        { code: 'COMP 002', name: 'Computer Programming 1' },
        { code: 'GEED 004', name: 'Mathematics in the Modern World' },
        { code: 'GEED 005', name: 'Purposive Communication' },
        { code: 'GEED 020', name: 'Politics, Governance and Citizenship' },
        { code: 'GEED 032', name: 'Filipinolohiya' },
        { code: 'NSTP 1',    name: 'NSTP 1' },
        { code: 'PATHFIT 1', name: 'PATHFIT 1' },
      ]
    },
    {
      label: '1st Year, 2nd Semester',
      courses: [
        { code: 'COMP 003', name: 'Computer Programming 2' },
        { code: 'COMP 004', name: 'Discrete Structures 1' },
        { code: 'GEED 001', name: 'Understanding the Self' },
        { code: 'GEED 007', name: 'Science, Technology and Society' },
        { code: 'GEED 033', name: 'Pagsasalin sa Kontekstong Filipino' },
        { code: 'MATH 017', name: 'Differential Calculus' },
        { code: 'NSTP 2',    name: 'NSTP 2' },
        { code: 'PATHFIT 2', name: 'PATHFIT 2' },
      ]
    },
    {
      label: '2nd Year, 1st Semester',
      courses: [
        { code: 'COMP 005', name: 'Discrete Structures 2' },
        { code: 'COMP 006', name: 'Data Structures & Algorithms' },
        { code: 'COMP 009', name: 'Object Oriented Programming' },
        { code: 'COSC 201', name: 'Logic Design & Digital Computer Circuits' },
        { code: 'COSC 202', name: 'Modeling and Simulation' },
        { code: 'MATH 018', name: 'Linear Algebra' },
        { code: 'GEED 008', name: 'Ethics' },
        { code: 'PATHFIT 3', name: 'PATHFIT 3' },
      ]
    }
  ],
  IT: [
    {
      label: '1st Year, 1st Semester',
      courses: [
        { code: 'COMP 001', name: 'Introduction to Computing' },
        { code: 'COMP 002', name: 'Computer Programming 1' },
        { code: 'ACCO 014', name: 'Principles of Accounting' },
        { code: 'GEED 004', name: 'Mathematics in the Modern World' },
        { code: 'GEED 005', name: 'Purposive Communication' },
        { code: 'GEED 032', name: 'Filipinolohiya' },
        { code: 'PATHFIT 1', name: 'PATHFIT 1' },
        { code: 'NSTP 1',    name: 'NSTP 1' },
      ]
    },
    {
      label: '1st Year, 2nd Semester',
      courses: [
        { code: 'COMP 003', name: 'Computer Programming 2' },
        { code: 'COMP 004', name: 'Discrete Structures 1' },
        { code: 'GEED 002', name: 'Readings in Philippine History' },
        { code: 'GEED 010', name: "People and the Earth's Ecosystems" },
        { code: 'GEED 020', name: 'Politics and Governance' },
        { code: 'GEED 033', name: 'Pagsasalin sa Kontekstong Filipino' },
        { code: 'PATHFIT 2', name: 'PATHFIT 2' },
        { code: 'NSTP 2',    name: 'NSTP 2' },
      ]
    },
    {
      label: '2nd Year, 1st Semester',
      courses: [
        { code: 'COMP 006', name: 'Data Structures and Algorithms' },
        { code: 'COMP 007', name: 'Operating Systems' },
        { code: 'COMP 008', name: 'Data Communications and Networking' },
        { code: 'MATH 018', name: 'Linear Algebra' },
        { code: 'GEED 001', name: 'Understanding the Self' },
        { code: 'GEED 028', name: 'Reading Visual Arts' },
        { code: 'INTE 201', name: 'Programming 3 (Structured Programming)' },
        { code: 'PATHFIT 3', name: 'PATHFIT 3' },
      ]
    }
  ]
};

const PASSING = 75;

// ── Render subject list ───────────────────────────────────────────────────────
function renderSubjects(program) {
  const container = document.getElementById('subject-list');
  const gwaPanel  = document.getElementById('gwa-display');

  if (!program) {
    container.innerHTML = '<p style="color:#888;font-size:0.9rem;font-style:italic;">← Select your program above to load subjects.</p>';
    gwaPanel.style.display = 'none';
    return;
  }

  let html = '';
  SUBJECTS[program].forEach(sem => {
    html += `<div class="semester-block"><p class="semester-title">${sem.label}</p>`;
    sem.courses.forEach(({ code, name }) => {
      const id = 'g-' + code.replace(/[\s.]/g, '_');
      html += `
        <div class="subject-row">
          <div class="subject-label">
            <span class="subject-code">${code}</span>${name}
          </div>
          <input type="number" class="grade-input" id="${id}"
            min="0" max="100" step="0.01" placeholder="—"
            oninput="onGradeInput(this)"
            data-code="${code}" data-name="${name.replace(/"/g, '&quot;')}">
          <select class="mark-select" id="${id}-mark" onchange="onMarkChange(this,'${id}')">
            <option value="normal">Normal</option>
            <option value="inc">INC</option>
            <option value="w">W</option>
          </select>
        </div>`;
    });
    html += '</div>';
  });

  container.innerHTML = html;
  gwaPanel.style.display = 'flex';
  updateGWA();
}

// ── Grade input / mark handlers ───────────────────────────────────────────────
function onGradeInput(input) {
  if (input.value !== '') document.getElementById(input.id + '-mark').value = 'normal';
  styleGrade(input);
  updateGWA();
  hideError('err-subjects');
}

function onMarkChange(sel, gradeId) {
  const input = document.getElementById(gradeId);
  if (sel.value !== 'normal') {
    input.value = '';
    input.classList.remove('ok', 'bad');
    sel.classList.add('has-issue');
  } else {
    sel.classList.remove('has-issue');
    styleGrade(input);
  }
  updateGWA();
}

function styleGrade(input) {
  input.classList.remove('ok', 'bad');
  const v = parseFloat(input.value);
  if (input.value === '' || isNaN(v)) return;
  input.classList.add(v >= PASSING ? 'ok' : 'bad');
}

// ── Live GWA ──────────────────────────────────────────────────────────────────
function updateGWA() {
  const program = getRadio('program');
  if (!program) return;

  const rows = collectGrades(program);
  const numeric = rows.filter(r => r.mark === 'normal' && r.grade !== null);

  if (numeric.length === 0) { paintGWA(null); return; }

  const gwa = numeric.reduce((s, r) => s + r.grade, 0) / numeric.length;
  paintGWA(gwa);
}

function paintGWA(gwa) {
  const valEl = document.getElementById('gwa-value');
  const barEl = document.getElementById('gwa-bar');
  if (gwa === null) {
    valEl.textContent = '—';
    valEl.className = 'gwa-value';
    barEl.style.width = '0%';
    barEl.className = 'gwa-bar-fill';
    return;
  }
  const ok = gwa >= 83;
  valEl.textContent = gwa.toFixed(2) + '%';
  valEl.className = 'gwa-value ' + (ok ? 'ok' : 'bad');
  barEl.style.width = Math.min(gwa, 100) + '%';
  barEl.className = 'gwa-bar-fill ' + (ok ? 'ok' : 'bad');
}

// ── Collect grades ────────────────────────────────────────────────────────────
function collectGrades(program) {
  const rows = [];
  SUBJECTS[program].forEach(sem => {
    sem.courses.forEach(({ code, name }) => {
      const id    = 'g-' + code.replace(/[\s.]/g, '_');
      const input = document.getElementById(id);
      const sel   = document.getElementById(id + '-mark');
      if (!input) return;
      rows.push({
        code, name,
        grade: input.value !== '' ? parseFloat(input.value) : null,
        mark:  sel ? sel.value : 'normal'
      });
    });
  });
  return rows;
}

// ── Wiring ────────────────────────────────────────────────────────────────────
document.querySelectorAll('input[name="program"]').forEach(r =>
  r.addEventListener('change', () => { renderSubjects(r.value); hideError('err-program'); })
);
document.querySelectorAll('input[name="dost-applied"]').forEach(r =>
  r.addEventListener('change', () => {
    document.getElementById('dost-qualify-block').classList.toggle('visible', r.value === 'yes');
    hideError('err-dost');
  })
);
['dost-qualified','citizen'].forEach(name =>
  document.querySelectorAll(`input[name="${name}"]`).forEach(r =>
    r.addEventListener('change', () => hideError('err-' + name.replace('-','_').replace('dost_qualified','dost-qualify')))
  )
);
document.querySelectorAll('input[name="dost-qualified"]').forEach(r =>
  r.addEventListener('change', () => hideError('err-dost-qualify'))
);
document.querySelectorAll('input[name="citizen"]').forEach(r =>
  r.addEventListener('change', () => hideError('err-citizen'))
);

function showError(id) { document.getElementById(id).classList.add('visible'); }
function hideError(id)  { const el = document.getElementById(id); if (el) el.classList.remove('visible'); }
function getRadio(name) { const el = document.querySelector(`input[name="${name}"]:checked`); return el ? el.value : null; }

// ── Check eligibility ─────────────────────────────────────────────────────────
function checkEligibility() {
  let valid = true;

  const program = getRadio('program');
  if (!program) { showError('err-program'); valid = false; } else hideError('err-program');

  let allFilled = true, failingList = [], gwa = null;

  if (program) {
    const rows = collectGrades(program);
    const numericGrades = [];

    rows.forEach(r => {
      if (r.mark !== 'normal') {
        failingList.push(`${r.code} – ${r.name} (${r.mark.toUpperCase()})`);
        return;
      }
      if (r.grade === null) { allFilled = false; return; }
      if (r.grade < PASSING) failingList.push(`${r.code} – ${r.name} (${r.grade.toFixed(2)}%)`);
      numericGrades.push(r.grade);
    });

    if (!allFilled) { showError('err-subjects'); valid = false; } else hideError('err-subjects');
    if (numericGrades.length) gwa = numericGrades.reduce((s, v) => s + v, 0) / numericGrades.length;
  }

  const citizen     = getRadio('citizen');
  if (!citizen)     { showError('err-citizen'); valid = false; } else hideError('err-citizen');

  const dostApplied = getRadio('dost-applied');
  if (!dostApplied) { showError('err-dost'); valid = false; }   else hideError('err-dost');

  let dostQualified = null;
  if (dostApplied === 'yes') {
    dostQualified = getRadio('dost-qualified');
    if (!dostQualified) { showError('err-dost-qualify'); valid = false; } else hideError('err-dost-qualify');
  }

  if (!valid) return;

  const reasons = [];

  if (citizen === 'no')
    reasons.push('You must be a <strong>Filipino citizen</strong> to qualify.');

  if (gwa !== null && gwa < 83)
    reasons.push(`Your computed GWA of <strong>${gwa.toFixed(2)}%</strong> is below the required <strong>83.00%</strong>.`);

  if (failingList.length)
    reasons.push(
      'You have <strong>failing or conditional marks</strong> in the following subject(s):' +
      '<ul style="margin:0.3rem 0 0 1rem;font-size:0.85rem;">' +
      failingList.map(s => `<li>${s}</li>`).join('') + '</ul>'
    );

  if (dostQualified === 'yes')
    reasons.push('You <strong>qualified</strong> for a DOST-SEI scholarship — JLSS applicants must not already hold a DOST-SEI qualification.');

  const box   = document.getElementById('result-box');
  const title = document.getElementById('result-title');
  const list  = document.getElementById('result-reasons');
  const note  = document.getElementById('result-note');

  box.className = 'result-box visible';
  list.innerHTML = '';

  if (!reasons.length) {
    box.classList.add('eligible');
    title.textContent = '✅ You appear to be eligible to apply for JLSS!';
    list.innerHTML =
      `<li>GWA of <strong>${gwa !== null ? gwa.toFixed(2)+'%' : 'N/A'}</strong> meets the 83% minimum</li>` +
      '<li>No failing or conditional marks</li>' +
      '<li>Filipino citizen</li>' +
      '<li>No disqualifying DOST-SEI qualification</li>';
    note.innerHTML = 'This result is based on the information you provided. Verify all requirements at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a> before applying.';
  } else {
    box.classList.add('ineligible');
    title.textContent = '❌ You do not appear to be eligible for JLSS.';
    reasons.forEach(r => { const li = document.createElement('li'); li.innerHTML = r; list.appendChild(li); });
    note.innerHTML = 'This checker is a guide only. Verify at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a> if you believe this is an error.';
  }

  box.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}
</script>
