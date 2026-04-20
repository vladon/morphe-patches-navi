# Morphe patches — Yandex Navigator

Репозиторий: **[github.com/vladon/morphe-patches-navi](https://github.com/vladon/morphe-patches-navi)**

Bytecode patches for **Yandex Navigator** (`ru.yandex.yandexnavi`), aligned with reverse‑engineering notes in the parent `navi` repo (`AGENTS.md`).

## Included patches

| Patch | Effect |
|-------|--------|
| **Enable debug panel** | Forces `ru.yandex.yandexmaps.debug.v0.c()Z` and `ru.yandex.yandexmaps.debug.m0.i()Z` to always return `true`, unlocking the internal Maps‑shell debug drawer gate (28.6.5 / versionCode `739172520`). |

---

## Что сделать на GitHub (один раз)

1. Войди на [github.com](https://github.com) под своим аккаунтом (логин — это **`gpr.user`** / `GITHUB_ACTOR`).
2. Открой: **Settings** (аватар → Settings) → внизу слева **Developer settings** → **Personal access tokens** → **Tokens (classic)** → **Generate new token (classic)**.
3. Укажи имя токена (например `Morphe Gradle`), срок действия по желанию.
4. Включи scope **`read:packages`** (достаточно, чтобы Gradle тянул артефакты Morphe из GitHub Packages). Остальные галочки не обязательны.
5. Нажми **Generate token**, **сразу скопируй** строку вида `ghp_xxxxxxxx…` — второй раз GitHub её не покажет. Это значение для **`gpr.key`** / `GITHUB_TOKEN`.

Отдельно приглашение в репозиторий Morphe **не нужно**: пакеты публичного org `MorpheApp` читаются с любого аккаунта при наличии PAT с `read:packages`.

---

## Локально: подставить значения и собрать

**Вариант A (удобно):** скопируй [`gradle.properties.example`](gradle.properties.example) в файл **`gradle.properties`** в том же каталоге (он в `.gitignore` и **не коммитится**). Подставь `gpr.user` и `gpr.key`.

Затем:

```powershell
cd d:\projects\navi\morphe-patches-navi
.\gradlew.bat :patches:build
```

**Вариант B (на сессию PowerShell):**

```powershell
cd d:\projects\navi\morphe-patches-navi
$env:GITHUB_ACTOR = "ТВОЙ_ЛОГИН_GITHUB"
$env:GITHUB_TOKEN = "ghp_вставь_полный_токен"
.\gradlew.bat :patches:build
```

После успешной сборки: **`patches/build/libs/*.mpp`** и при необходимости **`patches-list.json`** (задача `generatePatchesList` при `publish`).

---

## Compatibility

Declared in `Constants.kt`: package `ru.yandex.yandexnavi`, `ApkFileType.APKS`, version **28.6.5**, signing cert SHA‑256 from official Play `base.apk` (see `apksigner verify --print-certs`).

## References

- [Morphe development](https://github.com/MorpheApp/morphe-documentation/tree/main/docs/morphe-development)
- [Morphe Patcher docs](https://github.com/MorpheApp/morphe-patcher/blob/main/docs/README.md)
