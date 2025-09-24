# Cardinal-onboarding-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cardinal Landscaping Form</title>
<style>
body { font-family: Arial, sans-serif; background-color: #f9f5f5; padding: 20px; color: #333; }
h1 { text-align: center; color: #8B0000; margin-bottom: 30px; }
form { max-width: 900px; margin: auto; background: #fff; padding: 25px; border-radius: 12px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); border-top: 6px solid #8B0000; }
label { display: block; margin-top: 15px; font-weight: bold; }
input, select, textarea { width: 100%; padding: 10px; margin-top: 5px; border-radius: 6px; border: 1px solid #ccc; font-size: 1em; }
.checkbox-group { display: flex; gap: 20px; margin-top: 5px; }
button { margin-top: 20px; padding: 12px 25px; border: none; border-radius: 6px; background-color: #8B0000; color: #fff; font-size: 1em; cursor: pointer; }
button:hover { background-color: #b22222; }
.total { font-weight: bold; margin-top: 15px; color: #8B0000; font-size: 1.2em; }
.section { border: 1px solid #eee; padding: 15px; border-radius: 8px; margin-top: 15px; background-color: #fef5f5; }
.tooltip-text { font-weight: normal; font-size: 0.85em; color: #555; }
</style>
</head>
<body>

<h1>Cardinal Landscaping Onboarding Form</h1>

<form id="landscapingForm" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">

<label for="name">Full Name:</label>
<input type="text" name="name" id="name" required>

<label for="email">Email:</label>
<input type="email" name="email" id="email" required>

<label for="phone">Phone:</label>
<input type="text" name="phone" id="phone" required>

<label for="address">Property Address:</label>
<input type="text" name="address" id="address" required>

<!-- Bed Sizes -->
<div class="section">
<label>Bed Quantity <span class="tooltip-text">(Small = 1 parking space, Medium = 2 parking spaces, Large = 4 parking spaces)</span></label>
<select id="smallBeds" name="smallBeds">
  <option value="0">0 Small Beds</option>
  <option value="1">1 Small Bed</option>
  <option value="2">2 Small Beds</option>
  <option value="3">3 Small Beds</option>
  <option value="4">4 Small Beds</option>
  <option value="5">5 Small Beds</option>
  <option value="6">6 Small Beds</option>
  <option value="7">7 Small Beds</option>
  <option value="8">8 Small Beds</option>
  <option value="9">9 Small Beds</option>
  <option value="10">10 Small Beds</option>
</select>

<select id="mediumBeds" name="mediumBeds">
  <option value="0">0 Medium Beds</option>
  <option value="1">1 Medium Bed</option>
  <option value="2">2 Medium Beds</option>
  <option value="3">3 Medium Beds</option>
  <option value="4">4 Medium Beds</option>
  <option value="5">5 Medium Beds</option>
  <option value="6">6 Medium Beds</option>
  <option value="7">7 Medium Beds</option>
  <option value="8">8 Medium Beds</option>
  <option value="9">9 Medium Beds</option>
  <option value="10">10 Medium Beds</option>
</select>

<select id="largeBeds" name="largeBeds">
  <option value="0">0 Large Beds</option>
  <option value="1">1 Large Bed</option>
  <option value="2">2 Large Beds</option>
  <option value="3">3 Large Beds</option>
  <option value="4">4 Large Beds</option>
  <option value="5">5 Large Beds</option>
  <option value="6">6 Large Beds</option>
  <option value="7">7 Large Beds</option>
  <option value="8">8 Large Beds</option>
  <option value="9">9 Large Beds</option>
  <option value="10">10 Large Beds</option>
</select>
</div>

<!-- Mulching -->
<div class="section">
<label>Do you want Mulching?</label>
<div class="checkbox-group">
  <label><input type="radio" name="mulchService" value="yes"> Yes</label>
  <label><input type="radio" name="mulchService" value="no" checked> No</label>
</div>

<label>Mulch Color:</label>
<select id="mulchColor" name="mulchColor">
  <option value="red">Red</option>
  <option value="brown">Brown</option>
  <option value="black">Black</option>
</select>
</div>

<!-- Edging -->
<div class="section">
<label>Edging:</label>
<select id="edging" name="edging">
  <option value="none">No Edging</option>
  <option value="earth">Earth</option>
  <option value="plastic">Plastic</option>
  <option value="metal">Metal</option>
</select>
<label>Install New Edging?</label>
<div class="checkbox-group">
  <label><input type="radio" name="newEdging" value="yes" checked> Yes</label>
  <label><input type="radio" name="newEdging" value="no"> No</label>
</div>
</div>

<!-- Shrubs -->
<div class="section">
<label>Shrub Care - Quantity:</label>
<select id="shrubQuantity" name="shrubQuantity">
  <option value="none">No Shrub Trimming</option>
  <option value="few">Fewer than 10</option>
  <option value="some">10–20</option>
  <option value="many">More than 20</option>
</select>
<label><input type="checkbox" id="tallShrubs" name="tallShrubs"> Shrubs over 10 feet</label>
</div>

<!-- Weeding -->
<div class="section">
<label>Weeding Level:</label>
<select id="weedLevel" name="weedLevel">
  <option value="none">No Weeding</option>
  <option value="moderate">Moderate</option>
  <option value="overgrown">Overgrown</option>
  <option value="takenOver">Completely Taken Over</option>
</select>
</div>

<!-- Description & upload -->
<div class="section">
<label>Describe Scope / Upload Photos (optional):</label>
<textarea id="description" name="description" rows="4" placeholder="Optional description..."></textarea>
<input type="file" id="upload" name="upload" multiple>
</div>

<!-- Visit date & down payment -->
<div class="section">
<label>Preferred Visit Date (optional):</label>
<input type="date" id="visitDate" name="visitDate">

<label>Down Payment ($):</label>
<input type="number" id="downPayment" name="downPayment" readonly>

<p id="waitTime" class="tooltip-text"></p>
</div>

<p class="total">Estimated Total: $<span id="totalPrice">0</span></p>

<button type="submit">Submit Form</button>

</form>

<script>
function calculateTotal() {
    let small = parseInt(document.getElementById('smallBeds').value)||0;
    let medium = parseInt(document.getElementById('mediumBeds').value)||0;
    let large = parseInt(document.getElementById('largeBeds').value)||0;
    
    let smallPrice=50, mediumPrice=100, largePrice=150;

    function bedPrice(count, base) {
        let total=0;
        for(let i=0;i<count;i++) total+=base*(i<2?1:0.85); // 15% discount past 2 beds
        return total;
    }

    let totalBeds = small+medium+large;
    let total = bedPrice(small, smallPrice) + bedPrice(medium, mediumPrice) + bedPrice(large, largePrice);

    // Only show price if a service is selected
    let trimming = document.getElementById('shrubQuantity').value !== 'none';
    let weeding = document.getElementById('weedLevel').value !== 'none';
    let edging = document.getElementById('edging').value !== 'none';
    let mulch = document.querySelector('input[name="mulchService"]:checked').value
    // Only show price if a service is selected
    let showPrice = trimming || weeding || edging || mulch === 'yes';
    if(!showPrice){
        document.getElementById('totalPrice').textContent = "0.00";
        document.getElementById('downPayment').value = "0.00";
        document.getElementById('waitTime').textContent = '';
        return;
    }

    // Edging costs
    if(edging !== 'none'){
        let edgeCostSmall = small * (edging==='earth'?35:65);
        let edgeCostMedium = medium * (edging==='earth'?45:95);
        let edgeCostLarge = large * (edging==='earth'?65:125);
        let edgeTotal = edgeCostSmall + edgeCostMedium + edgeCostLarge;
        if(edging==='metal') edgeTotal *= 1.35;
        total += edgeTotal;
    }

    // Shrubs
    let shrub=document.getElementById('shrubQuantity').value;
    if(shrub==='few') total+=100;
    else if(shrub==='some') total+=175;
    else if(shrub==='many') total+=250;
    if(document.getElementById('tallShrubs').checked && shrub!=='none') total+=75;

    // Weeding
    let weed=document.getElementById('weedLevel').value;
    if(weed==='moderate') total+=50;
    else if(weed==='overgrown') total+=100;
    else if(weed==='takenOver') total+=150;

    // Discounts for only one service
    if(trimming && !weeding && !edging && mulch!=='yes') total *= 0.7; // 30% off trimming only
    if(weeding && !trimming && !edging && mulch!=='yes') total *= 0.5;  // 50% off weeding only

    document.getElementById('totalPrice').textContent = total.toFixed(2);

    // Down payment logic
    let visitDate=document.getElementById('visitDate').value;
    let down = visitDate ? total*0.4 : total*0.2;
    document.getElementById('downPayment').value = down.toFixed(2);

    // Wait time only if no date selected
    document.getElementById('waitTime').textContent = (!visitDate && total>0)?'Estimated time to service: 3–5 days':'';
}

// Run calculation on change
['smallBeds','mediumBeds','largeBeds','edging','mulchService','shrubQuantity','tallShrubs','weedLevel','visitDate'].forEach(id=>{
    let el = document.getElementById(id) || 
document.querySelector(`input[name="${id}"]`);
    if(el) el.addEventListener('change', calculateTotal);
});

// Initial calculation
window.addEventListener('DOMContentLoaded', calculateTotal);
<label>Medium Beds (2 parking spaces): <input type="number" id="mediumBeds" min="0" value="0"></label>
  <label>Large Beds (3 parking spaces): <input type="number" id="largeBeds" min="0" value="0"></label>

  <label>
    <input type="checkbox" id="paymentPlan">
    Pay over 12 months (20% surcharge)
  </label>

  <p id="monthlyPayment" style="font-weight:bold;"></p>

  <button type="submit">Submit</button>
</form>

<div id="paypal-button-container" style="display:none;"></div>

<script>
// Function to calculate total based on bed sizes
function calculateTotal() {
    const small = parseInt(document.getElementById('smallBeds').value) || 0;
    const medium = parseInt(document.getElementById('mediumBeds').value) || 0;
    const large = parseInt(document.getElementById('largeBeds').value) || 0;

    // Prices based on your earlier rules
    let total = 0;
    total += small * 100;
    total += medium * 100 * 0.8; // 20% reduction from base
    total += large * 100 * 0.64; // 20% reduction per tier

    return total;
}});
</script>
</body>
</html>
