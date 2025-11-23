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
