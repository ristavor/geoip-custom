# GeoIP for V2Ray

This project automatically weekly releases GeoIP files for routing purpose in Project V. It also provides a command line interface(CLI) for users to customize their own GeoIP files.

These two concepts are notable: `input` and `output`. The `input` is the data source, whereas the `output` is the destination of the converted data. What the CLI does is to aggregate all `input` format data, then convert them to `output` format by using the options in the config file.

## Download links

- **geoip.dat**：[https://github.com/v2fly/geoip/releases/latest/download/geoip.dat](https://github.com/v2fly/geoip/releases/latest/download/geoip.dat)
- **geoip.dat.sha256sum**：[https://github.com/v2fly/geoip/releases/latest/download/geoip.dat.sha256sum](https://github.com/v2fly/geoip/releases/latest/download/geoip.dat.sha256sum)
- **geoip-only-cn-private.dat**：[https://github.com/v2fly/geoip/releases/latest/download/geoip-only-cn-private.dat](https://github.com/v2fly/geoip/releases/latest/download/geoip-only-cn-private.dat)
- **geoip-only-cn-private.dat.sha256sum**：[https://github.com/v2fly/geoip/releases/latest/download/geoip-only-cn-private.dat.sha256sum](https://github.com/v2fly/geoip/releases/latest/download/geoip-only-cn-private.dat.sha256sum)
- **cn.dat**：[https://github.com/v2fly/geoip/releases/latest/download/cn.dat](https://github.com/v2fly/geoip/releases/latest/download/cn.dat)
- **cn.dat.sha256sum**：[https://github.com/v2fly/geoip/releases/latest/download/cn.dat.sha256sum](https://github.com/v2fly/geoip/releases/latest/download/cn.dat.sha256sum)
- **private.dat**：[https://github.com/v2fly/geoip/releases/latest/download/private.dat](https://github.com/v2fly/geoip/releases/latest/download/private.dat)
- **private.dat.sha256sum**：[https://github.com/v2fly/geoip/releases/latest/download/private.dat.sha256sum](https://github.com/v2fly/geoip/releases/latest/download/private.dat.sha256sum)

## GeoIP usage example in V2Ray

```json
"routing": {
  "rules": [
    {
      "type": "field",
      "outboundTag": "Direct",
      "ip": [
        "223.5.5.5/32",
        "119.29.29.29/32",
        "180.76.76.76/32",
        "114.114.114.114/32",
        "geoip:cn",
        "geoip:private",
        "ext:cn.dat:cn",
        "ext:private.dat:private",
        "ext:geoip-only-cn-private.dat:cn",
        "ext:geoip-only-cn-private.dat:private"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Proxy-1",
      "ip": [
        "1.1.1.1/32",
        "1.0.0.1/32",
        "8.8.8.8/32",
        "8.8.4.4/32"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Proxy-2",
      "ip": [
        "geoip:us",
        "geoip:ca"
      ]
    },
    {
      "type": "field",
      "outboundTag": "Proxy-3",
      "ip": [
        "geoip:hk",
        "geoip:mo",
        "geoip:tw",
        "geoip:jp",
        "geoip:sg"
      ]
    }
  ]
}
```

## Generate GeoIP files manually

- Install `golang` and `git`
- Clone project code: `git clone https://github.com/v2fly/geoip.git`
- Navigate to project root directory: `cd geoip`
- Download `GeoLite2-Country-CSV.zip` from [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/), then unzip it to `geolite2` directory
- Install project dependencies: `go mod download`
- Generate files: `go run ./`

`go run ./` will use `config.json` in current directory as the default config file. The generated files are located at `output` directory by default.

Run `go run ./ -h` for more usage information. See `config-example.json` for more configuration options.

## CLI showcase

```bash
$ ./geoip -h
Usage of ./geoip:
  -c string
    	Path to the config file (default "config.json")
  -l	List all available input and output formats

$ ./geoip -l
All available input formats:
  - maxmindGeoLite2CountryCSV (Convert MaxMind GeoLite2 country CSV data to other formats)
  - text (Convert plaintext data to other formats)
  - private (Convert LAN and private CIDR to other formats)
  - test (Convert specific CIDR to other formats (for test only))
All available output formats:
  - v2rayGeoIPDat (Convert data to V2Ray GeoIP dat format)
  - text (Convert data to plaintext format)
```

## Notice

This product includes GeoLite2 data created by MaxMind, available from [MaxMind](http://www.maxmind.com).

## License

[CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/)
