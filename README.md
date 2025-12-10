// تعريف مجموعات الأعمدة مع عدد الأعمدة الفرعية
const groups = [
  { title: "ت",            cls: "",         colspan: 1,   sub: ["#"] }, // عمود الأرقام
  { title: "معلومات الطالب", cls: "hdr-student", colspan: 3, sub: ["الاسم","اسم الأب","قيد"] },
  { title: "معلومات ولي الأمر", cls: "hdr-parent", colspan: 2, sub: ["هاتف","ملاحظة"] },
  { title: "المعلومات الأكاديمية", cls: "hdr-acad", colspan: 3, sub: ["المادة","الصف","المستوى"] },
  { title: "الحضور اليومي", cls: "hdr-att", colspan: DAYS_COUNT, sub: Array.from({length:DAYS_COUNT}, (_,i)=> (i+1).toString()) },
  { title: "المجاميع/ملاحظات", cls: "hdr-totals", colspan: 4, sub: ["اجمالي","غياب","درجات","ملاحظة"] }
];

// إنشاء صف الرأس العلوي (المجموعات الملونة)
function buildHeaderTop(){
  const tr = document.createElement("tr");
  groups.forEach(g=>{
    const th = document.createElement("th");
    th.className = g.cls + " rtl";
    th.setAttribute("colspan", g.colspan);
    th.textContent = g.title;
    tr.appendChild(th);
  });
  return tr;
}

// إنشاء صف الرؤوس الفرعية (أسماء الأعمدة)
function buildHeaderSub(){
  const tr = document.createElement("tr");
  groups.forEach(g=>{
    // لكل مجموعة نضيف رؤوس فرعية
    const subs = g.sub;
    subs.forEach((s, idx)=>{
      const th = document.createElement("th");
      th.className = (g.cls ? g.cls + " " : "") + "rtl";
      // إذا كانت المجموعة هي عمود الأرقام اجعله sticky
      if(g.title === "ت"){
        th.classList.add("row-number","sticky-left");
        th.innerHTML = "<strong>#</strong>";
      } else {
        // للأيام اجعلها أصغر
        if(g.title === "الحضور اليومي"){
          th.classList.add("day-header");
          th.textContent = s;
        } else {
          th.textContent = s;
        }
      }
      tr.appendChild(th);
    });
  });
  return tr;
}

// توليد صفوف البيانات
function buildBody(){
  const tbody = document.getElementById("tbody");
  for(let r=1; r<=ROWS_COUNT; r++){
    const tr = document.createElement("tr");

    // عمود الأرقام (ثابت)
    const tdNum = document.createElement("td");
    tdNum.className = "row-number sticky-left";
    tdNum.textContent = r;
    tr.appendChild(tdNum);

    // معلومات الطالب: الاسم، اسم الأب، قيد
    const nameCell = document.createElement("td"); nameCell.textContent = "اسم الطالب " + r;
    const fatherCell = document.createElement("td"); fatherCell.textContent = "اسم الأب";
    const regCell = document.createElement("td"); regCell.textContent = "قيد" + (1000 + r);
    tr.appendChild(nameCell);
    tr.appendChild(fatherCell);
    tr.appendChild(regCell);

    // معلومات ولي الأمر: هاتف، ملاحظة
    const phoneCell = document.createElement("td"); phoneCell.textContent = "07xxxxxxxx";
    const noteCell = document.createElement("td"); noteCell.textContent = "";
    tr.appendChild(phoneCell);
    tr.appendChild(noteCell);

    // المعلومات الأكاديمية: مادة، صف، مستوى
    const subjCell = document.createElement("td"); subjCell.textContent = "مادة أ";
    const gradeCell = document.createElement("td"); gradeCell.textContent = "الثالث";
    const levelCell = document.createElement("td"); levelCell.textContent = "متوسط";
    tr.appendChild(subjCell);
    tr.appendChild(gradeCell);
    tr.appendChild(levelCell);

    // أعمدة الحضور اليومية (ضيقة)
    for(let d=1; d<=DAYS_COUNT; d++){
      const td = document.createElement("td");
      td.className = "day-cell";
      // مثال: تترك خالية، أو تضع علامة حضور/غياب
      td.innerHTML = "";
      tr.appendChild(td);
    }

    // مجاميع ودرجات
    const totCell = document.createElement("td"); totCell.textContent = ""; // إجمالي حضور
    const absentCell = document.createElement("td"); absentCell.textContent = ""; // غياب
    const markCell = document.createElement("td"); markCell.textContent = ""; // درجات
    const remarkCell = document.createElement("td"); remarkCell.textContent = ""; // ملاحظة
    tr.appendChild(totCell);
    tr.appendChild(absentCell);
    tr.appendChild(markCell);
    tr.appendChild(remarkCell);

    tbody.appendChild(tr);
  }
  // تنفيذ البناء
(function init(){
  const thead = document.getElementById("thead");
  thead.appendChild(buildHeaderTop());
  thead.appendChild(buildHeaderSub());
  buildBody();
})();

</script>

</body>
</html>
}
