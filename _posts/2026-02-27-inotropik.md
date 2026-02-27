---
layout: post
title: Kalkulator Inotropik & Vasopressor Syringe Pump
---

<!-- Input Berat Badan -->
<div style="background: #fdfdfd; padding: 20px; border-radius: 10px; margin-bottom: 20px; font-family: sans-serif; border: 2px solid #2c3e50; display: inline-block;">
  <label style="font-weight: bold; font-size: 16px;">Berat Badan (BB) Pasien: </label>
  <input type="number" id="inputBB" value="60" oninput="hitungSemuaDosis()" style="padding: 8px; width: 80px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; font-weight: bold; text-align: center;"> <span style="font-weight: bold;">kg</span>
</div>

<style>
  .tabel-utama { width: 100%; border-collapse: collapse; font-family: Arial, sans-serif; font-size: 12px; table-layout: fixed; }
  .tabel-utama td { vertical-align: top; border: 1px solid #bdc3c7; padding: 10px; background: #fff; }
  .obat-title { font-weight: bold; display: block; margin-bottom: 3px; font-size: 13px; border-bottom: 2px solid; padding-bottom: 3px; text-transform: uppercase; }
  .rumus-text { font-size: 10px; color: #7f8c8d; font-style: italic; display: block; margin-bottom: 8px; }
  .hasil-angka { font-weight: bold; color: #2980b9; }
  .inner-table { width: 100%; }
  .inner-table tr:nth-child(even) { background-color: #f2f2f2; }
  .inner-table td { border: none; padding: 5px 2px; border-bottom: 1px solid #eee; }
  .scroll-container { max-height: 350px; overflow-y: auto; border: 1px solid #ecf0f1; padding-right: 5px; }
  
  /* Warna Tema */
  .nora { color: #e74c3c; border-color: #e74c3c; }
  .ntg { color: #d35400; border-color: #d35400; }
  .dobu250 { color: #27ae60; border-color: #27ae60; }
  .dopa { color: #2980b9; border-color: #2980b9; }
  .milri { color: #8e44ad; border-color: #8e44ad; }
  .dobu500 { color: #16a085; border-color: #16a085; }
</style>

<table class="tabel-utama">
  <tr>
    <!-- Noradrenalin -->
    <td>
      <span class="obat-title nora">Noradrenalin (4mg/50cc)</span>
      <span class="rumus-text">1cc = 80 mcg | (Dosis * BB * 60) / 80</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-nora"></table>
      </div>
    </td>
    <!-- NTG -->
    <td>
      <span class="obat-title ntg">NTG (50mg/50cc)</span>
      <span class="rumus-text">1cc = 1000 mcg | (Dosis * BB * 60) / 1000</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-ntg"></table>
      </div>
    </td>
    <!-- Dobutamin 250 -->
    <td>
      <span class="obat-title dobu250">Dobutamin (250mg/50cc)</span>
      <span class="rumus-text">1cc = 5000 mcg | (Dosis * BB * 60) / 5000</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-dobu250"></table>
      </div>
    </td>
  </tr>
  <tr>
    <!-- Dopamin -->
    <td>
      <span class="obat-title dopa">Dopamin (200mg/50cc)</span>
      <span class="rumus-text">1cc = 4000 mcg | (Dosis * BB * 60) / 4000</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-dopa"></table>
      </div>
    </td>
    <!-- Milrinon -->
    <td>
      <span class="obat-title milri">Milrinon (20mg/50cc)</span>
      <span class="rumus-text">1cc = 400 mcg | (Dosis * BB * 60) / 400</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-milri"></table>
      </div>
    </td>
    <!-- Dobutamin 500 -->
    <td>
      <span class="obat-title dobu500">Dobutamin (500mg/50cc)</span>
      <span class="rumus-text">1cc = 10000 mcg | (Dosis * BB * 60) / 10000</span>
      <div class="scroll-container">
        <table class="inner-table" id="tabel-dobu500"></table>
      </div>
    </td>
  </tr>
</table>

<script>
// Definisi Array Dosis sesuai permintaan user
const doses = {
  // Noradrenalin: 0.05, lalu 0.1 sampai 1.0 (inc 0.1)
  nora: [0.05, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0],
  
  // NTG: 0.5 sampai 2.5 (inc 0.25)
  ntg: generateRange(0.5, 2.5, 0.25),
  
  // Dobutamin 250: 5 sampai 25 (inc 2.5)
  dobu250: generateRange(5, 25, 2.5),
  
  // Dopamin: 3, lalu 5 sampai 25 (inc 2.5)
  dopa: [3, ...generateRange(5, 25, 2.5)],
  
  // Milrinon: 0.375, 0.5, 0.75, lalu 1.0 sampai 2.25 (inc 0.25)
  milri: [0.375, 0.5, 0.75, ...generateRange(1.0, 2.25, 0.25)],
  
  // Dobutamin 500: 5 sampai 25 (inc 2.5)
  dobu500: generateRange(5, 25, 2.5)
};

// Konstanta Pembagi (1cc = x mcg)
const constants = {
  nora: 80, ntg: 1000, dobu250: 5000, dopa: 4000, milri: 400, dobu500: 10000
};

// Helper fungsi untuk membuat range angka
function generateRange(start, end, step) {
  let arr = [];
  for (let i = start; i <= end + 0.001; i += step) {
    arr.push(Number(i.toFixed(3)));
  }
  return arr;
}

function renderTables() {
  Object.keys(doses).forEach(key => {
    const container = document.getElementById("tabel-" + key);
    let rows = "";
    doses[key].forEach(d => {
      let safeId = "id_" + key + "_" + d.toString().replace(".", "_");
      rows += `<tr>
                <td style="width:70px">${d} mcg</td>
                <td style="width:15px">=</td>
                <td><span id="${safeId}" class="hasil-angka">0</span></td>
                <td style="text-align:right">cc/jam</td>
              </tr>`;
    });
    container.innerHTML = rows;
  });
}

function hitungSemuaDosis() {
  const bb = parseFloat(document.getElementById("inputBB").value) || 0;
  
  Object.keys(doses).forEach(key => {
    const konstanta = constants[key];
    doses[key].forEach(d => {
      let safeId = "id_" + key + "_" + d.toString().replace(".", "_");
      let element = document.getElementById(safeId);
      if (element) {
        // Rumus: (Dosis * BB * 60) / Konstanta
        let hasil = (d * bb * 60) / konstanta;
        element.innerText = hasil.toFixed(2);
      }
    });
  });
}

// Jalankan saat halaman dimuat
window.onload = function() {
  renderTables();
  hitungSemuaDosis();
};
</script>
