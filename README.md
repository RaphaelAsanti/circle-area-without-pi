<div class="calculator" aria-live="polite" style="margin-top:12px;">
  <h3 style="margin-top:0.25rem;margin-bottom:8px;">Scale a circle from a known circle</h3>

  <!-- Option 1: use a multiplier k -->
  <div style="margin-bottom:10px;">
    <label for="A_known">Known area A_known (same units as result)</label>
    <input id="A_known" type="number" min="0" step="any" placeholder="e.g. 200" />

    <label for="multiplier">Linear multiplier k (r₂ ÷ r₁)</label>
    <input id="multiplier" type="number" min="0" step="any" placeholder="e.g. 1.5" />

    <button id="scaleBtn" class="btn" type="button" style="margin-top:6px;">Compute A_new = A_known × k²</button>
    <div id="scaleResult" class="result" aria-live="polite"></div>
  </div>

  <hr style="border:none;border-top:1px dashed #ddd;margin:10px 0;">

  <!-- Option 2: derive k from radii or diameters -->
  <div>
    <label>Or enter radii/diameters to compute k automatically</label>
    <div style="display:flex;gap:8px;">
      <div style="flex:1;">
        <label for="r1">Known radius/diameter (r₁ or d₁)</label>
        <input id="r1" type="number" min="0" step="any" placeholder="e.g. 5" />
      </div>
      <div style="flex:1;">
        <label for="r2">New radius/diameter (r₂ or d₂)</label>
        <input id="r2" type="number" min="0" step="any" placeholder="e.g. 7.5" />
      </div>
    </div>

    <button id="fromRadiiBtn" class="btn" type="button" style="margin-top:6px;">Compute A_new from radii/diameters</button>
    <div id="fromRadiiResult" class="result" aria-live="polite"></div>
  </div>

  <div style="font-size:0.9rem;color:var(--muted);margin-top:8px;">
    Note: k is a linear scale (ratio of radii or diameters). Areas scale as k².
  </div>
</div>

<script>
(function(){
  function formatNumber(n) {
    return Number.isFinite(n) ? (+n.toFixed(4)).toString() : '—';
  }

  var scaleBtn = document.getElementById('scaleBtn');
  var A_known = document.getElementById('A_known');
  var multiplier = document.getElementById('multiplier');
  var scaleResult = document.getElementById('scaleResult');

  scaleBtn.addEventListener('click', function(){
    var A = parseFloat(A_known.value);
    var k = parseFloat(multiplier.value);

    if (!isFinite(A) || !isFinite(k)) {
      scaleResult.textContent = 'Please enter numbers for known area and multiplier.';
      scaleResult.style.color = '#b22222';
      return;
    }
    if (A < 0 || k < 0) {
      scaleResult.textContent = 'Please enter non-negative values.';
      scaleResult.style.color = '#b22222';
      return;
    }

    var Anew = A * (k * k);
    scaleResult.style.color = '';
    scaleResult.textContent = 'A_new = ' + formatNumber(Anew) + ' (same units as A_known)';
  });

  var fromRadiiBtn = document.getElementById('fromRadiiBtn');
  var r1 = document.getElementById('r1');
  var r2 = document.getElementById('r2');
  var fromRadiiResult = document.getElementById('fromRadiiResult');

  fromRadiiBtn.addEventListener('click', function(){
    var A = parseFloat(A_known.value);
    var v1 = parseFloat(r1.value);
    var v2 = parseFloat(r2.value);

    if (!isFinite(A)) {
      fromRadiiResult.textContent = 'Please enter the known area (A_known).';
      fromRadiiResult.style.color = '#b22222';
      return;
    }
    if (!isFinite(v1) || !isFinite(v2)) {
      fromRadiiResult.textContent = 'Please enter both radii/diameters.';
      fromRadiiResult.style.color = '#b22222';
      return;
    }
    if (v1 <= 0 || v2 < 0) {
      fromRadiiResult.textContent = 'Please enter positive values (v₁ must be > 0).';
      fromRadiiResult.style.color = '#b22222';
      return;
    }

    var k = v2 / v1;
    var Anew = A * (k * k);
    fromRadiiResult.style.color = '';
    fromRadiiResult.textContent = 'k = ' + formatNumber(k) + ' → A_new = ' + formatNumber(Anew);
  });
})();
</script>
