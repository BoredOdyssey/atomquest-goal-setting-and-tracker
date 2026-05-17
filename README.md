# atomquest-goal-setting-and-tracker
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  HeadingLevel, AlignmentType, BorderStyle, WidthType, ShadingType,
  LevelFormat, PageBreak, ExternalHyperlink
} = require('docx');
const fs = require('fs');

const GREEN = '0D6E56', DARK_GREEN = '084D3C', BLUE = '1A4E8A', WHITE = 'FFFFFF';
const LIGHT_GREEN = 'E1F5EE', LIGHT_BLUE = 'E6F1FB', LIGHT_AMBER = 'FAEEDA', LIGHT_PURPLE = 'EEEDFE';
const BORDER_COLOR = 'E2E8F0';

const border = { style: BorderStyle.SINGLE, size: 1, color: BORDER_COLOR };
const borders = { top: border, bottom: border, left: border, right: border };
const headerBorder = { style: BorderStyle.SINGLE, size: 1, color: '1A4E8A' };
const headerBorders = { top: headerBorder, bottom: headerBorder, left: headerBorder, right: headerBorder };

function h1(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_1,
    spacing: { before: 320, after: 160 },
    children: [new TextRun({ text, bold: true, size: 32, color: GREEN, font: 'Segoe UI' })]
  });
}
function h2(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_2,
    spacing: { before: 240, after: 120 },
    children: [new TextRun({ text, bold: true, size: 26, color: BLUE, font: 'Segoe UI' })]
  });
}
function h3(text) {
  return new Paragraph({
    spacing: { before: 180, after: 80 },
    children: [new TextRun({ text, bold: true, size: 22, color: '1A1A2E', font: 'Segoe UI' })]
  });
}
function p(text, opts={}) {
  return new Paragraph({
    spacing: { before: 60, after: 60 },
    children: [new TextRun({ text, size: 20, font: 'Segoe UI', ...opts })]
  });
}
function bullet(text, bold=false) {
  return new Paragraph({
    numbering: { reference: 'bullets', level: 0 },
    spacing: { before: 40, after: 40 },
    children: [new TextRun({ text, size: 20, font: 'Segoe UI', bold })]
  });
}
function code(text) {
  return new Paragraph({
    spacing: { before: 60, after: 60 },
    indent: { left: 720 },
    children: [new TextRun({ text, size: 18, font: 'Courier New', color: '0D6E56' })]
  });
}
function divider() {
  return new Paragraph({
    spacing: { before: 160, after: 160 },
    border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: BORDER_COLOR } },
    children: []
  });
}
function space(n=1) {
  return Array.from({length:n}, () => new Paragraph({ spacing: { before: 60, after: 60 }, children: [] }));
}

function makeTable(headers, rows, colWidths) {
  const totalW = colWidths.reduce((s,w)=>s+w,0);
  return new Table({
    width: { size: totalW, type: WidthType.DXA },
    columnWidths: colWidths,
    rows: [
      new TableRow({
        tableHeader: true,
        children: headers.map((h, i) => new TableCell({
          borders: headerBorders,
          width: { size: colWidths[i], type: WidthType.DXA },
          shading: { fill: '1A4E8A', type: ShadingType.CLEAR },
          margins: { top: 80, bottom: 80, left: 120, right: 120 },
          children: [new Paragraph({ children: [new TextRun({ text: h, bold: true, color: WHITE, size: 18, font: 'Segoe UI' })] })]
        }))
      }),
      ...rows.map((row, ri) => new TableRow({
        children: row.map((cell, i) => new TableCell({
          borders,
          width: { size: colWidths[i], type: WidthType.DXA },
          shading: { fill: ri % 2 === 0 ? 'F8FAFC' : WHITE, type: ShadingType.CLEAR },
          margins: { top: 80, bottom: 80, left: 120, right: 120 },
          children: [new Paragraph({ children: [new TextRun({ text: cell, size: 18, font: 'Segoe UI' })] })]
        }))
      }))
    ]
  });
}

