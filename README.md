# 📘 README — Экспорт анимаций из Adobe Animate в Onion Runtime (Defold)

Этот документ описывает короткий рабочий процесс подготовки анимаций в **Adobe Animate**, экспорт в формат Onion, конвертацию в бинарные `.onionanim` / `.onionsymbol` и импорт в **Defold**.

---

# 🚀 1. Установка JSFL-скриптов

Скопировать папки:

- `Commands/`
- `Onion/`

в директорию Adobe Animate:

### **macOS**
```
~/Library/Application Support/Adobe/Animate
```

После этого в Animate появится меню:

```
Commands → Onion_Export
```

---

# 🎨 2. Подготовка проекта в Adobe Animate

В **Library** нужно вручную создать **строго такие папки**:

```
ANIMATIONS
EXPORT_ANIMATIONS
EXPORT_SYMBOLS
EXTERNAL_SYMBOLS
GRAPHICS
MOVIE
```

## 📌 2.1 Подготовка графики

1. В папку **GRAPHICS** положить исходные изображения.
2. Для каждой графики создать **Symbol → Graphic**.
3. Внутри символа должны быть два слоя:
   - `graphics` — сама графика
   - `FRAME` — слой с типом **Guide**
4. Эти graphic-символы перенести в **EXPORT_SYMBOLS**.

---

# 🎬 3. Подготовка анимаций

1. В папку **EXPORT_ANIMATIONS** перетащить нужные символы из `EXPORT_SYMBOLS`.
2. Анимировать как обычно.

Внутри каждого экспортируемого MovieClip должны быть **два обязательных слоя**:

- **ENTITY** — Label с названием сущности  
- **ANIMATION** — Label с названием анимации  

Оба слоя должны длиться до конца всей анимации.

---

# 📤 4. Экспорт из Animate

Открыть:

```
Commands → Onion_Export
```

Параметры:

- Symbol Group Name — обычно имя файла
- Vector Graphics Prefix — то же
- ✓ Export PNG
- ✓ Export symbols uses in animations

В результате будут сгенерированы:

```
animations.json
symbols.json
```

---

# 🔧 5. Конвертация JSON → Onion бинарников

Перейти в:

```
tools/animate_export_support
```

### **Анимации**
```
python convert_animations.py -i data_for_export/animations.json -o data_for_export/animations.onionanim
```

### **Символы**
```
python convert_symbols.py -i data_for_export/symbols.json -o data_for_export/symbols.onionsymbol
```

---

# 🧩 6. Импорт в Defold

Создать файл:

```
*.onion.scene
```

Указать в нём:

- путь к `animations.onionanim`
- путь к `symbols.onionsymbol`
- путь к атласу PNG-графики

Добавить компонент **Onion Model** и назначить сцену.

---

# ▶ 7. Пример запуска анимации

```lua
local model_url = "#onionmodel"
onion.set_entity(model_url, "entity_name")
onion.set_symbol_group(model_url, "symbol_group_name")
onion.play_animation(model_url, "loop", go.PLAYBACK_LOOP_PINGPONG, { offset = 0.8 })
```

---
