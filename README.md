<p align="center">
  <img src="https://i.imgur.com/FIGZdR3.png">
</p>

Current version: **2.0.17**

# About
This takes ESPN/ESPN+, FOX Sports, and MLB.tv programming and transforms it into a "live TV" experience with virtual linear channels. It will discover what is on, and generate a schedule of channels that will give you M3U and XMLTV files that you can import into something like [Jellyfin](https://jellyfin.org), [Channels](https://getchannels.com), or [xTeVe](https://github.com/xteve-project/xTeVe).

## Notes
* This was not made for pirating streams. This is made for using your own credentials and have a different presentation than the ESPN, FOX Sports, and MLB.tv apps currently provide.
* The Mouse might not like it and it could be taken down at any minute. Enjoy it while it lasts. ¯\\_(ツ)_/¯

If you're using the `USE_LINEAR` option, have your client pull the XMLTV file every 4 hours so it can stay current if channels change.

# Using
The server exposes 2 main endpoints:

| Endpoint | Description |
|---|---|
| /channels.m3u | The channel list you'll import into your client |
| /xmltv.xml | The schedule that you'll import into your client |

# Running
The recommended way of running is to pull the image from [Docker Hub](https://hub.docker.com/r/m0ngr31/eplustv).

## Environement Variables
| Environment Variable | Description | Required? | Default |
|---|---|---|---|
| START_CHANNEL | What the first channel number should be. | No | 1 |
| NUM_OF_CHANNELS | How many channels to create? This is dependent on the networks you are using. A good number to start with is >= 150 if you are using ESPN+. | No | 150 |
| USE_LINEAR | If you are using linear channels like ESPN, FS1, ect., and want those events on their own dedicated channels. | No | False |
| PROXY_SEGMENTS | Proxy keyed `*.ts` files. | No | False |
| PUID | Current user ID. Use if you have permission issues. Needs to be combined with PGID. | No ||
| PGID | Current group ID. Use if you have permission issues. Needs to be combined with PUID. | No ||
| MAX_RESOLUTION | Max resolution to use. Valid options are `UHD/HDR`, `UHD/SDR`, `1080p`, `720p`, and `540p` (Some providers don't offer 4K or 1080p and will attempt to play the highest framerate available for selected resolution). | No | UHD/SDR ||
| PORT | Port the API will be served on. You can set this if it conflicts with another service in your environment. | No | 8000 |

### Available channel options

#### ESPN
Use if you would like to login with a TV provider or ESPN+ and access various ESPN events
| Environment Variable | Description | Default |
|---|---|---|
| ESPNPLUS | Set to false if you only want events that come from your TV provider | True |
| ESPN | ESPN: Set if your TV provider supports it | False |
| ESPN2 | ESPN2: Set if your TV provider supports it | False |
| ESPN3 | ESPN2: Set if your TV provider supports it | False |
| ESPNU | ESPNU: Set if your TV provider supports it | False |
| SEC | SEC Network: Set if your TV provider supports it | False |
| SECPLUS | SEC Network+: Set if your TV provider supports it | False |
| ACCN | ACCN: Set if your TV provider supports it | False |
| ACCNX | ACCNX: Set if your TV provider supports it | False |
| LONGHORN | Longhorn Network: Set if your TV provider supports it | False |

#### FOX Sports
Use if you would like to login with a TV provider and access various FOX Sports events
| Environment Variable | Description | Default |
|---|---|---|
| FOXSPORTS | Set if your TV provider supports it | False |
| FOXSPORTS_ALLOW_REPLAYS | If you would like to schedule events that aren't live | False |

#### MLB.tv
Use if you would like to login with your MLB.tv account
| Environment Variable | Description | Default |
|---|---|---|
| MLBTV | Set if you would like to use MLB.tv | False |
| MLBTV_USER | MLB.tv Username | False |
| MLBTV_PASS | MLB.tv Password | False |
| MLBTV_ONLY_FREE | Only schedule free games | False |

## Volumes
| Volume Name | Description | Required? |
|---|---|---|
| /app/config | Used to store DB and application state | Yes |


## Docker Run
By default, the easiest way to get running is:

```bash
docker run -p 8000:8000 -v config_dir:/app/config m0ngr31/eplustv
```

If you run into permissions issues:

```bash
docker run -p 8000:8000 -v config_dir:/app/config -e PUID=$(id -u $USER) -e PGID=$(id -g $USER) m0ngr31/eplustv
```

Once it runs for the first time, check the Docker logs to see what the next steps for authentication are.
