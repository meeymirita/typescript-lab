# TypeScript Lab — Warehouse

![TypeScript](typescript.png)

**Статус: ⚪ методичка готова, прохождение впереди.**
**Сложность: высокая** — абстрактное мышление на уровне типов (generics, conditional/mapped types) непривычно после динамического PHP. Логично проходить после или параллельно с Vue-лабой (сессия 5 её использует).

## О чём

Типизация домена складского учёта (Warehouse) с нуля — без фреймворков до последней сессии, чтобы увидеть TypeScript в чистом виде и потом узнавать его в Nest/Vue. Что типы реально ловят (перепутанные аргументы, `NaN` от строки вместо числа, `undefined` в рантайме), а что — нет.

## Стек

TypeScript 5.6 + Node 22 + `tsx` + Vitest + Zod, в финале — Express и Vue 3 + TS. Отдельный репозиторий на npm workspaces: `packages/core`, `cli`, `api`, `web`. Всё в Docker.

## Формат

Методичка [`TypeScript_Lab_Warehouse.html`](TypeScript_Lab_Warehouse.html) — открывается в браузере. Каждый шаг заканчивается зелёным `npm run typecheck` — это главный критерий готовности.

## Что внутри (5 сессий, порядок строгий)

- **Сессия 1** — стенд (Docker, workspaces, `tsconfig.base`, `tsx`, Vitest); песочница: аннотации, вывод типов, примитивы/объекты, union и литералы, `type` vs `interface`, функции, `any`/`unknown`/`never`, `strict`, `as const`
- **Сессия 2** — домен склада: branded IDs, размеченное объединение `Movement`, exhaustive `switch`, `Result` вместо исключений, type predicates, `readonly` — `applyMovement` с тестами, невозможные состояния невыразимы на уровне типов
- **Сессия 3** — generics и абстракции: `Repository<T>`, `TypedEmitter<Events>`, mapped/conditional/template literal types, `satisfies` — сервис `Warehouse`, собранный из типизированных кубиков
- **Сессия 4** — CLI: `parseArgs`, команды как union из template literal types, валидация через Zod и `z.infer`, `unknown` в `catch`, `.d.ts` для JS, сборка esbuild — рабочий `wh`: `item:add`, `stock:in/out/transfer/list/low`, `import:csv`
- **Сессия 5** — сквозная типизация: `ApiContract`, generic-клиент с conditional types, Express + Zod на бэкенде, Vue 3 + TS (`defineProps`/`defineEmits` с generics, типизированный store), `vue-tsc` — один источник типов и в API, и в браузере

---

Часть сборного репозитория лабораторных работ — [submodule-group-lab](https://github.com/meeymirita/submodule-group-lab).
