# SETUP

Готовое дерево на базе `chirpy-starter` (тема `jekyll-theme-chirpy ~> 7.6`)
с уже применённой косметикой: моноширинный JetBrains Mono + зелёный акцент,
тёмная тема без переключателя.

## 1. Подставить свои значения

Везде стоит плейсхолдер `SxRip` — заменить на свой GitHub-логин
(строчными буквами):

```powershell
# PowerShell, из корня репо
Get-ChildItem -Recurse -Include *.yml,*.md -File |
  ForEach-Object { (Get-Content $_ -Raw) -replace 'SxRip','твойлогин' |
  Set-Content $_ -NoNewline }
```

```bash
# bash / WSL
grep -rl SxRip --include='*.yml' --include='*.md' . | xargs sed -i 's/SxRip/твойлогин/g'
```

Дальше пройтись по `# TODO` в `_config.yml` и `_data/contact.yml`:

| Где | Что |
|---|---|
| `_config.yml` → `title`, `tagline`, `description` | название и подзаголовок сайдбара |
| `_config.yml` → `social.name`, `social.email` | автор постов и копирайт в футере |
| `_config.yml` → `twitter.username` | хэндл без `@`, или пусто |
| `_data/contact.yml` → `discord.url` | инвайт; **пустой url = иконка не отрисуется**, блок можно удалить |
| `_tabs/about.md` | текст страницы About |

## 2. Картинки

- `assets/img/avatar.png` — 512×512, аватар в сайдбаре
- `assets/img/og-default.png` — 1200×630, дефолтная og:image
- `assets/img/favicons/` — распаковать набор с realfavicongenerator.net

Без `avatar.png` в сайдбаре будет битая картинка — либо положи файл,
либо очисти `avatar:` в `_config.yml`.

## 3. Создать репозиторий и запушить

Репозиторий должен называться `<логин>.github.io`, строчными, **public**
(private + Pages только на платном плане). Создать пустым, без README.

```bash
git init -b main
git add .
git commit -m "init: chirpy blog"
git remote add origin git@github.com:SxRip/SxRip.github.io.git
git push -u origin main
```

## 4. Включить Pages

Settings → Pages → **Source: GitHub Actions**.

Не «Deploy from a branch» — Chirpy тянет `jekyll-archives`, которого нет
в whitelist классического Pages-билда, и сайт соберётся мусором.

Первый билд идёт ~2–3 минуты, статус во вкладке Actions.

## 5. Локальный превью

Нативный Ruby на Windows — потеря времени. Два рабочих пути:

**Dev Container** (в репо уже есть `.devcontainer/`): Docker Desktop +
VS Code + расширение Dev Containers → `Reopen in Container` → всё встанет само.

**WSL2**: поставить Ruby 3.4 + bundler, дальше

```bash
bundle install
bundle exec jekyll s --livereload   # http://127.0.0.1:4000
```

Правки в `_config.yml` требуют рестарта сервера, остальное подхватывается на лету.

## 6. Написать пост

`_posts/YYYY-MM-DD-slug.md`. Пример со всем синтаксисом темы лежит в
`_posts/2026-07-21-hello-world.md` — посмотри и удали.

Черновики — в `_drafts/` без даты в имени, показываются только с флагом
`--drafts` и не публикуются.

## 7. Комментарии (опционально)

giscus держит комментарии в GitHub Discussions твоего же репо:

1. Settings → General → Features → включить Discussions
2. Поставить приложение <https://github.com/apps/giscus> на репозиторий
3. На <https://giscus.app> получить `repo_id` и `category_id`
4. В `_config.yml` → `comments.provider: giscus` и заполнить блок `giscus`

## Что стоит знать

- **Панель «Recently Updated»** берёт дату из git-истории файла через
  `_plugins/posts-lastmod-hook.rb`. Работает только при полном клоне —
  в workflow стоит `fetch-depth: 0`, не срезай.
- **`baseurl`** оставлен пустым: это верно для `<логин>.github.io`.
  Для project-site (`<логин>.github.io/blog`) туда нужно `/blog`,
  иначе поедут все пути к ассетам.
- **Апгрейд темы**: поднять версию в `Gemfile` → `bundle update jekyll-theme-chirpy`.
  Мажорные версии меняют `_config.yml` и `assets/css/jekyll-theme-chirpy.scss` —
  сверяйся с Upgrade Guide в вики темы. Верхняя часть моего SCSS скопирована
  из v7.6.0, при мажоре её надо переписать.
- **`.gitmodules`** ссылается на `chirpy-static-assets` — это опция самохостинга
  JS/CSS вендоров, по умолчанию выключена (`assets.self_host.enabled` пусто).
  Нужна — `git submodule add https://github.com/cotes2020/chirpy-static-assets.git assets/lib`
  и раскомментировать `submodules: true` в workflow.
- **Внешние шрифты**: тема грузит Source Sans Pro / Lato с CDN даже при
  переопределённом font-stack. Если хочешь ноль внешних запросов — правь
  ссылку `webfonts` в `_data/origin/cors.yml`.
- **htmlproofer** в workflow валит билд на битых внутренних ссылках.
  Это фича, но если мешает — шаг `Test site` можно выкинуть.
- Шрифт под OFL-1.1, лицензия лежит в `assets/fonts/JetBrainsMono-OFL.txt`,
  удалять её нельзя.
