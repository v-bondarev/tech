# kino-pub-rules

Готовый rule-set для sing-box с доменами `kino.pub` и связанными поддоменами.

## Состав

- `kino-pub.json` — исходный source rule-set.
- `kino-pub.srs` — бинарный rule-set после компиляции.
- `.github/workflows/release.yml` — GitHub Actions для автоматической сборки и публикации `.srs` в Releases.

## Структура rule-set

Используется source format sing-box с `version` и `rules`, который затем компилируется в бинарный `.srs` через `sing-box rule-set compile`.

## Локальная сборка

```bash
sing-box rule-set compile --output kino-pub.srs kino-pub.json
```

## Публикация на GitHub вручную

1. Создай новый репозиторий, например `kino-pub-rules`.
2. Загрузи в него `README.md`, `kino-pub.json` и `.github/workflows/release.yml`.
3. Создай тег и отправь его:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USER/kino-pub-rules.git
git push -u origin main
git tag v1.0.0
git push origin v1.0.0
```

После пуша тега workflow сам скачает sing-box, соберёт `kino-pub.srs` и приложит его к GitHub Release.

## Пример подключения в sing-box

```json
{
  "route": {
    "rule_set": [
      {
        "tag": "kino-pub",
        "type": "remote",
        "format": "binary",
        "url": "https://github.com/USER/kino-pub-rules/releases/download/v1.0.0/kino-pub.srs",
        "download_detour": "direct"
      }
    ],
    "rules": [
      {
        "rule_set": "kino-pub",
        "outbound": "proxy"
      }
    ]
  }
}
```

## Обновление

1. Измени `kino-pub.json`.
2. Закоммить изменения.
3. Создай новый тег, например `v1.0.1`.
4. GitHub Actions автоматически пересоберёт и выложит новый `.srs` в новый Release.
