# 🧩 Onion Runtime — Lua API Overview

Ниже приведена краткая документация по API Onion Runtime вместе с рабочими примерами использования.  
Этот документ дополняет основной README и предназначен для разработчиков, использующих Onion Model в Defold.

---

# 📘 1. Основные функции API

## ▶ `onion.play_animation(url, animation_id, playback, options?, callback?)`
Проигрывает анимацию.

**Пример:**
```lua
onion.play_animation("#onionmodel", "loop", go.PLAYBACK_LOOP_FORWARD, { offset = 0.5 })
```

С коллбеком:
```lua
local function cb(self, msg_id, message, sender)
    print(msg_id, message)
end

onion.play_animation("#onionmodel", "shake", go.PLAYBACK_ONCE_FORWARD, nil, cb)
```

---

## ▶ `onion.set_entity(url, entity_id)`
Устанавливает активную сущность (ENTITY label в Animate).

```lua
onion.set_entity("#onionmodel", "clock")
```

---

## ▶ `onion.set_symbol_group(url, group_id)`
Устанавливает группу символов (скин).

```lua
onion.set_symbol_group("#onionmodel", "clock_gold")
```

---

## ▶ Управление символами и слоями

### Скрыть/показать слой:
```lua
onion.hide_layer("#onionmodel", "fx")
onion.show_layer("#onionmodel", "body")
```

### Скрыть/показать symbol:
```lua
onion.hide_symbol("#onionmodel", "arrow")
onion.show_symbol("#onionmodel", "arrow")
```

### Показать всё:
```lua
onion.show_all_layers("#onionmodel")
onion.show_all_symbols("#onionmodel")
```

---

## ▶ Override символов (замена графики)

```lua
onion.override_symbol("#onionmodel", "arrow", "gear3", "clock_gold")
```

Очистить:
```lua
onion.clear_override_symbol("#onionmodel", "arrow")
onion.clear_all_override_symbols("#onionmodel")
```

---

## ▶ Override группы символов (скин-миксы)

```lua
onion.add_override_symbol_group("#onionmodel", "shards")
onion.clear_override_symbol_group("#onionmodel", "clock")
onion.clear_all_override_symbol_groups("#onionmodel")
```

---

## ▶ Флипы

```lua
onion.set_hflip("#onionmodel", true)
onion.set_vflip("#onionmodel", true)
```

---

## ▶ Blend Mode

```lua
onion.set_blend_mode("#onionmodel", onion.BLEND_ADD)
local m = onion.get_blend_mode("#onionmodel")
```

---

# 📦 2. Пример полноценного игрового скрипта

```lua
local fps = require("main.fps")

local function animation_callback(context, message_id, message, sender)
    if message_id == hash("onion_animation_event") then
        print("EVENT:", sender)
        pprint(message)
    elseif message_id == hash("onion_animation_done") then
        print("DONE:", sender)
        pprint(message)
    end
end

local function spawn_clock_loop(x, y, z)
    local url = factory.create("#clock_loop_factory", vmath.vector3(x, y, z))
    local model_url = msg.url(nil, url, "onionmodel")

    onion.set_entity(model_url, "clock")
    onion.set_symbol_group(model_url, "clock")
    onion.play_animation(model_url, "loop", go.PLAYBACK_LOOP_FORWARD, { offset = 0.8 })

    onion.add_override_symbol_group(model_url, "shards")
end

local function spawn_clock_shake(x, y, z)
    local url = factory.create("#clock_shake_factory", vmath.vector3(x, y, z))
    local model_url = msg.url(nil, url, "onionmodel")

    onion.set_entity(model_url, "clock")
    onion.set_symbol_group(model_url, "clock")
    onion.play_animation(model_url, "shake", go.PLAYBACK_ONCE_FORWARD, { offset = 0.2 }, animation_callback)

    go.set(model_url, "dissolve", vmath.vector4(0.35))
end

local function spawn_clock_fx(x, y, z)
    local url = factory.create("#clock_fx_factory", vmath.vector3(x, y, z))
    local model_url = msg.url(nil, url, "onionmodel")

    onion.set_entity(model_url, "clock")
    onion.set_symbol_group(model_url, "clock")
    onion.play_animation(model_url, "loop_fx", go.PLAYBACK_LOOP_FORWARD)
end

local function spawn_clock_skin(x, y, z)
    local url = factory.create("#clock_fx_factory", vmath.vector3(x, y, z))
    local model_url = msg.url(nil, url, "onionmodel")

    onion.set_entity(model_url, "clock")
    onion.set_symbol_group(model_url, "clock_gold")
    onion.play_animation(model_url, "loop_fx", go.PLAYBACK_LOOP_FORWARD, { offset = 0.5 })
end

local function spawn_clock_mix(x, y, z)
    local url = factory.create("#clock_fx_factory", vmath.vector3(x, y, z))
    local model_url = msg.url(nil, url, "onionmodel")

    onion.set_entity(model_url, "clock")
    onion.set_symbol_group(model_url, "clock")
    onion.play_animation(model_url, "loop_fx", go.PLAYBACK_LOOP_FORWARD, { offset = 0.3 })

    onion.override_symbol(model_url, "arrow", "gear2", "clock")
    onion.override_symbol(model_url, "gear1", "gear3", "clock_gold")
    onion.override_symbol(model_url, "gear2", "gear4", "clock_gold")
    onion.hide_symbol(model_url, "body")
end

function init(self)
    fps:init()

    factory.create("#clock_gold_skin_resource_factory")
    factory.create("#shards_resource_factory")

    spawn_clock_loop(-200, 200, 0)
    spawn_clock_fx(0, 200, 0)
    spawn_clock_shake(200, 200, 0)
    spawn_clock_skin(0, 0, 0)
    spawn_clock_mix(200, 0, 0)
end
```

---

Если хочешь, я могу:

✅ объединить API и базовый README в один файл  
✅ сделать расширенную документацию  
✅ сделать документацию в стиле Defold manual  

Скажи — и я соберу!