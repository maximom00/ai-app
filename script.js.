const apiKey = "YOUR_API_KEY"; // ضع مفتاح API الخاص بك هنا

// وظيفة للحصول على الطقس حسب المدينة
function getWeatherByCity() {
  const city = prompt("أدخل اسم المدينة:");
  if (!city) return;
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric&lang=ar`;
  fetchWeather(url);
}

// وظيفة للحصول على الطقس باستخدام الموقع الجغرافي
function getWeatherByLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      const { latitude, longitude } = position.coords;
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}&units=metric&lang=ar`;
      fetchWeather(url);
    }, () => {
      alert("لم نتمكن من تحديد موقعك.");
    });
  } else {
    alert("المتصفح لا يدعم GPS.");
  }
}

// وظيفة لإظهار التحميل
function showLoading(show = true) {
  document.getElementById("loadingSpinner").style.display = show ? "block" : "none";
}

// وظيفة لجلب بيانات الطقس من API
async function fetchWeather(url) {
  showLoading(true);
  try {
    const response = await fetch(url);
    const data = await response.json();
    showLoading(false);

    if (data.cod === 200) {
      const iconCode = data.weather[0].icon;
      const iconUrl = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
      
      // تحديث اسم المدينة ودرجة الحرارة
      document.getElementById("cityName").textContent = `${data.name}, ${data.sys.country}`;
      document.getElementById("temperature").innerHTML = `${data.main.temp_max}°C / ${data.main.temp_min}°C`;

      // وصف الطقس
      document.getElementById("description").textContent = data.weather[0].description;

      // عرض الرطوبة والرياح
      document.getElementById("humidity").innerHTML = `<span>${data.main.humidity}%</span><br>رطوبة`;
      document.getElementById("wind").innerHTML = `<span>${data.wind.speed} m/s</span><br>رياح`;
      document.getElementById("compass").innerHTML = `<span>${data.wind.deg}°</span><br>اتجاه الرياح`;

      // تحديث الوقت المحلي
      const localTime = new Date().toLocaleTimeString();
      document.getElementById("localTime").textContent = `الوقت المحلي: ${localTime}`;

      // عرض التاريخ
      const date = new Date().toLocaleDateString();
      document.getElementById("date").textContent = `تاريخ اليوم: ${date}`;

    } else {
      alert("حدث خطأ في جلب البيانات.");
    }
  } catch (error) {
    showLoading(false);
    alert("حدث خطأ في الاتصال.");
  }
}

// تحديث تلقائي للطاقة كل 5 دقائق
setInterval(() => {
  const location = localStorage.getItem("location");
  if (location) {
    fetchWeather(location);
  }
}, 300000); // 5 دقائق

// وظيفة لإعادة تحميل البيانات يدويًا
function reloadWeather() {
  getWeatherByCity();
}

// زر الوضع الداكن
document.getElementById("getCityBtn").addEventListener("click", () => {
  document
