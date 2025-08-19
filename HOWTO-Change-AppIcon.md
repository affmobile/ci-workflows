# 🔄 Как обновить иконку iOS-приложения в репозитории

Эта инструкция объясняет, как поменять иконку приложения.  
Весь процесс автоматизирован с помощью GitHub Actions: нужно только подготовить PNG-иконку и загрузить архив **AppIcons.zip** в репозиторий.

---

## 0. Проверка (и добавление) workflow через вкладку Actions

Перед тем как загружать иконку, убедитесь, что в проекте подключён общий workflow.

1. Перейдите в репозиторий на GitHub.
2. Откройте вкладку **Actions**.  
   ![Вкладка Actions](screenshots/actions-tab.png)
3. Проверьте список workflows:
   - Если есть **“Sync iOS AppIcon”** — всё настроено, переходите к шагу 1.
   - Если нет — добавьте его:

**Как добавить workflow через Actions:**

1. Во вкладке **Actions** нажмите **New workflow**.  
   ![Новый workflow](screenshots/actions-new.png)
2. Внизу страницы выберите **set up a workflow yourself**.  
   ![Set up yourself](screenshots/actions-setup.png)
3. Введите имя файла:
   ```
   .github/workflows/appicon-sync.yml
   ```
4. Вставьте содержимое:

   ```yaml
   name: Sync iOS AppIcon

   on:
     push:
       branches: [main]
     workflow_dispatch:

   permissions:
     contents: write

   jobs:
     sync-icons:
       uses: affmobile/ci-workflows/.github/workflows/appicon-sync.yml@main
       with:
         zip: AppIcons.zip
         target: ios/Runner/Assets.xcassets/AppIcon.appiconset
   ```

5. Нажмите **Start commit → Commit new file**.

Теперь проект подключён к центральному workflow, можно обновлять иконку.

---

## 1. Подготовка PNG-файла

1. Возьмите изображение будущей иконки.  
   - Размер: **1024×1024 px**  
   - Формат: **PNG**  
   - Без альфа-канала (прозрачности).

   ![PNG 1024x1024](screenshots/png-1024.png)

---

## 2. Генерация AppIcons.zip через [appicon.co](https://www.appicon.co/)

1. Перейдите на сайт [https://www.appicon.co/](https://www.appicon.co/).  
   ![Сайт appicon.co](screenshots/appicon-home.png)
2. Нажмите **Choose File** и выберите подготовленный PNG-файл.  
   ![Выбор PNG](screenshots/choose-png.png)
3. Отметьте платформы:  
   - ✅ iPhone  
   - ✅ iPad  
   ![Выбор iPhone/iPad](screenshots/choose-devices.png)
4. Нажмите **Generate** и скачайте архив.  

   ⚠️ **Важно:** сайт по умолчанию сохраняет файл как **`AppIcons.zip`**.  
   - Проверьте, что название именно **`AppIcons.zip`**.  
   - Если файл скачался как `AppIcons (1).zip` или с другим именем, переименуйте его вручную в:
     ```
     AppIcons.zip
     ```

   ![Проверка имени файла](screenshots/check-filename.png)

---

## 3. Загрузка AppIcons.zip в репозиторий

1. Откройте проект на GitHub.  
   ![Главная страница репозитория](screenshots/repo-main.png)
2. Нажмите **Add file → Upload files**.  
   ![Кнопка загрузки](screenshots/upload-files.png)
3. Перетащите архив `AppIcons.zip` в окно загрузки.  
   ![Перетаскивание архива](screenshots/drag-drop.png)
4. Зафиксируйте изменения (commit) напрямую в ветку **main**.  
   В сообщении к коммиту можно написать, например:
   ```
   chore(ios): update AppIcons.zip
   ```
   ![Форма коммита](screenshots/commit.png)

---

## 4. Автоматическая замена иконки

После попадания коммита в `main` автоматически запустится GitHub Action. Он:
- распакует `AppIcons.zip`;
- найдёт `AppIcon.appiconset`;
- заменит иконки в `ios/Runner/Assets.xcassets/AppIcon.appiconset`;
- создаст новый коммит с обновлённой иконкой.

![Запуск GitHub Action](screenshots/gh-action-running.png)

В истории коммитов появится запись вида:
```
chore(ios): sync AppIcon from AppIcons.zip
```
![Коммит синхронизации](screenshots/synced-commit.png)

---

## ✅ Всё готово!

- Проверили/добавили workflow → PNG 1024×1024 → AppIcons.zip через appicon.co → загрузка в репозиторий → автоматическая замена иконки.  
- Ручное копирование иконок в Xcode не требуется.
