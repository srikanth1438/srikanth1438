import requests
# Constants for the weather API
API_URL = "http://api.openweathermap.org/data/2.5/weather"
API_KEY = "22864d7337384632704129bf0b9254d8"
def fetch_weather_data(location):
    # Build the request URL
    request_url = f"{API_URL}?q={location}&appid={API_KEY}&units=metric"

    # Send the request to the API
    response = requests.get(request_url)

    # Parse the JSON response
    weather_data = response.json()

    if response.status_code != 200:
        return None, weather_data.get("message", "Failed to fetch weather data")

    # Extract relevant weather data
    temperature = weather_data["main"]["temp"]
    weather_conditions = weather_data["weather"][0]["description"]
    humidity = weather_data["main"]["humidity"]
    wind_speed = weather_data["wind"]["speed"]

    return {
        "temperature": temperature,
        "weather_conditions": weather_conditions,
        "humidity": humidity,
        "wind_speed": wind_speed
    }, None

def display_weather_data(weather_data):
    print(f"Temperature: {weather_data['temperature']}°C")
    print(f"Weather Conditions: {weather_data['weather_conditions']}")
    print(f"Humidity: {weather_data['humidity']}%")
    print(f"Wind Speed: {weather_data['wind_speed']} m/s")
def main():
    # Get the location input from the user
    location = input("Enter the city name or coordinates (lat,lon): ")

    # Fetch the weather data for the input location
    weather_data, error = fetch_weather_data(location)

    if error:
        print(f"Error: {error}")
    else:
        # Display the fetched weather data
        display_weather_data(weather_data)

if __name__ == "__main__":
    main()