const doc = new Document({
  numbering: {
    config: [
      { reference: 'bullets', levels: [{ level: 0, format: LevelFormat.BULLET, text: '•',
          alignment: AlignmentType.LEFT, style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
      { reference: 'numbers', levels: [{ level: 0, format: LevelFormat.DECIMAL, text: '%1.',
          alignment: AlignmentType.LEFT, style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
    ]
  },
  styles: {
    default: { document: { run: { font: 'Segoe UI', size: 20 } } },
    paragraphStyles: [
      { id: 'Heading1', name: 'Heading 1', basedOn: 'Normal', next: 'Normal', quickFormat: true,
        run: { size: 32, bold: true, font: 'Segoe UI', color: GREEN },
        paragraph: { spacing: { before: 320, after: 160 }, outlineLevel: 0 } },
      { id: 'Heading2', name: 'Heading 2', basedOn: 'Normal', next: 'Normal', quickFormat: true,
        run: { size: 26, bold: true, font: 'Segoe UI', color: BLUE },
        paragraph: { spacing: { before: 240, after: 120 }, outlineLevel: 1 } },
    ]
  },
  sections: [{
    properties: {
      page: { size: { width: 12240, height: 15840 }, margin: { top: 1080, right: 1080, bottom: 1080, left: 1080 } }
    },
    children: [

      // ── TITLE PAGE ──────────────────────────────────────────
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { before: 1440, after: 240 },
        children: [new TextRun({ text: '⚛ AtomQuest', bold: true, size: 64, color: GREEN, font: 'Segoe UI' })]
      }),
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { before: 0, after: 120 },
        children: [new TextRun({ text: 'Goal Setting & Tracking Portal', size: 36, color: BLUE, font: 'Segoe UI' })]
      }),
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { before: 0, after: 480 },
        children: [new TextRun({ text: 'Hackathon 1.0  ·  README & Developer Guide', size: 22, color: '6B7280', font: 'Segoe UI' })]
      }),

      makeTable(
        ['Item', 'Detail'],
        [
          ['Version', '1.0.0'],
          ['Stack', 'HTML + CSS + JavaScript (Vanilla)'],
          ['Storage', 'Browser localStorage'],
          ['Hosting', 'Static — GitHub Pages / Netlify / Local'],
          ['Cost', '₹0 — Completely free'],
          ['File', 'atomquest.html (single file)'],
        ],
        [3000, 6360]
      ),

      ...space(2),
      new Paragraph({ children: [new PageBreak()] }),

      // ── OVERVIEW ────────────────────────────────────────────
      h1('1. Project Overview'),
      p('AtomQuest is a fully functional, single-file web portal for in-house employee goal setting and tracking. It covers the complete lifecycle of employee goals — from creation and manager approval to quarterly achievement logging, check-ins, and analytics — with zero infrastructure cost.'),
      ...space(1),
      p('Built using only HTML, CSS, and Vanilla JavaScript, with all data persisted in the browser\'s built-in localStorage. No server, no database, no installation required.'),

      divider(),

      // ── QUICK START ─────────────────────────────────────────
      h1('2. Quick Start'),
      h2('Option A — Local (Instant)'),
      bullet('Download atomquest.html'),
      bullet('Double-click to open in any browser'),
      bullet('That\'s it — no setup needed'),
      ...space(1),
      h2('Option B — GitHub Pages (Free Hosting)'),
      bullet('Create a new GitHub repository'),
      bullet('Upload atomquest.html and architecture.html'),
      bullet('Go to Settings → Pages → Deploy from main branch'),
      bullet('Your live URL is ready in ~60 seconds'),
      ...space(1),
      h2('Option C — Netlify (Free Hosting)'),
      bullet('Go to netlify.com → "Deploy manually"'),
      bullet('Drag and drop the atomquest.html file'),
      bullet('Live URL is generated instantly'),

      divider(),

      // ── DEMO ACCOUNTS ───────────────────────────────────────
      h1('3. Demo Accounts'),
      p('All accounts are pre-loaded. Click any row in the login page to auto-fill credentials.'),
      ...space(1),
      makeTable(
        ['Role', 'Username', 'Password', 'Access'],
        [
          ['Admin / HR', 'admin', 'admin123', 'Full access — all features, bypasses windows'],
          ['Manager (L1)', 'manager1', 'manager123', 'Team goals, approvals, check-ins, dashboard'],
          ['Employee 1', 'emp1', 'emp123', 'My goals, achievements, check-ins (Engineering)'],
          ['Employee 2', 'emp2', 'emp123', 'My goals, achievements, check-ins (Engineering)'],
        ],
        [1800, 1600, 1600, 4360]
      ),

      divider(),

      // ── FEATURES ────────────────────────────────────────────
      h1('4. Feature List'),

      h2('Phase 1 — Goal Creation & Approval'),
      makeTable(
        ['Feature', 'Status', 'Details'],
        [
          ['Goal creation (title, description, thrust area)', '✅ Done', 'Employee-facing form'],
          ['4 UoM types', '✅ Done', 'Numeric↑, Numeric↓, Timeline, Zero-based'],
          ['Target & weightage per goal', '✅ Done', 'Validated on save'],
          ['Total weightage = 100% enforcement', '✅ Done', 'Live visual bar + validation'],
          ['Min 10% per goal', '✅ Done', 'Enforced on save'],
          ['Max 8 goals per employee', '✅ Done', 'Button hidden at limit'],
          ['Manager review — edit target/weightage', '✅ Done', 'Inline in review modal'],
          ['Approve or return for rework', '✅ Done', 'With optional comment'],
          ['Goal locking on approval', '✅ Done', '🔒 badge shown'],
          ['Admin unlock with reason', '✅ Done', 'Logged in audit trail'],
          ['Shared / departmental KPI push', '✅ Done', 'Admin pushes to entire dept'],
          ['Shared goal title & target read-only', '✅ Done', 'Locked=true on push'],
        ],
        [3500, 1400, 4460]
      ),
      ...space(1),

      h2('Phase 2 — Achievement Tracking & Check-ins'),
      makeTable(
        ['Feature', 'Status', 'Details'],
        [
          ['Q1–Q4 achievement logging', '✅ Done', 'Per goal, per quarter'],
          ['Status: Not Started / On Track / Completed', '✅ Done', 'Dropdown per quarter'],
          ['Auto progress score calculation', '✅ Done', 'Formula based on UoM type'],
          ['Manager planned vs actual view', '✅ Done', 'Team check-ins page'],
          ['Structured check-in comments', '✅ Done', 'Per employee per quarter'],
          ['Quarterly window enforcement', '✅ Done', 'Open/closed banners & disabled buttons'],
          ['Achievement report table', '✅ Done', 'Admin reports page'],
          ['CSV export', '✅ Done', 'All goals × all quarters'],
          ['Audit log', '✅ Done', 'Every action logged with timestamp'],
        ],
        [3500, 1400, 4460]
      ),
      ...space(1),

      h2('Bonus Features (Section 5)'),
      makeTable(
        ['Feature', 'Status', 'Details'],
        [
          ['5.3 Escalation Module', '✅ Done', '3 configurable rules, 2-tier chain, escalation log'],
          ['5.4 Analytics — QoQ trends', '✅ Done', 'Line chart of avg scores Q1–Q4'],
          ['5.4 Analytics — Status distribution', '✅ Done', 'Donut chart'],
          ['5.4 Analytics — Thrust area breakdown', '✅ Done', 'Bar chart'],
          ['5.4 Analytics — Manager effectiveness', '✅ Done', 'Check-in rate per manager'],
          ['5.4 Analytics — Dept heatmap', '✅ Done', 'Approval % by department'],
          ['5.1 Microsoft Entra ID / SSO', '⏭ Skipped', 'Requires Azure — not free'],
          ['5.2 Email / Teams notifications', '⏭ Skipped', 'Requires email server — not free'],
        ],
        [3500, 1400, 4460]
      ),

      divider(),

      // ── UOM FORMULAS ────────────────────────────────────────
      h1('5. Score Calculation Formulas'),
      makeTable(
        ['UoM Type', 'Description', 'Formula'],
        [
          ['Numeric (↑ Higher=Better)', 'e.g. Revenue, NPS, Satisfaction %', 'Achievement ÷ Target × 100'],
          ['Numeric (↓ Lower=Better)', 'e.g. TAT, Cost, Error rate', 'Target ÷ Achievement × 100'],
          ['Timeline', 'Date-based delivery', 'Completed on/before deadline = 100%; 1% deducted per day late'],
          ['Zero-based', 'e.g. Safety incidents, Defects', 'Achievement = 0 → 100%; else 0%'],
        ],
        [2500, 2500, 4360]
      ),
      ...space(1),
      p('All scores are capped at 150% to handle over-achievement. Scores are for tracking only — not used as ratings.', { color: '6B7280', italics: true }),

      divider(),

      // ── QUARTERLY WINDOWS ───────────────────────────────────
      h1('6. Quarterly Window Schedule'),
      makeTable(
        ['Phase', 'Window Opens', 'Action'],
        [
          ['Phase 1 — Goal Setting', '1st May', 'Goal creation, submission & approval'],
          ['Q1 Check-in', 'July', 'Log Q1 achievements, manager check-in'],
          ['Q2 Check-in', 'October', 'Log Q2 achievements, manager check-in'],
          ['Q3 Check-in', 'January', 'Log Q3 achievements, manager check-in'],
          ['Q4 Check-in', 'April', 'Log Q4 achievements, manager check-in'],
        ],
        [3000, 2000, 4360]
      ),
      ...space(1),
      p('Note: Admin role bypasses all window restrictions and has full access at any time.', { color: '6B7280', italics: true }),

      divider(),

      // ── ESCALATION ──────────────────────────────────────────
      h1('7. Escalation Module'),
      p('The escalation module (Admin → Escalation) scans for 3 configurable rule violations:'),
      ...space(1),
      makeTable(
        ['Rule', 'Default Threshold', 'Escalation Level', 'Action'],
        [
          ['Employee has not submitted goals', '7 days after cycle open', 'Manager', 'Notify manager of the employee'],
          ['Goals pending manager approval', '3 days after submission', 'Manager', 'Flag overdue approvals'],
          ['Manager has not done quarterly check-in', '14 days into window', 'HR', 'Escalate to HR / skip-level'],
        ],
        [2500, 2000, 1800, 3060]
      ),
      ...space(1),
      p('All thresholds are editable. Every scan is logged in the Escalation Log with timestamp and level.'),

      divider(),

      // ── DATA STRUCTURE ──────────────────────────────────────
      h1('8. Data Structure (localStorage)'),
      makeTable(
        ['Key', 'Contents'],
        [
          ['aq_users', 'All user accounts (id, username, password, role, manager_id, department)'],
          ['aq_goals', 'All goals (id, employee_id, title, thrust, uom, target, weightage, status, locked)'],
          ['aq_achievements', 'Quarterly actuals (goal_id, quarter, actual, goal_status, score)'],
          ['aq_checkins', 'Manager check-in comments (manager_id, employee_id, quarter, comment)'],
          ['aq_audit', 'Audit log entries (goal_id, user_id, action, details, date)'],
          ['aq_esclog', 'Escalation scan history'],
          ['aq_session', 'Current logged-in user session'],
          ['aq_next_id', 'Auto-increment counters for each entity'],
        ],
        [2500, 6860]
      ),

      divider(),

      // ── ARCHITECTURE ────────────────────────────────────────
      h1('9. Architecture Summary'),
      p('See architecture.html for the full visual diagram. Summary:'),
      ...space(1),
      makeTable(
        ['Layer', 'Technology', 'Cost'],
        [
          ['Presentation', 'HTML5 + CSS3 + Vanilla JS', 'Free'],
          ['Application Logic', 'JavaScript ES6+ (client-side)', 'Free'],
          ['Charts / Visualisation', 'HTML5 Canvas API (built-in)', 'Free'],
          ['Data Storage', 'localStorage (browser-native)', 'Free'],
          ['Export', 'Blob API → CSV download', 'Free'],
          ['Hosting', 'GitHub Pages / Netlify free tier', 'Free'],
          ['Version Control', 'Git + GitHub', 'Free'],
          ['Total Cost', '—', '₹0'],
        ],
        [3000, 3500, 2860]
      ),

      divider(),

      //── SUBMISSION CHECKLIST ─────────────────────────────────
      h1('10. Submission Checklist'),
      makeTable(
        ['Item', 'Status', 'File / Link'],
        [
          ['Working portal', '✅ Ready', 'atomquest.html'],
          ['Employee user journey', '✅ Ready', 'Login: emp1 / emp123'],
          ['Manager user journey', '✅ Ready', 'Login: manager1 / manager123'],
          ['Admin user journey', '✅ Ready', 'Login: admin / admin123'],
          ['Architecture diagram', '✅ Ready', 'architecture.html'],
          ['README', '✅ Ready', 'README.docx (this file)'],
          ['Source code repository', '📋 Pending', 'Push to GitHub before deadline'],
          ['Live hosted demo URL', '📋 Pending', 'Deploy to GitHub Pages / Netlify'],
        ],
        [3000, 1600, 4760]
      ),

      divider(),

      // ── RESETTING DATA ───────────────────────────────────────
      h1('11. Resetting Demo Data'),
      p('If you need to reset all data back to the original demo state:'),
      bullet('Log in as Admin'),
      bullet('Click "Reset Demo Data" in the sidebar'),
      bullet('Confirm the prompt'),
      ...space(1),
      p('This clears all goals, achievements, check-ins, and audit entries, then re-seeds the original demo data.'),

      divider(),

      // ── FOOTER NOTE ─────────────────────────────────────────
      ...space(2),
      new Paragraph({
        alignment: AlignmentType.CENTER,
        spacing: { before: 240, after: 0 },
        children: [new TextRun({ text: 'AtomQuest · Hackathon 1.0 · Built with ❤ using free technologies', size: 18, color: '6B7280', font: 'Segoe UI' })]
      }),
    ]
  }]
});

Packer.toBuffer(doc).then(buf => {
  fs.writeFileSync('/home/claude/atomquest/README.docx', buf);
  console.log('README.docx created successfully');
});
