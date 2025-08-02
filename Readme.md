# Virtual Audio Bridge for Home Assistant

[![GitHub Release][releases-shield]][releases]
[![GitHub Activity][commits-shield]][commits]
[![License][license-shield]](LICENSE)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]

## About

Virtual Audio Bridge creates virtual audio sink devices that are visible to Home Assistant OS and any applications running on it. When audio is played to these virtual sinks, it's automatically converted to HTTP streams that can be consumed by Music Assistant, network audio players, or any device that can play from a stream URL.

**Perfect for:**
- Streaming audio from Amniotic ambient sound mixer
- Creating custom audio zones
- Bridging local audio to network players
- Multi-room audio setups
- Converting any Home Assistant audio to network streams

## Features

- 🎵 **Multiple Virtual Audio Sinks** - Create as many audio zones as needed
- 🌐 **HTTP Streaming** - Each sink streams over HTTP in WAV format
- 🏠 **Home Assistant Integration** - Auto-creates media player entities
- 🎛️ **Music Assistant Compatible** - Stream URLs work as radio stations
- ⚙️ **Highly Configurable** - Custom sample rates, channels, ports
- 🔄 **Real-time Streaming** - Low-latency audio streaming
- 📱 **MQTT Discovery** - Automatic device discovery in Home Assistant

## Installation

### Via HACS (Recommended)

1. Add this repository to HACS as a custom repository:
   - Repository: `https://github.com/synssins/homeassistant-virtual-audio-bridge`
   - Category: Add-on Repository
2. Install the "Virtual Audio Bridge" add-on
3. Configure the add-on (see Configuration section)
4. Start the add-on

### Manual Installation

1. Clone this repository into your Home Assistant `addons` folder
2. Refresh the Add-on Store
3. Install "Virtual Audio Bridge"

## Configuration

### Basic Configuration

```yaml
audio_sinks:
  - name: "Living Room Audio"
    sink_name: "living_room_sink"
    description: "Virtual sink for living room"
    sample_rate: 44100
    channels: 2
    stream_port: 8765
    stream_path: "/living_room"
    
  - name: "Kitchen Audio"
    sink_name: "kitchen_sink" 
    description: "Virtual sink for kitchen"
    sample_rate: 44100
    channels: 2
    stream_port: 8766
    stream_path: "/kitchen"

stream_format: "wav"
stream_quality: "cd"
auto_create_media_players: true
mqtt_discovery: true
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `audio_sinks` | list | *required* | List of virtual audio sinks to create |
| `audio_sinks[].name` | string | *required* | Display name for the sink |
| `audio_sinks[].sink_name` | string | *required* | Internal sink name (must be unique) |
| `audio_sinks[].description` | string | optional | Description for the sink |
| `audio_sinks[].sample_rate` | int | 44100 | Audio sample rate (8000-192000) |
| `audio_sinks[].channels` | int | 2 | Number of audio channels (1-8) |
| `audio_sinks[].stream_port` | int | *required* | HTTP port for streaming (8000-65535) |
| `audio_sinks[].stream_path` | string | "/stream" | URL path for the stream |
| `stream_format` | string | "wav" | Audio format (wav/mp3/flac) |
| `stream_quality` | string | "cd" | Quality preset (low/medium/high/cd) |
| `auto_create_media_players` | bool | true | Auto-create HA media player entities |
| `mqtt_discovery` | bool | true | Enable MQTT discovery |

## Usage

### 1. Setting Default Audio Device

After starting the addon, you can set a virtual sink as the default audio device:

```bash
# List available sinks
pactl list short sinks

# Set virtual sink as default
pactl set-default-sink living_room_sink
```

### 2. Accessing Streams

Each virtual sink creates an HTTP stream accessible at:
```
http://homeassistant.local:PORT/PATH
```

For example:
- Living Room: `http://homeassistant.local:8765/living_room`
- Kitchen: `http://homeassistant.local:8766/kitchen`

### 3. Using with Music Assistant

1. In Music Assistant, go to Settings → Radio
2. Add a new radio station with the stream URL
3. The stream will appear as a playable radio station

### 4. Using with Amniotic

1. Configure Amniotic to use one of the virtual sinks
2. Audio from Amniotic will automatically stream to the configured URL
3. Play the stream on any network audio device

### 5. Home Assistant Media Players

If `auto_create_media_players` is enabled, media player entities are automatically created:
- `media_player.virtual_audio_bridge_living_room_sink`
- `media_player.virtual_audio_bridge_kitchen_sink`

## Troubleshooting

### Audio Not Streaming

1. Check addon logs for errors
2. Verify the virtual sink was created:
   ```bash
   pactl list short sinks
   ```
3. Test if audio is flowing:
   ```bash
   pactl list sink-inputs
   ```

### Cannot Access Stream URLs

1. Ensure ports are not blocked by firewall
2. Check if Home Assistant is accessible on the configured ports
3. Verify the addon is running without errors

### Poor Audio Quality

1. Increase sample rate in configuration
2. Check network bandwidth
3. Try different stream formats

### Multiple Instances

To run multiple virtual sinks, simply add more entries to the `audio_sinks` configuration list. Each sink operates independently.

## Advanced Configuration

### Custom Audio Routing

```bash
# Route specific application to virtual sink
pactl move-sink-input <INPUT_ID> <SINK_NAME>

# Monitor audio levels
pactl list sinks | grep -A 15 <SINK_NAME>
```

### Stream Testing

```bash
# Test stream with curl
curl http://homeassistant.local:8765/living_room > test_audio.wav

# Test with VLC
vlc http://homeassistant.local:8765/living_room
```

## Support

- 📖 [Documentation](https://github.com/synssins/homeassistant-virtual-audio-bridge/wiki)
- 🐛 [Issue Tracker](https://github.com/synssins/homeassistant-virtual-audio-bridge/issues)
- 💬 [Discussions](https://github.com/synssins/homeassistant-virtual-audio-bridge/discussions)

## Contributing

Contributions are welcome! Please read the [contributing guidelines](CONTRIBUTING.md) first.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

[releases-shield]: https://img.shields.io/github/release/synssins/homeassistant-virtual-audio-bridge.svg
[releases]: https://github.com/synssins/homeassistant-virtual-audio-bridge/releases
[commits-shield]: https://img.shields.io/github/commit-activity/y/synssins/homeassistant-virtual-audio-bridge.svg
[commits]: https://github.com/synssins/homeassistant-virtual-audio-bridge/commits/main
[license-shield]: https://img.shields.io/github/license/synssins/homeassistant-virtual-audio-bridge.svg
[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg