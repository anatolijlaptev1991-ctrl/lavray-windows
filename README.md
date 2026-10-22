# LAVRay для Windows

Платформенный репозиторий LAVRay на Windows: упаковка, системная интеграция, CI и релизы.
Общая кодовая база (Go-ядро, Rust/Tauri-оболочка, React UI) живёт в
[**lavray**](https://github.com/anatolijlaptev1991-ctrl/lavray) и подключается сюда
git-сабмодулем на зафиксированном коммите — версия платформы всегда собирается из
аудированного состояния общего репозитория.

## Подключение общего репозитория

```bash
git submodule add https://github.com/anatolijlaptev1991-ctrl/lavray.git lavray
git submodule update --init --recursive
```

Обновление сабмодуля — отдельный PR: новый пин = новый коммит `lavray` (никогда не ветка).

## Зона ответственности этого репозитория

- **Упаковка:** установщик **NSIS** + портативный **ZIP** (установка «в Program Files» и
  распаковка без установки — оба официальных артефакта релиза).
- **Трей:** иконка-шеврон 16px (тёмная/светлая), меню подключения, статус-«луч», balloon/Toast
  уведомления.
- **Автозапуск:** ключ реестра Run / планировщик (с учётом UAC).
- **Системный прокси:** управление настройками WinInet (прокси на 127.0.0.1), корректный
  откат при аварийном выходе.
- **TUN / UAC:** wintun-интеграция, повышение прав только на включение TUN (запуск от
  администратора не обязателен для обычного режима), манифест с `requestedExecutionLevel`.
- **Deep-link:** схема `lavray://` (импорт подписок/профилей из браузера, реакции «Центра
  обновлений»).
- **Автообновления:** Tauri updater + NSIS-мажорные версии (см. `lavray/docs/UPDATE-SYSTEM.md`:
  подпись артефактов, откат, каналы stable/beta).
- **CI/релизы:** GitHub Actions — сборка на `windows-latest`, подпись, чексуммы, черновики
  релизов.

## Статус

Этап 2 (дизайн, аудит) завершён в общем репозитории; платформенная реализация — Этап 3.
Здесь пока каркас: CI-заготовка и шаблоны issues.

## Ссылки

- Общий репозиторий: https://github.com/anatolijlaptev1991-ctrl/lavray
- Документация: `lavray/docs/` (REQUIREMENTS — раздел 15 «Системная интеграция», UPDATE-SYSTEM).
- Сестринские платформы: [lavray-linux](https://github.com/anatolijlaptev1991-ctrl/lavray-linux) ·
  [lavray-macos](https://github.com/anatolijlaptev1991-ctrl/lavray-macos) ·
  [lavray-openwrt](https://github.com/anatolijlaptev1991-ctrl/lavray-openwrt)

## Лицензия

[GPL-3.0](LICENSE), унаследована от LAVRay/nekoray.
