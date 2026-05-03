---
layout: default
title: JLSS Eligibility Checker
---

<style>
  /* ── Layout ── */
  .checker-wrap { max-width: 760px; margin: 0 auto; padding: 0 0 3rem; }

  /* ── Notice box ── */
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

  /* ── Section card ── */
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
    letter-spacing: 0.01em;
    color: #222;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .form-card-body { padding: 1rem 1.1rem; }

  /* ── Program toggle ── */
  .program-toggle {
    display: flex;
    gap: 0;
    border: 2px solid #2879d0;
    border-radius: 6px;
    overflow: hidden;
    width: fit-content;
    margin: 0.4rem 0 0.2rem;
  }
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
  .program-toggle input[type="radio"] { display: none; }
  .program-toggle input[type="radio"]:checked + label {
    background: #2879d0;
    color: #fff;
  }
  .program-toggle label:not(:last-of-type) {
    border-right: 2px solid #2879d0;
  }

  /* ── Semester block ── */
  .semester-block { margin-bottom: 1.1rem; }
  .semester-title {
    font-size: 0.82rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    color: #555;
    margin: 0 0 0.5rem;
    padding-bottom: 0.3rem;
    border-bottom: 1px dashed #ccc;
  }

  /* ── Subject row ── */
  .subject-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 0.5rem;
    padding: 0.3rem 0;
    border-bottom: 1px solid #f0f0f0;
  }
  .subject-row:last-child { border-bottom: none; }
  .subject-label { font-size: 0.88rem; flex: 1; min-width: 0; line-height: 1.3; }
  .subject-code { font-size: 0.75rem; color: #777; display: block; }
  .grade-select {
    font-size: 0.84rem;
    padding: 0.28rem 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    background: #fff;
    color: #333;
    min-width: 160px;
    flex-shrink: 0;
    cursor: pointer;
  }
  .grade-select.has-issue { border-color: #d9534f; background: #fff5f5; }
  .grade-select.is-pass   { border-color: #5cb85c; }

  /* ── GWA row ── */
  .gwa-row {
    display: flex;
    align-items: center;
    gap: 0.8rem;
    flex-wrap: wrap;
  }
  .gwa-row label { font-weight: 600; font-size: 0.92rem; }
  .gwa-input {
    width: 90px;
    padding: 0.35rem 0.55rem;
    border: 2px solid #ccc;
    border-radius: 5px;
    font-size: 1rem;
    font-weight: 700;
    text-align: center;
  }
  .gwa-input.ok  { border-color: #5cb85c; color: #3a7a3a; }
  .gwa-input.bad { border-color: #d9534f; color: #c0392b; }
  .gwa-hint { font-size: 0.8rem; color: #777; }

  /* ── Yes / No radio ── */
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
  .sub-question label { font-weight: 600; font-size: 0.9rem; }

  /* ── Check button ── */
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

  /* ── Result box ── */
  .result-box {
    margin-top: 1.6rem;
    border-radius: 6px;
    padding: 1.1rem 1.3rem;
    display: none;
    animation: fadeIn 0.25s ease;
  }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }
  .result-box.eligible   { background: #eafaf1; border: 2px solid #27ae60; }
  .result-box.ineligible { background: #fdf2f2; border: 2px solid #d9534f; }
  .result-box.visible    { display: block; }
  .result-title { font-size: 1.1rem; font-weight: 700; margin: 0 0 0.5rem; }
  .result-box.eligible   .result-title { color: #1a7a45; }
  .result-box.ineligible .result-title { color: #b03030; }
  .result-reasons { margin: 0; padding-left: 1.3rem; font-size: 0.9rem; line-height: 1.7; }
  .result-reasons li { margin-bottom: 0.2rem; }
  .result-note { font-size: 0.8rem; color: #666; margin-top: 0.7rem; border-top: 1px solid rgba(0,0,0,0.08); padding-top: 0.7rem; }

  /* ── Validation errors ── */
  .error-msg { color: #d9534f; font-size: 0.8rem; margin-top: 0.3rem; display: none; }
  .error-msg.visible { display: block; }

  @media (max-width: 540px) {
    .subject-row { flex-direction: column; align-items: flex-start; gap: 0.25rem; }
    .grade-select { min-width: 100%; }
    .gwa-input { width: 80px; }
  }
</style>

<div class="checker-wrap">

  <div class="notice">
    <strong>ℹ️ Note:</strong> This checker assumes you are a <strong>Regular 2nd Year BSIT or BSCS student</strong> enrolled in a <strong>State University or College (SUC)</strong>. If you study at a <strong>Private Higher Education Institution (HEI)</strong> or a <strong>Local University and College (LUC)</strong>, please verify your eligibility directly at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a>.
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
    <div class="form-card-header">② Academic Record</div>
    <div class="form-card-body">

      <!-- GWA input -->
      <div class="gwa-row" style="margin-bottom:1rem;">
        <label for="gwa-input">General Weighted Average (GWA):</label>
        <input type="number" id="gwa-input" class="gwa-input" min="0" max="100" step="0.01" placeholder="e.g. 88">
        <span class="gwa-hint">Must be <strong>83% or higher</strong></span>
      </div>
      <div class="error-msg" id="err-gwa">Please enter your GWA (0–100).</div>

      <hr style="border:none;border-top:1px solid #eee;margin:0.8rem 0;">
      <p style="margin:0 0 0.8rem;font-size:0.88rem;color:#444;">
        For each subject below, select whether it was <strong>Passed</strong>, or if it has a <strong>conditional / failing mark</strong>.
        A failing or conditional mark is: a grade <em>below 3.0</em>, <em>INC</em> (Incomplete), or <em>W</em> (Withdrawn).
      </p>

      <!-- Subject list — rendered by JS -->
      <div id="subject-list">
        <p style="color:#888;font-size:0.9rem;font-style:italic;">← Select your program above to load subjects.</p>
      </div>
      <div class="error-msg" id="err-subjects">Please select a grade status for every subject.</div>

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

  <!-- Result -->
  <div class="result-box" id="result-box">
    <div class="result-title" id="result-title"></div>
    <ul class="result-reasons" id="result-reasons"></ul>
    <div class="result-note" id="result-note"></div>
  </div>

</div><!-- /.checker-wrap -->

<script>
// ── Subject data ────────────────────────────────────────────────────────────
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
        { code: 'NSTP 1',   name: 'NSTP 1' },
        { code: 'PATHFIT 1',name: 'PATHFIT 1' },
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
        { code: 'NSTP 2',   name: 'NSTP 2' },
        { code: 'PATHFIT 2',name: 'PATHFIT 2' },
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
        { code: 'PATHFIT 3',name: 'PATHFIT 3' },
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
        { code: 'PATHFIT 1',name: 'PATHFIT 1' },
        { code: 'NSTP 1',   name: 'NSTP 1' },
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
        { code: 'PATHFIT 2',name: 'PATHFIT 2' },
        { code: 'NSTP 2',   name: 'NSTP 2' },
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
        { code: 'PATHFIT 3',name: 'PATHFIT 3' },
      ]
    }
  ]
};

// ── Render subjects ──────────────────────────────────────────────────────────
function renderSubjects(program) {
  const container = document.getElementById('subject-list');
  if (!program) { container.innerHTML = '<p style="color:#888;font-size:0.9rem;font-style:italic;">← Select your program above to load subjects.</p>'; return; }

  let html = '';
  SUBJECTS[program].forEach(sem => {
    html += `<div class="semester-block"><p class="semester-title">${sem.label}</p>`;
    sem.courses.forEach(course => {
      const id = 'g-' + course.code.replace(/\s/g, '_');
      html += `
        <div class="subject-row">
          <div class="subject-label">
            <span class="subject-code">${course.code}</span>
            ${course.name}
          </div>
          <select class="grade-select" id="${id}" onchange="styleGradeSelect(this)">
            <option value="">— Select —</option>
            <option value="pass">✓ Passed</option>
            <option value="conditional">⚠ Below 3.0 / Conditional</option>
            <option value="inc">✗ INC (Incomplete)</option>
            <option value="w">✗ W (Withdrawn)</option>
          </select>
        </div>`;
    });
    html += '</div>';
  });
  container.innerHTML = html;
}

function styleGradeSelect(sel) {
  sel.classList.remove('has-issue', 'is-pass');
  if (sel.value === 'pass') sel.classList.add('is-pass');
  else if (sel.value && sel.value !== '') sel.classList.add('has-issue');
}

// ── Program toggle ───────────────────────────────────────────────────────────
document.querySelectorAll('input[name="program"]').forEach(radio => {
  radio.addEventListener('change', () => {
    renderSubjects(radio.value);
    hideError('err-program');
  });
});

// ── DOST conditional block ───────────────────────────────────────────────────
document.querySelectorAll('input[name="dost-applied"]').forEach(radio => {
  radio.addEventListener('change', () => {
    const block = document.getElementById('dost-qualify-block');
    if (radio.value === 'yes') block.classList.add('visible');
    else block.classList.remove('visible');
    hideError('err-dost');
  });
});

document.querySelectorAll('input[name="dost-qualified"]').forEach(r => {
  r.addEventListener('change', () => hideError('err-dost-qualify'));
});
document.querySelectorAll('input[name="citizen"]').forEach(r => {
  r.addEventListener('change', () => hideError('err-citizen'));
});
document.getElementById('gwa-input').addEventListener('input', function() {
  hideError('err-gwa');
  this.classList.remove('ok','bad');
  const v = parseFloat(this.value);
  if (!isNaN(v)) this.classList.add(v >= 83 ? 'ok' : 'bad');
});

// ── Helpers ──────────────────────────────────────────────────────────────────
function showError(id) { document.getElementById(id).classList.add('visible'); }
function hideError(id) { document.getElementById(id).classList.remove('visible'); }

function getRadio(name) {
  const el = document.querySelector(`input[name="${name}"]:checked`);
  return el ? el.value : null;
}

// ── Main check ───────────────────────────────────────────────────────────────
function checkEligibility() {
  let valid = true;

  // Validate program
  const program = getRadio('program');
  if (!program) { showError('err-program'); valid = false; }
  else hideError('err-program');

  // Validate GWA
  const gwaRaw = document.getElementById('gwa-input').value.trim();
  const gwa = parseFloat(gwaRaw);
  if (!gwaRaw || isNaN(gwa) || gwa < 0 || gwa > 100) { showError('err-gwa'); valid = false; }
  else hideError('err-gwa');

  // Validate subject selects
  let allSubjectsSelected = true;
  let failingSubjects = [];
  if (program) {
    SUBJECTS[program].forEach(sem => {
      sem.courses.forEach(course => {
        const id = 'g-' + course.code.replace(/\s/g, '_');
        const el = document.getElementById(id);
        if (!el) return;
        if (!el.value) { allSubjectsSelected = false; }
        else if (el.value !== 'pass') {
          const labelMap = { conditional: 'Below 3.0/Conditional', inc: 'INC', w: 'W' };
          failingSubjects.push(`${course.code} – ${course.name} (${labelMap[el.value] || el.value})`);
        }
      });
    });
    if (!allSubjectsSelected) { showError('err-subjects'); valid = false; }
    else hideError('err-subjects');
  }

  // Validate citizen
  const citizen = getRadio('citizen');
  if (!citizen) { showError('err-citizen'); valid = false; }
  else hideError('err-citizen');

  // Validate DOST
  const dostApplied = getRadio('dost-applied');
  if (!dostApplied) { showError('err-dost'); valid = false; }
  else hideError('err-dost');

  let dostQualified = null;
  if (dostApplied === 'yes') {
    dostQualified = getRadio('dost-qualified');
    if (!dostQualified) { showError('err-dost-qualify'); valid = false; }
    else hideError('err-dost-qualify');
  }

  if (!valid) return;

  // ── Eligibility logic ────────────────────────────────────────────────────
  const reasons = [];

  if (citizen === 'no') {
    reasons.push('You must be a <strong>Filipino citizen</strong> to qualify.');
  }
  if (!isNaN(gwa) && gwa < 83) {
    reasons.push(`Your GWA of <strong>${gwa.toFixed(2)}%</strong> is below the required <strong>83%</strong>.`);
  }
  if (failingSubjects.length > 0) {
    reasons.push('You have <strong>failing or conditional marks</strong> in the following subject(s):<ul style="margin:0.3rem 0 0 1rem;font-size:0.85rem;">' + failingSubjects.map(s => `<li>${s}</li>`).join('') + '</ul>');
  }
  if (dostQualified === 'yes') {
    reasons.push('You <strong>qualified</strong> for a DOST-SEI scholarship — JLSS applicants must not be existing DOST-SEI qualifiers.');
  }

  // ── Display result ───────────────────────────────────────────────────────
  const box   = document.getElementById('result-box');
  const title = document.getElementById('result-title');
  const list  = document.getElementById('result-reasons');
  const note  = document.getElementById('result-note');

  box.className = 'result-box visible';
  list.innerHTML = '';

  if (reasons.length === 0) {
    box.classList.add('eligible');
    title.textContent = '✅ You appear to be eligible to apply for JLSS!';
    list.innerHTML = '<li>Meets GWA requirement (≥ 83%)</li><li>No failing or conditional marks</li><li>Filipino citizen</li><li>No disqualifying DOST-SEI qualification</li>';
    note.innerHTML = 'This result is based on the information you provided. Please verify your eligibility and complete requirements at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a> before applying.';
  } else {
    box.classList.add('ineligible');
    title.textContent = '❌ You do not appear to be eligible for JLSS.';
    reasons.forEach(r => {
      const li = document.createElement('li');
      li.innerHTML = r;
      list.appendChild(li);
    });
    note.innerHTML = 'This checker is a guide only. If you believe there is an error, please verify directly at <a href="https://jlss.science-scholarships.ph/" target="_blank">jlss.science-scholarships.ph</a>.';
  }

  box.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
}
</script>
