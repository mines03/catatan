---
layout: post
title: obat inotropik
---
<!-- Input BB Interaktif -->
<div style="background: #f4f4f4; padding: 15px; border-radius: 8px; margin-bottom: 20px; font-family: sans-serif; border: 1px solid #ddd;">
  <label style="font-weight: bold;">Berat Badan (BB): </label>
  <input type="number" id="inputBB" value="65" oninput="hitungSemuaDosis()" style="padding: 5px; width: 60px; border-radius: 4px; border: 1px solid #ccc;"> kg
</div>

<style>
  .tabel-utama { width: 100%; border-collapse: collapse; font-family: Arial, sans-serif; font-size: 13px; table-layout: fixed; }
  .tabel-utama td { vertical-align: top; border: 1px solid #000; padding: 8px; }
  .obat-title { font-weight: bold; display: block; margin-bottom: 5px; }
  .rumus-text { font-size: 11px; color: #666; font-style: italic; }
  .hasil-angka { font-weight: bold; color: blue; }
  .inner-table { width: 100%; margin-top: 8px; }
  .inner-table td { border: none; padding: 2px 0; }
</style>

<table class="tabel-utama">
  <!-- BARIS 1 -->
  <tr>
    <!-- Noradrenalin -->
    <td>
      <span class="obat-title" style="color: red;">Noradrenalin (4mg/50cc)</span>
      <span class="rumus-text">1cc = 80 mcg | (Dosis * BB * 60) / 80</span>
      <table class="inner-table">
        <tr><td>0.1 mcg</td><td>= <span id="nora_01" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>0.2 mcg</td><td>= <span id="nora_02" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>0.5 mcg</td><td>= <span id="nora_05" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
    <!-- NTG -->
    <td>
      <span class="obat-title" style="color: brown;">NTG (50mg/50cc)</span>
      <span class="rumus-text">1cc = 1000 mcg | (Dosis * BB * 60) / 1000</span>
      <table class="inner-table">
        <tr><td>0.5 mcg</td><td>= <span id="ntg_05" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>1.0 mcg</td><td>= <span id="ntg_10" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>2.0 mcg</td><td>= <span id="ntg_20" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
    <!-- Dobutamin 250mg -->
    <td>
      <span class="obat-title" style="color: green;">Dobutamin (250mg/50cc)</span>
      <span class="rumus-text">1cc = 5000 mcg | (Dosis * BB * 60) / 5000</span>
      <table class="inner-table">
        <tr><td>5 mcg</td><td>= <span id="dobu250_5" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>10 mcg</td><td>= <span id="dobu250_10" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>15 mcg</td><td>= <span id="dobu250_15" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
  </tr>

  <!-- BARIS 2 -->
  <tr>
    <!-- Dopamin -->
    <td>
      <span class="obat-title" style="color: blue;">Dopamin (200mg/50cc)</span>
      <span class="rumus-text">1cc = 4000 mcg | (Dosis * BB * 60) / 4000</span>
      <table class="inner-table">
        <tr><td>3 mcg</td><td>= <span id="dopa_3" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>5 mcg</td><td>= <span id="dopa_5" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>10 mcg</td><td>= <span id="dopa_10" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
    <!-- Milrinon -->
    <td>
      <span class="obat-title" style="color: purple;">Milrinon (20mg/50cc)</span>
      <span class="rumus-text">1cc = 400 mcg | (Dosis * BB * 60) / 400</span>
      <table class="inner-table">
        <tr><td>0.375 mcg</td><td>= <span id="mil_0375" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>0.5 mcg</td><td>= <span id="mil_05" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>0.75 mcg</td><td>= <span id="mil_075" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
    <!-- Dobutamin 500mg -->
    <td>
      <span class="obat-title" style="color: darkgreen;">Dobutamin (500mg/50cc)</span>
      <span class="rumus-text">1cc = 10000 mcg | (Dosis * BB * 60) / 10000</span>
      <table class="inner-table">
        <tr><td>5 mcg</td><td>= <span id="dobu500_5" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>10 mcg</td><td>= <span id="dobu500_10" class="hasil-angka"></span></td><td>cc/j</td></tr>
        <tr><td>20 mcg</td><td>= <span id="dobu500_20" class="hasil-angka"></span></td><td>cc/j</td></tr>
      </table>
    </td>
  </tr>
</table>

<script>
function hitungSemuaDosis() {
  const bb = parseFloat(document.getElementById("inputBB").value) || 0;

  // Helper function untuk hitung dan tampilkan
  const update = (id, dosis, konstanta) => {
    let hasil = (dosis * bb * 60) / konstanta;
    // Tampilkan 3 angka di belakang koma jika tidak bulat
    document.getElementById(id).innerText = Number.isInteger(hasil) ? hasil : hasil.toFixed(3);
  };

  // 1. Noradrenalin (Konstanta 80)
  update("nora_01", 0.1, 80);
  update("nora_02", 0.2, 80);
  update("nora_05", 0.5, 80);

  // 2. NTG (Konstanta 1000)
  update("ntg_05", 0.5, 1000);
  update("ntg_10", 1.0, 1000);
  update("ntg_20", 2.0, 1000);

  // 3. Dobutamin 250 (Konstanta 5000)
  update("dobu250_5", 5, 5000);
  update("dobu250_10", 10, 5000);
  update("dobu250_15", 15, 5000);

  // 4. Dopamin (Konstanta 4000)
  update("dopa_3", 3, 4000);
  update("dopa_5", 5, 4000);
  update("dopa_10", 10, 4000);

  // 5. Milrinon (Konstanta 400)
  update("mil_0375", 0.375, 400);
  update("mil_05", 0.5, 400);
  update("mil_075", 0.75, 400);

  // 6. Dobutamin 500 (Konstanta 10000)
  update("dobu500_5", 5, 10000);
  update("dobu500_10", 10, 10000);
  update("dobu500_20", 20, 10000);
}

// Inisialisasi saat pertama kali buka
window.onload = hitungSemuaDosis;
</script>
