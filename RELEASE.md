# Автоматическое версионирование и релизы

## Как это работает

1. **Разработка**: Создаете feature ветки (`feature/name-branch`) и делаете коммиты
2. **Merge**: Делаете squash merge в main ветку
3. **Автоматический релиз**: При push в main запускается release workflow, который:
   - Анализирует коммиты и определяет тип версии (patch/minor/major)
   - Обновляет версию в package.json
   - Создает CHANGELOG.md
   - Публикует релиз в npm
   - Создает GitHub Release с тегом

## Настройка

### 1. GitHub Secrets

Добавьте в репозитории GitHub Secrets:

- `NPM_TOKEN`: Токен для публикации в npm (получить на [npmjs.com](https://www.npmjs.com/settings/whostolemysleep/tokens))
- `GITHUB_TOKEN`: Уже доступен в GitHub Actions (добавлен автоматически)

### 2. Права доступа

Убедитесь что GitHub Actions имеет права на:
- Создание тегов и релизов
- Push в репозиторий

## Типы версий

Версия определяется на основе сообщений коммитов:

- `feat:` - minor версия (1.1.0)
- `fix:` - patch версия (1.0.1)  
- `BREAKING CHANGE` - major версия (2.0.0)

## Пример использования

```bash
# Создаем feature ветку
git checkout -b feature/add-new-logging-level

# Делаем коммиты
git commit -m "feat: add debug logging level"
git commit -m "fix: resolve timestamp formatting issue"

# Merge в main с squash
git checkout main
git merge --squash feature/add-new-logging-level
git commit -m "feat: add debug logging level and fix timestamp"

# Push в main - запустит автоматический релиз
git push origin main
```

## Результат

После push в main автоматически:
- Обновится версия в package.json
- Создастся тег (например v1.1.0)
- Опубликуется новая версия в npm
- Создастся GitHub Release с CHANGELOG
