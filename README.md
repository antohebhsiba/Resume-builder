# Resume-builder
A simple resume builder for beginners
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Interactive Resume Builder</title>

<!-- PDF LIB -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

<style>
:root {
  --primary: #2563eb;
  --dark: #0f172a;
  --light: #ffffff;
}


body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
  background: #e5e7eb;
}

button, select {
  padding: 8px 12px;
  margin: 6px 0;
  cursor: pointer;
}


.container {
  display: grid;
  grid-template-columns: 1fr 1.6fr;
  min-height: 100vh;
}

.form {
  background: white;
  padding: 25px;
}

input, textarea {
  width: 100%;
  padding: 10px;
  margin: 6px 0;
  border-radius: 6px;
  border: 1px solid #cbd5e1;
}

.preview {
  width: 794px;
  height: 1123px;      
  margin: 40px auto;
  padding: 60px;
  background: white;
  box-sizing: border-box;
  overflow: hidden;   
}



h1 {
  margin-bottom: 6px;
}

h3 {
  margin-top: 22px;
  color: var(--primary);
}

ul {
  padding-left: 20px;
}


.template1 {
  border-left: 6px solid var(--primary);
}

.template2 {
  background: var(--dark);
  color: white;
}

.template2 h1,
.template2 h3 {
  color: #38bdf8;
}


.dark {
  background: #020617;
  color: white;
}

.dark .form {
  background: #020617;
}

.dark input,
.dark textarea {
  background: #020617;
  color: white;
  border: 1px solid #334155;
}
.blue-line {
  background: #2563eb;
  height: 100%;
  align-self: stretch;
}

@media (max-width: 900px) {
  .container {
    grid-template-columns: 1fr;
  }
}
@media print {
  body * {
    visibility: hidden;
  }

  #resume, #resume * {
    visibility: visible;
  }

  #resume {
    position: absolute;
    left: 0;
    top: 0;
  }
}
.name {
  font-size: 36px;
  font-weight: 700;
  margin: 0 0 10px 0;
  line-height: 1.2;
  position: relative;  
}

</style>
</head>

<body>

<div class="container">

<!-- FORM -->
<div class="form">
  <h2>Resume Builder</h2>

  <button onclick="toggleTheme()">🌙 / ☀️ Theme</button><br>

  <select id="template" onchange="changeTemplate()">
    <option value="template1">Classic Blue</option>
    <option value="template2">Dark Modern</option>
  </select>

  <input id="name" placeholder="Your Name">
  <input id="role" placeholder="Your Role">
  <textarea id="about" placeholder="About You"></textarea>
  <input id="skills" placeholder="Skills (comma separated)">

  <h3>Experience</h3>
  <input id="expTitle" placeholder="Job Title">
  <input id="expCompany" placeholder="Company">
  <button onclick="addExperience()">Add Experience</button>

  <br><br>
  <button onclick="downloadPDF()">📄 Download PDF</button>
</div>

<!-- PREVIEW -->
<div id="pdf-wrapper">
  <div class="preview template1" id="resume">
    <h1 id="p_name">Your Name</h1>
    <h3 id="p_role">Your Role</h3>

    <h3>About</h3>
    <p id="p_about">About you...</p>

    <h3>Skills</h3>
    <ul id="p_skills"></ul>

    <h3>Experience</h3>
    <ul id="p_experience"></ul>
  </div>
</div>

</div>
<div id="resume">
  <div class="blue-line"></div>

  <div class="content">
  </div>
</div>


<script>
const fields = ["name","role","about","skills"];

fields.forEach(id => {
  const el = document.getElementById(id);
  el.value = localStorage.getItem(id) || "";
  el.addEventListener("input", saveAndUpdate);
});

function saveAndUpdate() {
  fields.forEach(id => {
    localStorage.setItem(id, document.getElementById(id).value);
  });
  updatePreview();
}


function updatePreview() {
  document.getElementById("p_name").innerText =
    document.getElementById("name").value || "Your Name";

  document.getElementById("p_role").innerText =
    document.getElementById("role").value || "Your Role";

  document.getElementById("p_about").innerText =
    document.getElementById("about").value || "About you...";

  const skillsList = document.getElementById("p_skills");
  skillsList.innerHTML = "";

  document.getElementById("skills").value.split(",").forEach(skill => {
    if (skill.trim()) {
      const li = document.createElement("li");
      li.innerText = skill.trim();
      skillsList.appendChild(li);
    }
  });
}



function addExperience() {
  if (!expTitle.value || !expCompany.value) return;

  const li = document.createElement("li");
  li.innerText = `${expTitle.value} - ${expCompany.value}`;
  document.getElementById("p_experience").appendChild(li);

  expTitle.value = "";
  expCompany.value = "";
}


function toggleTheme() {
  document.body.classList.toggle("dark");
}


function changeTemplate() {
  resume.className = "preview " + template.value;
}

function downloadPDF() {
  const resume = document.getElementById("resume");

  html2pdf().from(resume).set({
    margin: 0,
    filename: "resume.pdf",
    image: { type: "jpeg", quality: 1 },
    html2canvas: {
      scale: 2,
      scrollX: 0,
      scrollY: 0
    },
    jsPDF: {
      unit: "px",
      format: [794, 1123],
      orientation: "portrait"
    }
  }).save();
}



updatePreview();
</script>

</body>
</html>

