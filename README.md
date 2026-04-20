# Morphe patches — Yandex Navigator

Репозиторий: **[github.com/vladon/morphe-patches-navi](https://github.com/vladon/morphe-patches-navi)**

Bytecode patches for **Yandex Navigator** (`ru.yandex.yandexnavi`), aligned with reverse‑engineering notes in the parent `navi` repo (`AGENTS.md`).

## Как подключить в Morphe Manager (важно)

Приложение **не качает патчи с «пустой» страницы репозитория** — ему нужен **GitHub Release** с файлом **`patches-<версия>.mpp`** и актуальные **`patches-list.json` / `patches-bundle.json`** (см. [шаблон Morphe Patches — Usage](https://github.com/MorpheApp/morphe-patches-template#-usage) и [документацию Morphe](https://github.com/MorpheApp/morphe-documentation/tree/main/docs/morphe-development)).

**Добавить источник одной ссылкой (рекомендуется):**

[https://morphe.software/add-source?github=vladon/morphe-patches-navi&name=Yandex+Navigator+patches](https://morphe.software/add-source?github=vladon/morphe-patches-navi&name=Yandex+Navigator+patches)

Параметр **`name`** задаёт подпись в списке источников; без него Morphe показывает **«Unnamed»**.

Открой эту ссылку **в браузере на телефоне** (или из README) — Morphe подхватит её как **App Link**. **Не вставляй** `morphe.software/add-source?…` в поле «URL пачки / patch bundle» внутри Morphe: для произвольных хостов приложение требует путь, оканчивающийся на **`.json`**, и покажет тост **«Patch bundle URL must point to a .json file»**.

**Если добавляешь источник вручную в Morphe** (вставкой URL в настройках), укажи **одно** из значений:

- `https://github.com/vladon/morphe-patches-navi` — репозиторий без лишнего пути (`/releases`, `/tree/…` не подходят для этого поля).
- либо прямой raw-файл: `https://raw.githubusercontent.com/vladon/morphe-patches-navi/main/patches-bundle.json`

**Важно:** Morphe Manager читает **`patches-bundle.json` с ветки `main` через `raw.githubusercontent.com`**, а не из GitHub Release. В этом файле на `main` должно быть заполнено поле **`download_url`** (указывает на `.mpp` в Releases). Workflow **Manual publish MPP** после релиза **коммитит** обновлённые `patches-bundle.json` и `patches-list.json` в `main`.

Поле **`created_at`** должно быть в виде **`YYYY-MM-DDTHH:mm:ss` без суффикса `Z`** (как в официальном [morphe-patches](https://raw.githubusercontent.com/MorpheApp/morphe-patches/main/patches-bundle.json)). С `…Z` десериализация в приложении падает, и в тосте видно обрезанный JSON (`Failed to download 'Unnamed': { "created_at": …`).

Пока **нет релиза** с `.mpp` и актуального **`download_url`** на `main`, загрузка пачки не сработает — сначала опубликуй релиз (ниже).

---

## Опубликовать первый релиз (без GPG / без semantic-release)

Штатный `release.yml` из шаблона ждёт **GPG-секреты** для semantic-release. Если их нет, используй ручной workflow:

1. На GitHub: **Actions** → **Manual publish MPP** → **Run workflow** → поле **version** (например `1.0.0`, как в `gradle.properties`).
2. Дождись зелёной галочки.
3. В репозитории появится **Release** `v1.0.0` с артефактами: `patches-1.0.0.mpp`, `patches-list.json`, `patches-bundle.json`.
4. Снова открой ссылку **add-source** выше **в браузере** на устройстве (не вставляй её в поле URL пачки внутри приложения).

Повторный запуск с той же версией: сначала **удали** старый release/tag `v…` вручную на GitHub, либо увеличь версию в workflow.

---

## Локальная сборка (ПК)

Учётные данные для `maven.pkg.github.com/MorpheApp/registry` — в **`%USERPROFILE%\.gradle\gradle.properties`** (или переменные окружения `GITHUB_ACTOR` / `GITHUB_TOKEN`):

```properties
gpr.user=ТВОЙ_ЛОГИН_GITHUB
gpr.key=ghp_...
```

Сборка MPP для Android:

```powershell
cd morphe-patches-navi
.\gradlew.bat :patches:buildAndroid generatePatchesList
```

Артефакт: `patches\build\libs\patches-<version>.mpp`.

---

## Included patches

| Patch | Effect |
|-------|--------|
| **Enable debug panel** | Forces `ru.yandex.yandexmaps.debug.v0.c()Z` and `ru.yandex.yandexmaps.debug.m0.i()Z` to always return `true`, unlocking the internal Maps‑shell debug drawer gate (28.6.5 / versionCode `739172520`). |

## Compatibility

Declared in `Constants.kt`: package `ru.yandex.yandexnavi`, `ApkFileType.APKS`, version **28.6.5**, signing cert SHA‑256 from official Play `base.apk`.

## References

- [Morphe development](https://github.com/MorpheApp/morphe-documentation/tree/main/docs/morphe-development)
- [Morphe Patches template README](https://github.com/MorpheApp/morphe-patches-template/blob/main/README.md)
- [Morphe Patcher docs](https://github.com/MorpheApp/morphe-patcher/blob/main/docs/README.md)
