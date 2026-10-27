# LAVRay for Windows

[![CI](https://github.com/anatolijlaptev1991-ctrl/lavray-windows/actions/workflows/ci.yml/badge.svg)](https://github.com/anatolijlaptev1991-ctrl/lavray-windows/actions/workflows/ci.yml)
![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0078D4?logo=windows)
![Arch](https://img.shields.io/badge/arch-x64-lightgrey)

**LAVRay** — кроссплатформенный клиент прокси с раздельной маршрутизацией
(наследник NekoBox/NekoRay). Это **платформенный репозиторий для Windows**:
установщик, portable-сборка, трей, системный прокси, TUN, CI и релизы.

> Общая кодовая база (Go-ядро, Rust-оболочка, React-UI) — в основном репозитории
> **[lavray](https://github.com/anatolijlaptev1991-ctrl/lavray)**, подключаемом
> git-сабмодулем на зафиксированном коммите — версия платформы всегда собирается
> из аудированного состояния общего репозитория.

---

## Установка

### Установщик (NSIS)

1. Скачайте `LAVRay_<version>_x64-setup.exe` из [Releases](../../releases).
2. Запустите. Установка — **per-machine** в `C:\Program Files\LAVRay`.
3. Запустите «LAVRay» из меню «Пуск».

> **Сборки v1 не подписаны** (решение владельца проекта). SmartScreen покажет
> «Система защитила ваш компьютер» → нажмите **«Подробнее»** → **«Выполнить в любом
> случае»**. Это ожидаемо; подлинность проверяется по SHA256 из `SHA256SUMS.txt`
> релиза.

### Portable

Распакуйте `LAVRay-portable-x64.zip` в любую папку и запустите `LAVRay.exe`.
Структура: `LAVRay.exe` + `binaries\lavray-core.exe`. Данные — в `%APPDATA%\LAVRay`.

### Проверка целостности

```powershell
Get-FileHash .\LAVRay_0.1.0_x64-setup.exe -Algorithm SHA256   # сверить с SHA256SUMS.txt
```

---

## Что умеет

- **Подключение**: системный прокси (WinINET) или TUN (права администратора).
- **Серверы**: импорт vmess/vless/ss/trojan/hysteria2/tuic-ссылок, подписки,
  QR из буфера, экспорт, поиск.
- **Маршрутизация**: 5 встроенных пресетов, включая **LAV Preset RU**
  (RU-сайты и игры напрямую; Riot-ядра через прокси; AI/Dev — через прокси).
- **Диагностика**: самопроверки, проверка маршрута адреса, публичный IP, журнал.
- **Трей**: закрытие окна прячет в трей, левый клик — открыть; выход из меню трея
  останавливает ядро. Второй запуск фокусирует существующее окно (single-instance).
- **Автообновления**: подписанные манифесты TUF-lite (ed25519, порог 2-из-3,
  защита от downgrade).

---

## Требования

| | |
|---|---|
| ОС | Windows 10 21H2+ / Windows 11 (x64) |
| WebView2 | предустановлен; на LTSC/корп. образах ставится bootstrapper'ом |
| TUN-режим | права администратора (служба) |
| RAM | ~120 МБ (оболочка ~28 + ядро ~58 МБ) |

---

## Где лежат данные

| Что | Путь |
|---|---|
| База профилей/настроек | `%APPDATA%\LAVRay\lavray.db` (SQLite WAL) |
| Ядро | `C:\Program Files\LAVRay\binaries\lavray-core.exe` |

Каталог данных пишется без админ-прав; TUN обслуживает отдельная служба.

---

## Сборка из исходников

```powershell
git clone --recurse-submodules https://github.com/anatolijlaptev1991-ctrl/lavray-windows.git
cd lavray-windows
powershell -ExecutionPolicy Bypass -File lavray\shell\build.ps1
```

Скрипт: UI (`npm ci && npm run build`) → Go-ядро (`CGO_ENABLED=0 go build`) →
`cargo tauri build --bundles nsis`. Артефакт:
`lavray\shell\target\release\bundle\nsis\LAVRay_<ver>_x64-setup.exe`.

Требования: Go 1.27+, Node 22+, Rust 1.80+, Visual Studio Build Tools (C++).

---

## CI и релизы

- `ci.yml` — линт/тесты/сборка на каждый push.
- `release-windows.yml` (основной репозиторий) — по тегу `v*`: NSIS + portable
  ZIP + `SHA256SUMS.txt`, автоприкрепление к GitHub Release.
- Платформенный CI: smoke-установка NSIS, проверка запуска ядра, ZIP-артефакт.

---

## Безопасность

Неподписанные сборки — осознанное решение v1. Целостность: SHA256 + подписанный
манифест обновлений (ed25519). Политика:
[SECURITY.md](https://github.com/anatolijlaptev1991-ctrl/lavray/blob/main/docs/SECURITY.md).

## Лицензия

[GPL-3.0](LICENSE) — производная nekoray (GPL-3.0) и sing-box.
