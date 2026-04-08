# Weather Destination Finder

An end-to-end Python pipeline that builds a global weather dataset, filters destinations by temperature preference, finds nearby hotels, and generates a multi-stop driving itinerary, all visualized on interactive maps.

## How It Works

The pipeline runs through four stages in a single notebook:

**1. Build the Weather Database** - Samples 2,000 random coordinates across the globe, maps each to its nearest city using `citipy`, then pulls current weather data (temperature, humidity, wind, cloud cover, description) from the [OpenWeatherMap API](https://openweathermap.org/api). Requests are batched with rate-limit handling to avoid API throttling. Results are cached to CSV so subsequent runs skip the fetch entirely.

**2. Filter by Temperature** - Prompts for a preferred temperature range and narrows the dataset to matching cities.

**3. Hotel Search** - For each candidate city, the [Geoapify Places API](https://www.geoapify.com/) searches for hotels within a 5 km radius. Results are displayed on an interactive HvPlot map where point size reflects temperature.

**4. Driving Itinerary** - Four cities are selected for a round-trip route. The [Geoapify Routing API](https://apidocs.geoapify.com/docs/routing/) returns the full route geometry, which is overlaid on the map using GeoViews.

## Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python 3.9 |
| Data Wrangling | Pandas, NumPy |
| APIs | OpenWeatherMap, Geoapify (Places + Routing) |
| Geo-visualization | HvPlot, GeoViews |
| Geocoding | citipy |

## Setup

1. Clone the repo and install dependencies:
   ```bash
   pip install pandas numpy hvplot geoviews citipy requests
   ```

2. Create a `config.py` file in the project root with your API keys:
   ```python
   weather_api_key = "YOUR_OPENWEATHERMAP_KEY"
   geoapify_key = "YOUR_GEOAPIFY_KEY"
   ```
   > **Note:** `config.py` is included in `.gitignore` to prevent key exposure.

3. Run the notebook:
   ```bash
   jupyter notebook weather_destination_finder.ipynb
   ```
   On the first run, weather data is fetched from the API and cached to `data/WeatherPy_Database.csv`. Subsequent runs load directly from the cache.

## Project Structure

```
data/
  WeatherPy_Database.csv       # auto-generated on first run
weather_destination_finder.ipynb
config.py                      # (not tracked - add your own keys)
README.md
```

## Example Output

<!-- Add a screenshot of your final itinerary map here -->
<!-- ![Itinerary Map](images/travel_map.png) -->

The sample itinerary routes through four cities in the northeastern United States:

**Freeport > Clifton > Bethel > Westport > Freeport**
