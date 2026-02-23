<section id="contact">
    <h2>Contact Us</h2>
    <form id="fuelForm">
        <input type="text" placeholder="Name" required>
        <input type="email" placeholder="Email" required>
        <input type="tel" placeholder="Phone" value="0762359288" required>
        <select required>
            <option value="">I am a...</option>
            <option value="b2b">Business (B2B)</option>
            <option value="b2c">Individual (B2C)</option>
        </select>
        <textarea placeholder="Message" rows="5" required></textarea>
        <button type="submit">Send Message & Track Fuel</button>
    </form>
    <div id="tracking" style="margin-top:20px; font-weight:bold; color:#0d6efd;">Delivery status will appear here...</div>
    <div id="map" style="height:400px; margin-top:20px;"></div>
</section>

<!-- Leaflet JS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

<script>
const form = document.getElementById('fuelForm');
const trackingDiv = document.getElementById('tracking');
let map, marker;

// Initialize map centered on a default location
function initMap() {
    map = L.map('map').setView([0, 0], 2); // world view initially
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
    }).addTo(map);
}

// Simulate fuel delivery movement from start to end coordinates
function simulateDelivery(startLat, startLng, endLat, endLng) {
    let lat = startLat;
    let lng = startLng;
    let progress = 0;

    if(marker) marker.remove();
    marker = L.marker([lat, lng]).addTo(map);
    map.setView([lat, lng], 13);

    const interval = setInterval(() => {
        progress += 0.02;
        if(progress >= 1) {
            progress = 1;
            marker.setLatLng([endLat, endLng]);
            map.setView([endLat, endLng], 13);
            trackingDiv.textContent = "Your fuel has been delivered successfully!";
            clearInterval(interval);
        } else {
            lat = startLat + (endLat - startLat) * progress;
            lng = startLng + (endLng - startLng) * progress;
            marker.setLatLng([lat, lng]);
            trackingDiv.textContent = `Fuel delivery in progress: ${Math.round(progress*100)}%`;
            map.setView([lat, lng], 13);
        }
    }, 1000);
}

initMap();

form.addEventListener('submit', function(e) {
    e.preventDefault();
    trackingDiv.textContent = "Fuel request received! Tracking your delivery...";
    
    // Simulated coordinates (replace with real GPS if available)
    const startLat = 40.7128; // e.g., warehouse
    const startLng = -74.0060;
    const endLat = 40.730610; // e.g., customer
    const endLng = -73.935242;

    simulateDelivery(startLat, startLng, endLat, endLng);
});
</script>
