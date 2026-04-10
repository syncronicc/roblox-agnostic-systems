# AudioService

A simple, flexible audio controller for Roblox games.

```lua
local Audio = require(AudioService)
local audio = Audio.new()
```

---

## Methods

### `service.new()`
Creates a new AudioService instance.

```lua
local audio = Audio.new()
```

---

### `:play(options, use_service?)`
Clones and plays a sound. Tracks it internally so you can stop it later. Returns the cloned `Sound`.

Pass `use_service = true` to use `SoundService:PlayLocalSound` — returns `nil` in that case and does not track the sound.

```lua
local bgm = audio:play({
    SFX    = workspace.Music,
    Looped = true,
    Volume = 0.8,
})
```

---

### `:play_once(options)`
Clones and plays a sound exactly once, then destroys it automatically. Not tracked — fire and forget.

```lua
audio:play_once({
    SFX    = sfxFolder.Explosion,
    Volume = 1,
})
```

---

### `:pause(sfx)`
Pauses a playing sound. Preserves `TimePosition` so it can be resumed.

```lua
audio:pause(bgm)
```

---

### `:resume(sfx)`
Resumes a paused sound from where it left off.

```lua
audio:resume(bgm)
```

---

### `:stop(sfx)`
Destroys the sound and removes it from internal tracking.

```lua
audio:stop(bgm)
```

---

### `:stop_all()`
Destroys every tracked sound and clears `_tracks`. Useful on round end or scene transitions.

```lua
audio:stop_all()
```

---

### `:is_playing(sfx)`
Returns `true` if the sound is currently playing.

```lua
if audio:is_playing(bgm) then
    print("music is running")
end
```

---

### `:is_paused(sfx)`
Returns `true` if the sound is not playing but has a non-zero `TimePosition`.

```lua
if audio:is_paused(bgm) then
    audio:resume(bgm)
end
```

---

### `:get_active_tracks()`
Returns a shallow copy of all currently tracked sounds. Safe to iterate — mutations don't affect the internal list.

```lua
for _, track in audio:get_active_tracks() do
    track.Volume = 0.2
end
```

---

## PlayOptions

| Field | Type | Default | Description |
|---|---|---|---|
| `SFX` | `Sound` | required | Source sound to clone |
| `Looped` | `boolean?` | `false` | Whether the sound loops |
| `Volume` | `number?` | `0.5` | Playback volume |
| `delay` | `number?` | `nil` | Seconds to wait before destroying after `.Ended` |
| `Target` | `any?` | `nil` | Parent for the cloned sound (used server-side) |

---

## License

MIT © 2026 @kts
