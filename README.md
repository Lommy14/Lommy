const total = cart.reduce((s,i)=>s+i.kcal,0);
totalCaloriesEl.textContent = `${total} kcal`;
}

// Quick add examples
function quickAdd(kind){
if(kind==='breakfast'){
addToCart('f1','food'); // ข้าว
addToCart('f3','food'); // ไข่
addToCart('d3','drink'); // กาแฟ
} else if(kind==='lunch'){
addToCart('f1','food'); addToCart('f4','food'); addToCart('d2','drink');
} else {
addToCart('f4','food'); addToCart('f2','food'); addToCart('d1','drink');
}
renderCart();
}

// Exercise cal calculation using MET formula
function calcExerciseCalories(){
const weight = Number(document.getElementById('weight').value) || 70;
const minutes = Number(document.getElementById('minutes').value) || 30;
const exId = exerciseSelect.value;
const ex = EXERCISES.find(e=>e.id===exId) || EXERCISES[0];
// kcal per minute = (MET * 3.5 * weightKg) / 200
const kcalPerMin = (ex.met * 3.5 * weight) / 200;
const total = Math.round(kcalPerMin * minutes);
exerciseCaloriesEl.textContent = `${total} kcal เผาผลาญ (โดยประมาณ)`;
return total;
}

// Advice based on gap between food and exercise
function showPlan(){
const totalFood = cart.reduce((s,i)=>s+i.kcal,0);
const burn = calcExerciseCalories();
const weight = Number(document.getElementById('weight').value) || 70;
const ageGroup = document.getElementById('ageGroup').value;
const rec = AGE_RECOMMEND[ageGroup];
let advice='';
if(totalFood===0){ advice='ยังไม่มีรายการอาหาร — เพิ่มรายการแล้วลองคำนวณใหม่'; }
else{
const diff = totalFood - burn;
advice += `รวมแคลอรี่ที่รับเข้า: ${totalFood} kcal. ประมาณการเผาผลาญจากการเลือกกิจกรรม: ${burn} kcal.\n`;
if(diff>0){ advice += `หลังออกกำลังกายยังเหลือ ${diff} kcal — หากต้องการลดน้ำหนัก ควรลดพลังงานรับเข้าส่วนนี้หรือเพิ่มกิจกรรม (แนะนำ 300-500 kcal/วัน เป็นจุดเริ่มต้น)`; }
else if(diff<0){ advice += `เผาผลาญมากกว่าแคลอรี่ที่รับเข้า ${Math.abs(diff)} kcal — คุณกำลังอยู่ในภาวะขาดพลังงานชั่วคราว` }
else { advice += 'พลังงานรับเข้าและเผาผลาญใกล้เคียงกัน — อยู่ในภาวะสมดุลโดยประมาณ' }
advice += `\nช่วงแคลอรี่ที่แนะนำสำหรับ ${rec.label}: ${rec.range}`;
}
adviceText.innerText = advice;
}

// Buttons
document.getElementById('calcExercise').addEventListener('click',()=>{
calcExerciseCalories();
});
document.getElementById('showPlan').addEventListener('click',()=>{
showPlan();
});

document.getElementById('resetBtn').addEventListener('click',()=>{
cart = []; renderCart(); document.getElementById('name').value=''; document.getElementById('weight').value=70; document.getElementById('portion').value=100; document.getElementById('minutes').value=30; adviceText.innerText='เพิ่มอาหาร เพื่อดูคำแนะนำการออกกำลังกายที่เหมาะสม';
});

document.getElementById('suggestBtn').addEventListener('click',()=>{
const ageGroup = document.getElementById('ageGroup').value;
const rec = AGE_RECOMMEND[ageGroup];
alert(`ช่วงแคลอรี่โดยประมาณสำหรับ ${rec.label}: ${rec.range} (เป็นค่าประมาณทั่วไป)`);
});

// init
renderFoodDrinks(); renderExercises(); renderCart();

// small helper to allow calling from inline onclick (not ideal but fine for single file)
window.addToCart = addToCart; window.quickAdd = quickAdd; window.decrease = decrease; window.increase = increase; window.removeItem = removeItem; window.calcExerciseCalories = calcExerciseCalories; window.showPlan = showPlan;
</script>
</body>
</html>
