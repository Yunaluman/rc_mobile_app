# IoT Monitoring App

**English** · [Français](README.fr.md)

## Overview

This Flutter app monitors and configures a connected IoT system (DHT11 sensor) in real time. It connects to a Firebase Realtime Database to display temperature and humidity data, chart their evolution over time, and configure alert thresholds with hysteresis.

## Main features

- **Current state**: real-time temperature and humidity
- **Trend chart**: sensor data over several time ranges
- **Threshold configuration**: temperature limits with hysteresis
- **Alert history**: paginated list of threshold-crossing events
- **Offline mode**: data persistence through Firebase
- **Auto refresh**: real-time data updates

## Technical architecture

- **Frontend**: Flutter (Material Design 3)
- **Backend**: Firebase Realtime Database
- **State**: Provider
- **Charts**: fl_chart
- **Data model**: dedicated models for each data type

## Installation

### Requirements

- Flutter SDK (^3.6.0)
- Firebase account
- Android Studio / VS Code

### Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/Yunaluman/rc_mobile_app.git
   cd rc_mobile_app
   ```

2. **Install dependencies**

   ```bash
   flutter pub get
   ```

3. **Configure Firebase**

   Add your `firebase_options.dart` file at the project root with:
   - apiKey
   - databaseURL
   - the other fields required by FirebaseOptions

4. **Run the app**

   ```bash
   flutter run
   ```

## Project structure

```
lib/
  ├── main.dart              # App entry point
  ├── firebase_options.dart  # Firebase configuration
  ├── models/                # Data models
  │    ├── current_state.dart
  │    ├── sensor_reading.dart
  │    ├── threshold_event.dart
  │    └── time_filter.dart
  ├── services/              # Business logic services
  │    ├── firebase_service.dart
  │    ├── sensor_service.dart
  │    ├── current_state_service.dart
  │    ├── sensor_reading_service.dart
  │    └── threshold_event_service.dart
  ├── utils/                 # Utilities
  │    └── map_converter.dart
  ├── screens/               # Main screens
  │    └── home_screen.dart
  └── widgets/               # Reusable widgets
       ├── current_state_widget.dart
       ├── sensor_readings_chart.dart
       ├── threshold_events_widget.dart
       └── threshold_config_widget.dart
```

## Usage

### Main screen

The main screen has several sections:

1. **Current state**: current temperature and humidity, color-coded against the thresholds
2. **Threshold configuration**: adjust the temperature alert thresholds
3. **Trend chart**: data evolution with several time filters
4. **Event list**: history of threshold crossings

### Threshold configuration

Set a target temperature with hysteresis:

- Turn on edit mode with the switch
- Use the slider to set the target temperature
- High (+1 °C) and low (−1 °C) thresholds are computed automatically
- Confirm with the "Save" button

## Data models

### CurrentState

Current sensor state:

- `temperature`: current temperature (°C)
- `humidity`: current humidity (%)
- `thresholdLow`: low temperature threshold
- `thresholdHigh`: high temperature threshold
- `lastUpdate`: last update date
- `isOverThreshold`: whether the temperature is outside the thresholds

### SensorReading

A historical sensor reading:

- `temperature`: measured temperature (°C)
- `humidity`: measured humidity (%)
- `timestamp`: date and time of the measurement

### ThresholdEvent

A threshold-crossing event:

- `temperature`: temperature at the time of the event (°C)
- `humidity`: humidity at the time of the event (%)
- `timestamp`: date and time of the event
- `eventType`: event type (crossing, back to normal)
- `thresholdHigh`: high threshold at the time of the event
- `thresholdLow`: low threshold at the time of the event

## Services

### FirebaseService

Base service for talking to Firebase Realtime Database:

- Connection initialization
- CRUD methods
- Data stream handling
- Connectivity checks
- Safe data type conversion

### SensorService

Main service coordinating all sensor services:

- Initializes the specialized services
- Manages app state
- Unified data access interface
- Methods to update thresholds and create events

### CurrentStateService

Handles the current sensor state:

- Fetching and updating state
- Temperature threshold configuration
- Threshold-crossing monitoring

### SensorReadingService

Handles historical sensor readings:

- Fetching data by time filter
- Processing and filtering readings

### ThresholdEventService

Handles threshold-crossing events:

- Creating and fetching events
- Notifying new events

## Hysteresis

The system uses hysteresis to avoid rapid flapping between states:

1. The user sets a target temperature (e.g. 23 °C)
2. The system applies a hysteresis margin automatically (±1 °C)
3. The high threshold is set to 24 °C and the low threshold to 22 °C
4. A crossing event is created only when:
   - the temperature rises above the high threshold (> 24 °C)
   - the temperature drops below the low threshold (< 22 °C)
5. The status returns to normal only once the temperature is back within the acceptable range

This avoids false positives and spurious alerts on small temperature fluctuations.

## Optimizations and robustness

- Safe conversion of Firebase data types
- Error and connectivity handling
- Data caching for offline mode
- Smart data refresh
- Detailed debug logs

## Possible improvements

- User authentication
- Push notifications on threshold crossings
- Support for other sensor types
- Historical data export
- Advanced admin interface

## License

Distributed under the MIT license. See the LICENSE file for details.

---

Built with ❤️ by Yunaluman THERESE
