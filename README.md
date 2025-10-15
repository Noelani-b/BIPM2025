# BIPM2025
Class work for HWR - Business Intelligence


Introduction: about me

!pip install folium

import folium
from pyproj import Geod

geod = Geod(ellps="WGS84")

# Define stops
stops = [
    {"city": "Sutter, CA", "lat": 38.9509675, "lon": -121.697088, "popup": "Born and raised (1992) 👶"},
    {"city": "Oahu, Hawaii", "lat": 21.4834365, "lon": -158.0364837, "popup": "Growing up (2000-2010)🏄"},
    {"city": "Los Angeles, CA", "lat": 34.0533447265625, "lon": -118.24234771728516, "popup": "Biochemistry + Optometry (2010-2016) 🎓"},
    {"city": "Tokyo, Japan", "lat": 35.6768601, "lon": 139.7638947, "popup": "Fashion (2016) 👚"},
    {"city": "Hamburg, Germany", "lat": 53.550341, "lon": 10.000654, "popup": "Video Game Startup (2017-2019) 🎮"},
    {"city": "Moscow, Russia", "lat": 55.625578, "lon": 37.6063916, "popup": "Video Game Startup (2017-2019) 🎮"},
    {"city": "Athens, Greece", "lat": 37.9755648, "lon": 23.7348324, "popup": "Freelance (2019) 💼"},
    {"city": "Los Angeles, CA", "lat": 34.0533447265625, "lon": -118.24234771728516, "popup": "Fashion Resale (2020) 🎓"},
    {"city": "Copenhagen, DK", "lat": 55.67670440673828, "lon": 12.568477630615234, "popup": "Copenhagen Business School + Revenue Operations (2020-2025) 💻"},
    {"city": "Berlin, Germany", "lat": 52.5200, "lon": 13.4050, "popup": "University (2025-Now) 🏫"}
]

# Create map
m = folium.Map(
    location=[40, 0],
    zoom_start=2.3,
    tiles="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
    attr="© OpenStreetMap contributors © CARTO"
)

# Add markers
for stop in stops:
    folium.Marker(
        [stop["lat"], stop["lon"]],
        popup=stop["popup"],
        tooltip=stop["city"],
        icon=folium.Icon(color="blue", icon="info-sign")
    ).add_to(m)

# Function to compute many intermediate great-circle points
def great_circle_points(lat1, lon1, lat2, lon2, npts=100):
    points = geod.npts(lon1, lat1, lon2, lat2, npts)
    coords = [(lat1, lon1)] + [(lat, lon) for lon, lat in points] + [(lat2, lon2)]
    return coords

# Draw arcs between consecutive stops
for i in range(len(stops) - 1):
    start, end = stops[i], stops[i + 1]
    arc = great_circle_points(start["lat"], start["lon"], end["lat"], end["lon"], npts=60)
    folium.PolyLine(
        arc,
        color="darkblue",
        weight=3,
        opacity=0.7
    ).add_to(m)

# Save and open
m.save("academic_journey_map.html")
import webbrowser; webbrowser.open("academic_journey_map.html")

m
