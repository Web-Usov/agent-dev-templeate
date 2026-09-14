# Agent Dev Template

Шаблон репозитория для облачной разработки через ChatGPT.com + GitHub без обязательного локального coding agent.

Основная идея: **GitHub хранит состояние проекта, спецификации и историю решений, GitHub Actions исполняет проверки, а AI-агент работает через Issue → ветку → PR → CI → исправления → merge.**

## Для чего этот шаблон

Этот репозиторий не привязан к Node.js, Python, Go или другому стеку. Он задаёт процесс и контракт, который конкретный проект дополняет своими командами сборки и тестирования.

Цикл разработки:

```text
Идея / Issue
    ↓
Спецификация
    ↓
Технический план
    ↓
Задачи
    ↓
Ветка + реализация
    ↓
Pull Request
    ↓
GitHub Actions
    ↓
Логи / диагностические артефакты
    ↓
Исправления
    ↓
Merge
```

## Быстрый старт нового проекта

1. Создайте новый репозиторий через **Use this template**.
2. Заполните `docs/constitution.md` правилами конкретного проекта.
3. Адаптируйте `scripts/ci` и `scripts/ci-full` под стек приложения либо добавьте хуки `scripts/project-ci` и `scripts/project-ci-full`.
4. Для первой функции скопируйте `specs/_template/` в отдельный каталог, например `specs/001-auth/`.
5. Заполните `spec.md` → `plan.md` → `tasks.md`.
6. Создайте Issue и рабочую ветку.
7. Реализуйте задачу через PR. Merge допускается только после успешного CI.

Подробный процесс: [docs/development-process.md](docs/development-process.md).

## Структура

```text
.github/
  ISSUE_TEMPLATE/       формы Issue на русском языке
  workflows/            CI и ручные проверки
  pull_request_template.md
specs/
  _template/            шаблоны spec / plan / tasks
docs/
  constitution.md       постоянные правила проекта
  development-process.md
  commands.md           контракт исполняемых команд
  adr/                   архитектурные решения
scripts/
  ci                     быстрые проверки
  ci-full                полные проверки
  diagnostics            диагностический пакет для CI
AGENTS.md                правила для AI-агентов
```

## Совместимость со Spec Kit

Структура намеренно повторяет смысловую цепочку GitHub Spec Kit: **constitution → specify → plan → tasks → implement → converge**, но не копирует и не форкает сам Spec Kit. Его можно подключить поверх этого шаблона, если проекту нужен официальный CLI и готовые agent skills.

Это позволяет использовать шаблон и без Spec Kit, и вместе с ним.

## GitHub Actions как execution layer

Шаблон предоставляет два основных workflow:

- `CI` — быстрые проверки на `push` и `pull_request`;
- `Полный CI` — более тяжёлые проверки, запускаемые вручную или через reusable workflow.

При ошибке создаётся диагностический artifact, который удобно передать AI-агенту для разбора.

GitHub Actions здесь **не является универсальным удалённым shell**. Произвольные команды из Issue, PR или комментариев не выполняются.

## Что сознательно не входит

- staging/production hosting;
- Railway, Render, Vercel, Fly.io, VPS и другие runtime providers;
- production secrets и production DB migrations;
- выбор конкретного backend/frontend framework.

GitHub Pages можно подключить отдельно для публикации документации; процесс описан в `docs/development-process.md`.

## Язык проекта

Документация, Issue и Pull Request в проектах на основе этого шаблона ведутся **на русском языке**, если конкретный проект явно не изменил это правило в `docs/constitution.md`.
