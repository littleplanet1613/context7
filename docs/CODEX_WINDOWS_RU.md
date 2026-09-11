# Context7 + Codex на Windows

Пошаговая установка Context7 для Codex на Windows без перезаписи наших существующих настроек.

## Что понадобится

- Windows 10/11;
- Node.js 18 или новее;
- установленный Codex;
- PowerShell;
- интернет для первого запуска `npx ctx7`.

Проверка Node.js:

```powershell
node -v
npm -v
```

Если `node` не найден, сначала установить актуальный Node.js LTS.

## Рекомендуемый вариант для нашей схемы

Мы уже используем глобальные настройки Codex и глобальные skills. Поэтому Context7 тоже лучше подключить глобально.

Запустить:

```powershell
npx ctx7 setup --codex
```

Context7 умеет настраивать Codex напрямую. В глобальном режиме он использует:

```text
%USERPROFILE%\.codex\config.toml
%USERPROFILE%\.codex\AGENTS.md
%USERPROFILE%\.agents\skills\context7-mcp\
```

Это важно: не нужно вручную заменять весь `config.toml`. Setup Context7 умеет работать с существующим конфигом Codex.

## Что выбрать при интерактивной установке

Если команда задаёт вопросы:

- клиент: Codex;
- область: global, если Context7 нужен во всех проектах;
- режим: для нашей схемы предпочтителен CLI + Skills;
- авторизация: OAuth или API key по желанию пользователя.

Бесплатный API key рекомендуется самим Context7 для более высоких лимитов, но ключ нельзя сохранять в GitHub.

## Проектная установка

Если Context7 нужен только одному проекту:

```powershell
npx ctx7 setup --codex --project
```

Тогда используются проектные пути:

```text
.codex\config.toml
AGENTS.md
.agents\skills\context7-mcp\
```

Для нашей общей рабочей среды этот вариант не основной, но полезен для изолированных проектов.

## Проверка после установки

Проверить, что CLI отвечает:

```powershell
npx ctx7 --help
```

Найти библиотеку:

```powershell
npx ctx7 library react
```

Получить документацию:

```powershell
npx ctx7 docs /facebook/react "useEffect cleanup"
```

После этого открыть Codex и дать тестовую задачу:

```text
Проверь через Context7 актуальную документацию React по useEffect cleanup и кратко сообщи, что нашёл.
```

## Как проверить файлы вручную

Проверка skills:

```powershell
Get-ChildItem "$HOME\.agents\skills"
```

Там должен появиться каталог Context7.

Проверка правил Codex:

```powershell
Get-Content "$HOME\.codex\AGENTS.md"
```

Проверка конфигурации:

```powershell
Get-Content "$HOME\.codex\config.toml"
```

Не публиковать содержимое конфигурации, если в нём есть ключи, токены или другие секреты.

## Телеметрия

Context7 CLI поддерживает отключение анонимной телеметрии через переменную окружения.

Для текущей PowerShell-сессии:

```powershell
$env:CTX7_TELEMETRY_DISABLED="1"
```

Чтобы сделать настройку постоянной для пользователя Windows:

```powershell
[Environment]::SetEnvironmentVariable("CTX7_TELEMETRY_DISABLED", "1", "User")
```

После этого открыть новую консоль.

## Удаление

Удалить настройку Context7 из Codex:

```powershell
npx ctx7 remove --codex
```

Проектный вариант:

```powershell
npx ctx7 remove --codex --project
```

Если `ctx7` был установлен глобально отдельной командой:

```powershell
npm uninstall -g ctx7
```

Если использовался только `npx ctx7`, отдельного глобального пакета для удаления нет.

## Важное правило для нашей сборки

Не затирать вручную:

```text
~/.codex/config.toml
~/.codex/AGENTS.md
```

В этих файлах у нас одновременно могут жить настройки Open Steps, Astra/Luna Orchestrator, Context7 и других инструментов. Любое изменение должно быть добавочным и проверяемым, а не заменой файла целиком.
