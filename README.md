> Why do I have a folder named ".expo" in my project?

The ".expo" folder is created when an Expo project is started using "expo start" command.

> What do the files contain?

- "devices.json": contains information about devices that have recently opened this project. This is used to populate the "Development sessions" list in your development builds.
- "packager-info.json": contains port numbers and process PIDs that are used to serve the application to the mobile device/simulator.
- "settings.json": contains the server configuration that is used to serve the application manifest.

> Should I commit the ".expo" folder?

No, you should not share the ".expo" folder. It does not contain any information that is relevant for other developers working on the project, it is specific to your machine.

Upon project creation, the ".expo" folder is already added to your ".gitignore" file.
568
import React, { useState, useEffect } from "react";
import { StyleSheet, Text, View, TextInput, Button, Image } from "react-native";

const apiKey = "5074db2b4667f1a2b5cceef9ef3050f7"; // Replace with your OpenWeather API key

export default function App() {
  const [city, setCity] = useState("");
  const [weatherData, setWeatherData] = useState(null);

  const getWeather = async () => {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`,
      );
      const data = await response.json();

      if (response.ok) {
        setWeatherData(data);
      } else {
        alert("City not found");
      }
    } catch (error) {
      alert("Error fetching weather data");
    }
  };

  useEffect(() => {
    if (weatherData) {
      console.log(weatherData);
    }
  }, [weatherData]);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Weather App</Text>
      <TextInput
        style={styles.input}
        placeholder="Enter city name"
        onChangeText={(text) => setCity(text)}
        value={city}
      />
      <Button title="Search" onPress={getWeather} />

      {weatherData && (
        <View style={styles.weatherInfo}>
          <Text style={styles.cityName}>
            {weatherData.name}, {weatherData.sys.country}
          </Text>
          <Text style={styles.temp}>
            Temperature: {weatherData.main.temp} °C
          </Text>
          <Text style={styles.humidity}>
            Humidity: {weatherData.main.humidity}%
          </Text>
          <Image
            style={styles.icon}
            source={{
              uri: `http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`,
            }}
          />
        </View>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  },
  title: {
    fontSize: 24,
    fontWeight: "bold",
    marginBottom: 20,
  },
  input: {
    borderWidth: 1,
    borderColor: "#ccc",
    padding: 10,
    marginBottom: 10,
    width: "80%",
  },
  weatherInfo: {
    alignItems: "center",
    marginTop: 20,
  },
  cityName: {
    fontSize: 20,
    fontWeight: "bold",
    marginBottom: 10,
  },
  temp: {
    fontSize: 18,
    marginBottom: 5,
  },
  humidity: {
    fontSize: 16,
    marginBottom: 10,
  },
  icon: {
    width: 100,
    height: 100,
  },
});


