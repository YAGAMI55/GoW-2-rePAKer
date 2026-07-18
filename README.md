# ⚔️ GoW 2 rePAKer

**Инструмент для распаковки и перепаковки игровых архивов God of War 2 PS2 версии (DVD5/DVD9)**

[![Go Version](https://img.shields.io/badge/Go-1.26.3-blue.svg)](https://golang.org/)
[![Release](https://img.shields.io/github/v/release/YAGAMI55/GoW-2-rePAKer)](https://github.com/YAGAMI55/GoW-2-rePAKer/releases)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

---

## 📖 О проекте

**GoW 2 rePAKer** — это утилита командной строки для работы с файлами ресурсов **God of War 2**. Она позволяет:
- **Распаковывать** игровые `.PAK`‑архивы (как DVD5, так и DVD9) с полным сохранением структуры.
- **Перепаковывать** изменённые файлы обратно в `.PAK` с автоматическим обновлением `.TOC`‑таблицы.
- **Оптимизировать** итоговый архив за счёт **релинка** (замена `.psw` на `.pss`) и **дедупликации** (удаление дубликатов по CRC64).

Всё это делает моддинг игры удобнее и быстрее! 🚀

---

## ✨ Возможности

- 🔍 **Распаковка** — извлекает все файлы из `PART1.PAK` и (опционально) `PART2.PAK`, создавая рядом `_metadata.json` для последующей упаковки.
- 📦 **Перепаковка** — собирает изменённые файлы обратно в `PART1.PAK` и генерирует обновлённый `TOC`.
- 🔗 **Релинк (‑relinkpss)** — автоматически заменяет ссылки с `.psw` на `.pss` (приоритет у `.pss`), экономя место.
- 🧹 **Дедупликация (‑dedup)** — находит и удаляет идентичные файлы (по CRC64), оставляя только уникальные копии (исключая `.pss` и `.psw`).
- 🖥️ **Кроссплатформенность** — работает на Windows, Linux и macOS.

---

## 🚀 Быстрый старт

### Скачать готовый бинарник
Перейдите в раздел [Releases](https://github.com/YAGAMI55/GoW-2-rePAKer/releases) и скачайте `gow2repacker.exe` для Windows (или бинарник для вашей ОС).

### Собрать из исходников
```bash
git clone https://github.com/YAGAMI55/GoW-2-rePAKer.git
cd GoW-2-rePAKer
go build -o gow2repacker main.go
```

---

## 📌 Использование

```bash
gow2repacker <команда> [аргументы]
```

### 1️⃣ Распаковка

**Для DVD5 (только PART1.PAK):**
```bash
gow2repacker extract GODOFWAR.TOC PART1.PAK none ./EXTRACT
```

**Для DVD9 (PART1.PAK + PART2.PAK):**
```bash
gow2repacker extract GODOFWAR.TOC PART1.PAK PART2.PAK ./EXTRACT
```

> После распаковки в папке `EXTRACT` появится файл `_metadata.json` — он понадобится для перепаковки.

---

### 2️⃣ Перепаковка

**Базовая упаковка:**
```bash
gow2repacker pack ./EXTRACT PART1_NEW.PAK GODOFWAR_NEW.TOC
```

**С релинком (psw → pss):**
```bash
gow2repacker pack ./EXTRACT PART1_NEW.PAK GODOFWAR_NEW.TOC -relinkpss
```

**С релинком и дедупликацией:**
```bash
gow2repacker pack ./EXTRACT PART1_NEW.PAK GODOFWAR_NEW.TOC -relinkpss -dedup
```

---

### 3️⃣ Справка
```bash
gow2repacker help
```

---

## ⚙️ Как это работает (для тех, кому интересно)

- **TOC** — таблица смещений, в которой для каждого файла хранятся: имя, размер, ID архива, ID смещения.
- **PAK** — бинарный архив, где данные лежат посекторно (сектор = 2048 байт).
- При распаковке утилита читает TOC, вычисляет реальные смещения в PAK (с учётом `SPLITLINE` для DVD9) и извлекает файлы.
- При перепаковке она использует `_metadata.json`, применяет релинк и дедупликацию (если включены), затем записывает новые PAK и TOC.

---

## 🛠️ Разработка

### Требования
- Go 1.26.3 или новее

### Сборка под разные ОС
```bash
GOOS=windows GOARCH=amd64 go build -o gow2repacker.exe main.go
GOOS=linux GOARCH=amd64 go build -o gow2repacker main.go
GOOS=darwin GOARCH=amd64 go build -o gow2repacker main.go
```

### GitHub Actions
В репозитории уже настроен автоматический билд при создании тега (например, `v1.0.0`). После пуша тега Actions скомпилирует `.exe` и прикрепит его к релизу.

---

## 📄 Лицензия
Распространяется под лицензией **MIT**. Подробнее в файле `LICENSE`.

---

## 🙏 Благодарности
Создано с ❤️ для сообщества моддеров God of War 2.
## Спасибо mogaika за исходники и gowbrowser
## https://github.com/mogaika/god_of_war_browser

---

⭐ Если проект был полезен, поставьте звёздочку на GitHub!
