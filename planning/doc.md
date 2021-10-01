# Documentation of stuff

## General Architecture
- Information is exchanged with both full world snapshots and relative update snapshots
- Snapshots are sent and received every five ticks
- The game runs at 60 ticks/s
- Server runs a few ticks in the future to ensure ok-ish playability

### Server
- Final authority on the game state
- Processes the game logic
- Initially sends a full world snapshot at the start of the game
- Continues to send update snapshots with changes
- Sends a full world snapshot on request
- Lag compensation (events are processed at the tick time they happen rather than the current server time)

### Client
- Uses interpolation between snapshots therefore it runs further in the past
- The client also processes game logic to make predictions focussing on character movements and blob-eating
- Renders the game using geogebra
- Uses websockets, I guess

## Message Protocol
General Message Format (GeMesFt): 2B Message length, 1B message subprotocol, nB opaque message data
### Snapshots
Player structure:	1B Player ID, 2B x-coord, 2B y-coord, 2B radius
Blob structure:		2B Blob ID, 2B x-coord, 2B y-coord
Blob eaten structure: 2B Blob ID

Full world snapshot: 2B Players length, <Players length>B player structures, 2B Blobs length, <Blobs length>B blob structures
Update snapshot:	2B Players length, <Players length>B player structures, 2B Blobs created length, <Blobs created length>B blob structures, 2B Blobs eaten length, <Blobs eaten length>B blobs eaten structures

### Client commands
Command structure: 1B Command Type, nB opaque data structure 
Player movement structure (1): 1B Player ID, 2B dx, 2B dy

Command message: 2B Commands length, <Commands length>B command structures

### Event message
