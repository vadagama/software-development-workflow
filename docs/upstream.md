---
schema_version: 1
id: STD-UPSTREAM
type: standard
title: "Граница с upstream и процесс"
status: draft
language: ru
owner: "Сопровождающие репозитория"
created: "2026-11-21"
updated: "2026-11-23"
related: [STD-ARTIFACTS]
---

# Граница с upstream и процесс

Базовый процесс — upstream skills [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills/blob/8c99fa36d90068c82d547f7c12a9d67a3e95f4ee/README.md), commit `8c99fa36d90068c82d547f7c12a9d67a3e95f4ee` (редакция upstream 0.6.17); версия закреплена в [lock](../upstream.lock.yaml). Все 26 upstream skills действуют в оригинале: надстройка не создаёт дублирующих skills и не переписывает их инструкции. Карта ниже — не замена их инструкций и не требование запускать все навыки для каждой задачи; она показывает этапы, на которых обновляются артефакты надстройки, и навыки, к которым надстройка что-то добавляет.

Порядок предпочтений: оригинальный skill → соглашения продукта → минимальное расширение только для непокрытой обязанности. Минимальное расширение для непокрытой обязанности — артефакты надстройки — упаковано в собственный скилл стандарта `software-development-workflow-addons`; он не переписывает и не заменяет upstream skills. SPEC, планы, задачи, CONSTRAINTS, ADR и CHANGELOG остаются в конвенциях upstream (`spec/`, `tasks/plan.md`, `tasks/todo.md`, `CONSTRAINTS.md`, каталог решений), а их содержание в продукте пишется на русском — см. [каталог артефактов](artifacts.md). Примеры upstream не являются требованиями к стеку продукта; оригинальные файлы skills и их supporting references устанавливаются штатным способом.

## Этапы процесса

| Этап | Основные skills upstream | Дополнение надстройки | Условие завершения |
| --- | --- | --- | --- |
| Контекст | `using-agent-skills`, `context-engineering` | Прочитать конфигурацию, платформу и HLD | Известны версии, профиль, ограничения и источник задач |
| Уточнение | `interview-me`, `idea-refine`, `spec-driven-development` | Требования и открытые вопросы — на русском | Критерии проверяемы, scope определён; пройдены применимые согласования upstream |
| Проектирование | `api-and-interface-design`, `documentation-and-adrs`, `constraint-driven-development` | Влияние на HLD и его список ADR; контракты в `docs/api`, `docs/events`, `docs/data` | Определены контракты, NFR и затронутые документы |
| Планирование | `planning-and-task-breakdown` | План и задачи ведутся по конвенциям upstream | Зависимости, критерии и проверки есть; чужой незавершённый план сохранён |
| Реализация | `incremental-implementation`, `test-driven-development`, `source-driven-development` | Код и затронутая документация изменяются вместе | Вертикальный срез подтверждён проверками |
| Проверка | `debugging-and-error-recovery`, применимые специализированные skills | Доказательства приёмки, совместимости, актуальности документов | Обязательные проверки пройдены; ограничения явно перечислены |
| Review | `code-review-and-quality`, `documentation-and-adrs` | Сверка SPEC → код → HLD; фиксация подтверждённого долга в `docs/debt` | Нет необъяснённых расхождений; изменения документации проверены |
| Выпуск | `git-workflow-and-versioning`, `ci-cd-and-automation`, `shipping-and-launch`, `observability-and-instrumentation` | Эксплуатационные разделы HLD; rollback; целевое состояние сверяется с фактическим | Выполнены правила платформы и продукта, приложены доказательства |
| Развитие | `deprecation-and-migration`, `performance-optimization`, `code-simplification` | Долг в `docs/debt` → задача upstream-плана → SPEC upstream | Эффект проверен; закрытие долга имеет доказательство |

Skills по UI, браузеру, доступности и безопасности выбираются по области изменения; их дополнения к документам надстройки перечислены ниже. Оценка влияния и результаты проверки записываются в разделах существующего плана или задачи — отдельные документы для них не создаются. Эксплуатация и безопасность описываются в HLD, а проверяемые требования — в SPEC и задачах.

До изменения: определить затронутые документы и обозначить целевое состояние. После: обновить фактическое состояние и приложить проверку. Если влияние отсутствует, кратко объяснить почему. Не менять несвязанные SPEC при локальной правке.

Полный цикл SPEC и согласований применяется в случаях, заданных upstream. Сам профиль не отменяет gates навыка. Для исправления опечатки или иной самодостаточной малой правки применяются исключения upstream, а не создаётся формальная SPEC задним числом.

Планы, задачи и их архивирование ведутся по конвенциям upstream `planning-and-task-breakdown`: не перезаписывать незавершённый план другой работы и согласовать разрешение конфликта до правки.

Задачи ведутся в выбранном источнике по upstream-конвенции — `tasks/todo.md` или внешний трекер. Документы надстройки ссылаются на задачи через `external_refs` вместо дублирования статусов.

## Дополнения к навыкам

| Навык upstream | Дополнение надстройки |
| --- | --- |
| `using-agent-skills` | Выбор навыков с учётом профиля и `capabilities` |
| `context-engineering` | Загружать выбранные версии стандарта, платформы и HLD |
| `api-and-interface-design` | Контракты в `docs/api/<boundary>.md` и `docs/events/<event>.md` со ссылкой на машинный первоисточник |
| `documentation-and-adrs` | HLD целиком в `docs/hld/README.md` со связанным списком принятых ADR и ссылками на каталог решений; русский язык; ADR — upstream-конвенция |
| `incremental-implementation`, `code-simplification` | Затронутые документы обновляются вместе с кодом |
| `code-review-and-quality` | Проверка влияния на документацию и сверка SPEC → код → HLD |
| `security-and-hardening` | Анализ угроз и принятых рисков в HLD |
| `performance-optimization`, `observability-and-instrumentation` | Измеримые NFR, эксплуатация и доказательства результата |
| `shipping-and-launch` | Эксплуатационные разделы HLD; CHANGELOG — upstream-конвенция |
| `git-workflow-and-versioning` | Документы надстройки хранятся в Git продукта |

Остальные 14 skills — `interview-me`, `idea-refine`, `spec-driven-development`, `constraint-driven-development`, `planning-and-task-breakdown`, `test-driven-development`, `source-driven-development`, `doubt-driven-development`, `frontend-ui-engineering`, `browser-testing-with-devtools`, `debugging-and-error-recovery`, `accessibility-engineering`, `ci-cd-and-automation`, `deprecation-and-migration` — действуют в оригинале: их артефакты остаются в upstream-конвенциях, а системные последствия (UI-требования, доступность, диагностика) отражаются в HLD и критериях задач.
