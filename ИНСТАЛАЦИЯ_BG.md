# Инсталация на Midea AC LAN интеграция за Home Assistant 2026.1.1

## Проблем

След актуализация на Home Assistant OS 2026.1.1, интеграцията дава грешка:

```
Setup failed for custom integration 'midea_ac_lan': Requirements for midea_ac_lan not found: ['midea-local>=6.5.0']
```

## Решение

### Метод 1: Ръчна инсталация на midea-local пакета

1. **Влезте в SSH терминала на Home Assistant**
   - Инсталирайте SSH add-on ако нямате
   - Свържете се към Home Assistant

2. **Инсталирайте midea-local пакета ръчно:**

   ```bash
   docker exec -it homeassistant /bin/bash
   pip install midea-local==6.5.0
   exit
   ```

3. **Рестартирайте Home Assistant**
   - Настройки -> Система -> Рестартиране

### Метод 2: Използване на оригинална версия (временно)

Ако продължава да не работи, може временно да се върнете към версия без промени:

```bash
# В папката custom_components/midea_ac_lan/
git checkout b189730
```

След това рестартирайте Home Assistant.

### Метод 3: Изчистване на кеша

1. **Изтрийте cache файловете:**

   ```bash
   rm -rf /config/custom_components/midea_ac_lan/__pycache__
   rm -rf /config/custom_components/midea_ac_lan/*/__pycache__
   ```

2. **Рестартирайте Home Assistant**

## Промени във версия 0.6.11

Тази версия включва следните промени за съвместимост с Home Assistant 2026.1.1:

✅ Премахнати deprecated `MAJOR_VERSION` и `MINOR_VERSION` константи
✅ Премахнати версионни проверки за backwards compatibility
✅ `ClimateEntityFeature.TURN_ON` и `TURN_OFF` са винаги активни
✅ Опростени импорти и модернизиран код
✅ Добавен български превод (bg.json)

## Ако нищо не работи

### Опция A: Инсталация през HACS

1. Отворете HACS
2. Отидете на Integrations
3. Потърсете "Midea AC LAN"
4. Кликнете Reinstall
5. Рестартирайте Home Assistant

### Опция B: Пълна преинсталация

1. **Премахнете интеграцията:**

   ```bash
   rm -rf /config/custom_components/midea_ac_lan
   ```

2. **Копирайте отново файловете от този проект**

3. **Уверете се че manifest.json съдържа:**

   ```json
   "requirements": ["midea-local>=6.5.0"],
   "version": "v0.6.11"
   ```

4. **Рестартирайте Home Assistant**

## Проверка на лога

За да видите точната грешка:

1. Настройки -> Система -> Дневници
2. Търсете за "midea_ac_lan"
3. Проверете дали има грешки свързани с:
   - Python import грешки
   - Requirements грешки
   - Connection грешки

## Необходими изисквания

- Home Assistant OS 2026.1.1 или по-нова
- midea-local пакет версия 6.5.0 или по-нова
- Python 3.11 или по-нова

## Допълнителна помощ

Ако проблемът продължава:

1. Проверете дали имате интернет връзка в Home Assistant
2. Проверете дали pip може да инсталира пакети
3. Опитайте да инсталирате midea-local ръчно (вижте Метод 1)

За техническа поддръжка, отворете issue на: https://github.com/wuwentao/midea_ac_lan/issues
