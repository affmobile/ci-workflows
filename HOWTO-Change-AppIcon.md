# 🔄 Как обновить иконку iOS-приложения в репозитории

Эта инструкция объясняет, как поменять иконку приложения.  
Весь процесс автоматизирован с помощью GitHub Actions: нужно только подготовить PNG-иконку и загрузить архив **AppIcons.zip** в репозиторий.

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
   - Если файл скачался как `AppIcons (1).zip` или другое имя, переименуйте его вручную:
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
   - В сообщении к коммиту можно написать, например:  
     ```
     chore(ios): update AppIcons.zip
     ```

   ![Форма коммита](screenshots/commit.png)

---

## 4. Автоматическая замена иконки

1. После того как коммит попадёт в ветку `main`, автоматически запустится GitHub Action.  
   Он:
   - распакует `AppIcons.zip`;  
   - найдёт `AppIcon.appiconset`;  
   - заменит иконки в `ios/Runner/Assets.xcassets/AppIcon.appiconset`;  
   - создаст новый коммит с обновлённой иконкой.

   ![Запуск GitHub Action](screenshots/gh-action-running.png)

2. В истории коммитов появится запись вида:  
