# roblox
Python Roblox module, primarily focused on the game client.

Since this module doesn't directly hook into the client, it is limited by the following things:
- It can only interact with one client at a time
- Keystrokes may fail to register in time, causing chat messages to cut off

DM me if you know of a way to send keystrokes to the client, without focusing the window.

# Usage
```python
from roblox import Roblox, RobloxClient, RobloxClientMutex
from time import sleep

mutex = RobloxClientMutex() # allows for multiple clients to be open at once

with open("cookie.txt") as f:
  session = Roblox(f.read().strip())

client = RobloxClient(session, 1818) # third argument can be a jobId
client.wait_for(15) # wait up to 15 seconds for game to load

client.chat_message("burger")
sleep(1)

client.screenshot().show()
client.close()
```

# Documentation

### RobloxClient(session, place_id, job_id=None, client_path=None)
Launches a new client instance.

### RobloxClient.wait_for(timeout=15, check_interval=0.25)
Waits until the client is past the loading screen. It uses the screenshot method and therefore may not be 100% reliable.

### RobloxClient.ping(match_job_id=False) -> bool
Checks if the user is currently in-game using the presence web-api, can be used as a kind of "ping" to check if the client has disconnected from the game.

### RobloxClient.screenshot() -> PIL.Image
Returns a `PIL.Image` screenshot of the client in it's current window size.

### RobloxClient.chat_message(message)
Attempts to write and send a chat message by simulating keystrokes on the client.

### RobloxClient.close()
Kills the client process.
