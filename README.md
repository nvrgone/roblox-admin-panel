# RU-ru

# Многофункциональный парсер команд (Админ-панель)

Реализация кастомной админ-панели, ориентированной на управление через текстовые команды (Cmd). Проект написан на языке **Luau** в **Roblox Studio** с упором на сетевую безопасность и защиту от читеров.

## 🚀 Функционал и возможности
* **Базовые команды:** Из начальных команд доступны `speed`, `jump`, `kill`, `heal`, `kick`, `clear`.
* **Гибкий таргетинг:** Поддержка аргументов и выбора цели (например: `speed [num] [PlayerName]`).
* **Плавный UI (TweenService):** Панель адаптирована под удобное использование — её можно плавно открывать и скрывать по нажатию горячей клавиши `H` с помощью красивых анимаций `TweenService`.
* **История команд (Command History / Logs):** Система сохраняет логи выполненных команд, позволяя отслеживать историю текущей сессии.
* **Информативный UI (Валидация ввода):** Реализована визуальная обработка ошибок. Если команда введена неверно или цель не найдена, интерфейс выводит понятное уведомление красным цветом (`Command not found` / `Player not found`).
* **Легкая расширяемость:** Логика написана так, что в код можно быстро дописать собственные условия для новых команд.

## 🛡️ Архитектура и безопасность (Anti-Exploit)
Доступ к панели жестко разграничен по таблице `User ID` администраторов. Защита построена по принципу **100% Server-Side Protection**:

1. **Изоляция интерфейса (ServerStorage):** Сам `AdminGui` хранится на сервере в `ServerStorage`. При входе игрока сервер проверяет его ID, и только подтвержденным админам клонирует GUI в `PlayerGui`. Обычные игроки и читеры физически не имеют доступа к файлу интерфейса.
2. **Связь через ReplicatedStorage:** Клиент-серверное взаимодействие реализовано через удаленные события (`RemoteEvents`), расположенные в общей зоне видимости.
3. **Двухуровневая валидация:** 
   * *На клиенте:* `LocalScript` делает первичную проверку для корректного отображения интерфейса (барьер для базового exploit-софта).
   * *На сервере:* При каждом вызове команды сервер заново проверяет `User ID` отправителя по своей таблице. Даже если читер попытается напрямую вызвать `RemoteEvent`, сервер мгновенно отклонит запрос.
     
# EN-en     

# Multifunctional Command Parser (Admin Panel)

A custom text-command based admin panel (`Cmd`) developed in **Roblox Studio** using **Luau**. The project is built with a strong emphasis on network security, exploit prevention, and smooth UX.

## 🚀 Features & Capabilities
* **Built-in Commands:** Out-of-the-box support for initial commands: `speed`, `jump`, `kill`, `heal`, `kick`, and `clear`.
* **Advanced Targeting:** Supports arguments and dynamic target selection (e.g., `speed [num] [PlayerName]`).
* **Smooth UI (TweenService):** The panel features fluid animations via `TweenService` and can be toggled open/close smoothly by pressing the `H` hotkey.
* **Command Logs:** Tracks and saves command history, allowing admins to view the active session logs.
* **Error Handling & Input Validation:** Visual feedback for incorrect inputs. Invalid commands or missing targets trigger distinct red error messages (`Command not found` / `Player not found`).
* **Highly Extensible:** The codebase is structured to easily allow adding custom conditions and new commands.

## 🛡️ Architecture & Security (Anti-Exploit)
Access is strictly managed via a server-side `User ID` whitelist. Security follows **100% Server-Side Protection** best practices:

1. **Interface Isolation (ServerStorage):** `AdminGui` is stored securely inside `ServerStorage`. Upon player connection, the server validates their ID and clones the GUI into `PlayerGui` *only* for verified admins. Regular players and exploiters have zero physical access to the UI assets.
2. **ReplicatedStorage Bridge:** Client-server communication is handled safely via `RemoteEvents` placed in the replicated environment.
3. **Two-Tier Validation:** 
   * *Client-Side:* `LocalScript` runs initial visibility checks for a clean user experience.
   * *Server-Side:* The server re-validates the sender's `User ID` against the whitelist on *every single request*. If an exploiter fires the `RemoteEvent` directly, the server instantly drops the request.
