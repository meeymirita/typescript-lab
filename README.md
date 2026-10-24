# TypeScript Lab — Warehouse

![TypeScript](https://raw.githubusercontent.com/meeymirita/works-lab/main/images/typescript.png)

> **24.09.2026 — методичка вычитана и исправлена.** Что найдено и что поправлено — в [fixes/frontend/typescript.md](https://github.com/meeymirita/lab-fixes/blob/main/frontend/typescript.md) репозитория `lab-fixes`.

**Статус: ✅ 10 сессий (расширена 21.10.2026, формат Caddy). Сессии 1–5 вычитаны и проверены запуском (04.10.2026, переведены в формат Caddy 17.10.2026). Сессии 6–10 (conditional types/infer, декораторы TS 5.x и DI, тесты на типы, project references, итоговое ТЗ без решения) 22.10.2026 прогнаны в Docker (Node 24, TS 6.0, Vitest 5.0.5): `tsc`, `tsc -b`, 30 тестов и CLI — зелёные после 8 исправлений (в том числе: Vitest 5 не транспилирует стандартные декораторы — нужен `unplugin-swc`); «Ожидаемые результаты» шагов 6.2–9.5 — настоящий вывод. 24.10.2026: вычитка сессий 6–10 и справочных разделов; шаг 1.1 запущен буквально через `docker compose`; пакет `web` (Vue, `vue-tsc`) проверен вместе с project references — найдена и исправлена потеря `./browser` в `exports` (шаг 9.2); исправлен эксперимент `expectTypeOf` в 8.1, оглавление («пять» → «десять сессий»); справка дополнена сессиями 6–9.**
**Сложность: высокая** — абстрактное мышление на уровне типов (generics, conditional/mapped types) непривычно после динамического PHP. Логично проходить после или параллельно с Vue-лабой (сессия 5 её использует).

## О чём

Типизация домена складского учёта (Warehouse) с нуля — без фреймворков до последней сессии, чтобы увидеть TypeScript в чистом виде и потом узнавать его в Nest/Vue. Что типы реально ловят (перепутанные аргументы, `NaN` от строки вместо числа, `undefined` в рантайме), а что — нет.

## Стек

TypeScript 6.0 (код проверен и на 5.9) + Node 24+ (LTS) + `tsx` + Vitest + Zod, в финале — Express и Vue 3 + TS. Отдельный репозиторий на npm workspaces: `packages/core`, `cli`, `api`, `web`. Всё в Docker.

## Формат

Методичка [`typescript.html`](typescript.html) ([открыть на сайте](https://anitech.meeymirita.ru/works/typescript.html)) — открывается в браузере. Каждый шаг заканчивается зелёным `npm run typecheck` — это главный критерий готовности.

## Что внутри (10 сессий, порядок строгий)

- **Сессия 1** — стенд (Docker, workspaces, `tsconfig.base`, `tsx`, Vitest); песочница: аннотации, вывод типов, примитивы/объекты, union и литералы, `type` vs `interface`, функции, `any`/`unknown`/`never`, `strict`, `as const`
- **Сессия 2** — домен склада: branded IDs, размеченное объединение `Movement`, exhaustive `switch`, `Result` вместо исключений, type predicates, `readonly` — `applyMovement` с тестами, невозможные состояния невыразимы на уровне типов
- **Сессия 3** — generics и абстракции: `Repository<T>`, `TypedEmitter<Events>`, mapped/conditional/template literal types, `satisfies` — сервис `Warehouse`, собранный из типизированных кубиков
- **Сессия 4** — CLI: `parseArgs`, команды как union из template literal types, валидация через Zod и `z.infer`, `unknown` в `catch`, `.d.ts` для JS, сборка esbuild — рабочий `wh`: `item:add`, `stock:in/out/transfer/list/low`, `import:csv`
- **Сессия 5** — сквозная типизация: `ApiContract`, generic-клиент с conditional types, Express + Zod на бэкенде, Vue 3 + TS (`defineProps`/`defineEmits` с generics, типизированный store), `vue-tsc` — один источник типов и в API, и в браузере
- **Сессия 6** — conditional types и `infer`: голый vs обёрнутый параметр, constrained infer, template literal types — типобезопасные фильтры `Filter<T>`/`FilterKey` и команда `stock:filter --query` в CLI
- **Сессия 7** — декораторы TS 5.x (method/class/accessor, `context.addInitializer`) без `experimentalDecorators`/`reflect-metadata` — `Token<T>`, маленький DI `Container`, `bootstrap()` вместо ручной сборки `Warehouse`
- **Сессия 8** — тесты на типы как отдельная дисциплина: когда `expectTypeOf` на самом деле проверяется, выделенные `*.types.test.ts`, разбор регрессии на `MovementOf`, скрипт-аудит `@ts-expect-error`
- **Сессия 9** — инструменты и масштаб: `composite`/`references`/`tsc -b`, declaration maps и двойные `exports`, когда нужен (и не нужен здесь) `paths`, `.tsbuildinfo` и инкрементальные сборки
- **Сессия 10** — итоговое ТЗ без решения: журнал аудита складских операций, собирающий домен, DI, фильтры, декораторы и типовые тесты из сессий 1–9

## Лицензия и авторство

Код — MIT, тексты — CC BY 4.0, обложки и иллюстрации не покрыты (см. [LICENSE](LICENSE)). Кто что сделал: [NOTICE](NOTICE).

---

Часть сборного репозитория лабораторных работ — [anitech-performance](https://github.com/meeymirita/anitech-performance).
