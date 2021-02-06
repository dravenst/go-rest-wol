# HTTP Server for Remote WOL Requesting from a CSV Computer List

A HTTP server that sends a Wake On LAN package on an HTTP request.

### Docker Image

https://hub.docker.com/repository/docker/dravenst/go-rest-wol

### GitHub Repository
https://github.com/dravenst/go-rest-wol

### Modifed to use simpler default UI for easy usage

![Screenshot](https://github.com/dravenst/go-rest-wol/raw/master/screenshot.PNG)

### Simple REST API to let a machine wake someone up

/api/wakeup/computer/**&lt;hostname&gt;** -  Returns a JSON object

```json
{
  "success":true,
  "message":"Succesfully Wakeup Computer Computer1 with Mac 64-07-2D-BB-BB-BF on Broadcast IP 192.168.10.254:9",
  "error":null
}
```

## Enviroment Variables

| Variable Name | Description |
| ------------- | ------------------------------------------------------------------------------- |
| WOLFILE       | Define the port on which the webserver will listen to **(Default: 8080)**       |
| WOLHTTPPORT   | Path to the CSV file containing the list of hosts **(Default: .\computer.csv)** |


## Commandline arguments

| Commandline argument | Example          | Description                                                                            |
| -------------------- | ---------------- | -------------------------------------------------------------------------------------- |
| --port               | --port 80        | Define the port on which the webserver will listen to **(Default: 8080)**              |
| --file               | --file comp.csv  | Path to the CSV file containing the list of hosts **(Default: .\computer.csv)**        |

## Computer list file CSV layout

### Columns
__&lt;name of the computer&gt;__,__&lt;mac address of the computer&gt;__,__&lt;broadcast ip to send the magic packet&gt;__


### Example
```csv
name,mac,ip
Computer1,64-07-2D-BB-BB-BF,192.168.10.254:9
Computer2,2D-F2-3D-06-17-00,192.168.10.254:9
Computer3,FF-B3-95-62-1C-DD,192.168.10.254:9
```

### Recommended Synology/NAS Settings
<ul>
<li>Update network_mode=host in container config (host mode was required to get my Synology NAS to work)
<li>Add volume mapping to directory where you maintain/update your own custom computer.csv file (e.g. /docker/wol -> /data)
<li>Add WOLFILE container config environment variable to map to the computer.csv location (e.g. /data/computer.csv)
<li>Add WOLHTTPPORT container config environment variable with unused port on Synology/NAS (e.g. 28080)
<li>Add Control Panel -> Application Portal -> Reverse Proxy to support port mapping to this port for new hostname (e.g. wol.example.com -> localhost:28080 on both http and https)
</ul>

Many Thanks to https://github.com/dabondi/go-rest-wol https://github.com/sabhiram/go-wol for starting this effort and the WOL code!
