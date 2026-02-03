# QuickRadiology B2B Pricing Page

Профессиональная страница для продажи QuickRadiology медицинским центрам и клиникам.

## Описание

Одностраничный сайт для B2B продаж, включающий:
- Преимущества для медицинских центров
- Расчет ROI (возврат инвестиций)
- Тарифные планы
- FAQ с ответами на все возражения
- Форма обратной связи

## Развертывание

### Вариант 1: Простое размещение

1. Скопируйте файл `index.html` на ваш веб-сервер
2. Убедитесь, что файл доступен по адресу, например: `https://quickradiology.com/b2b/`

### Вариант 2: Через Nginx

Добавьте в конфигурацию Nginx:

```nginx
server {
    listen 80;
    server_name quickradiology.com;
    
    location /b2b {
        alias /path/to/b2b_site;
        index index.html;
        try_files $uri $uri/ /b2b/index.html;
    }
}
```

### Вариант 3: GitHub Pages с кастомным поддоменом (b2b.quickradiology.com)

**Да, это возможно!** Вы можете оставить основной сайт `quickradiology.com` на Tilda, а поддомен `b2b.quickradiology.com` разместить на GitHub Pages.

#### Шаг 1: Создание репозитория и загрузка файлов

1. Создайте новый репозиторий на GitHub (например, `quickradiology-b2b`)
2. Загрузите файлы `index.html` и другие необходимые файлы в репозиторий
3. Перейдите в **Settings** → **Pages** в вашем репозитории

#### Шаг 2: Настройка GitHub Pages

1. В разделе **Source** выберите ветку (обычно `main` или `master`) и папку `/ (root)`
2. В разделе **Custom domain** введите: `b2b.quickradiology.com`
3. Отметьте галочку **Enforce HTTPS** (станет доступна после настройки DNS)
4. GitHub создаст файл `CNAME` в корне репозитория с содержимым `b2b.quickradiology.com`

#### Шаг 3: Настройка DNS записей

Вам нужно добавить DNS запись для поддомена. Это **не повлияет** на основной домен `quickradiology.com`, который продолжит работать с Tilda.

**Вариант A: Если DNS управляется через регистратора домена или Cloudflare:**

1. Войдите в панель управления DNS вашего домена `quickradiology.com`
2. Добавьте новую **CNAME запись**:
   - **Type**: `CNAME`
   - **Name/Host**: `b2b` (или `b2b.quickradiology.com` в зависимости от интерфейса)
   - **Value/Target**: `username.github.io` (замените `username` на ваш GitHub username)
   - **TTL**: `3600` (или оставьте по умолчанию)

**Вариант B: Если используете Cloudflare:**

1. Перейдите в раздел DNS вашего домена
2. Добавьте CNAME запись:
   - **Type**: CNAME
   - **Name**: `b2b`
   - **Target**: `username.github.io`
   - **Proxy status**: ⚠️ **Отключите прокси** (серый облачко) для GitHub Pages, иначе не будет работать

#### Шаг 4: Проверка настройки

1. Подождите 5-60 минут для распространения DNS изменений
2. Проверьте DNS запись командой:
   ```bash
   dig b2b.quickradiology.com CNAME
   ```
   Должна вернуться запись, указывающая на `username.github.io`
3. Откройте `https://b2b.quickradiology.com` в браузере

#### Важные моменты:

- ✅ Основной домен `quickradiology.com` продолжит работать с Tilda
- ✅ Поддомен `b2b.quickradiology.com` будет обслуживаться GitHub Pages
- ✅ Оба сайта могут использовать HTTPS
- ⚠️ DNS изменения могут занять до 24 часов, но обычно работают в течение часа
- ⚠️ Если используете Cloudflare, обязательно отключите прокси для CNAME записи поддомена

#### Альтернатива: Использование A записи (если CNAME не поддерживается)

Если ваш DNS провайдер не поддерживает CNAME для поддомена, используйте A записи:

1. Добавьте **4 A записи** для `b2b`:
   ```
   b2b → 185.199.108.153
   b2b → 185.199.109.153
   b2b → 185.199.110.153
   b2b → 185.199.111.153
   ```

   ⚠️ Эти IP адреса могут измениться. Проверьте актуальные на [GitHub Pages документации](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)

### Вариант 4: Netlify / Vercel

1. Загрузите папку `b2b_site` на Netlify или Vercel
2. Сайт будет автоматически развернут

## Настройка формы обратной связи

По умолчанию форма открывает почтовый клиент. Для полноценной работы:

1. **Используйте сервис формы** (например, Formspree, Netlify Forms):
   - Замените функцию `handleSubmit` в `index.html`
   - Добавьте отправку на ваш API endpoint

2. **Или настройте на вашем сервере**:
   - Создайте endpoint для приема данных формы
   - Отправляйте email через ваш почтовый сервер

Пример интеграции с Formspree:

```javascript
function handleSubmit(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    
    fetch('https://formspree.io/f/YOUR_FORM_ID', {
        method: 'POST',
        body: formData,
        headers: {
            'Accept': 'application/json'
        }
    })
    .then(response => response.json())
    .then(data => {
        alert('Спасибо за заявку! Мы свяжемся с вами в ближайшее время.');
        event.target.reset();
    })
    .catch(error => {
        alert('Произошла ошибка. Пожалуйста, попробуйте позже.');
    });
}
```

## Кастомизация

### Изменение цветов

Отредактируйте CSS переменные в начале файла:

```css
:root {
    --primary-color: #2563eb;      /* Основной цвет */
    --primary-dark: #1e40af;       /* Темный вариант */
    --secondary-color: #10b981;    /* Акцентный цвет */
}
```

### Изменение цен

Найдите секцию `.pricing` и обновите цены в соответствующих карточках.

### Добавление контента

Все секции четко разделены в HTML:
- `.hero` - главный баннер
- `.stats` - статистика
- `.benefits` - преимущества
- `.roi-section` - расчет ROI
- `.pricing` - тарифы
- `.features` - возможности
- `.faq` - вопросы и ответы
- `.cta-section` - форма обратной связи

## Аналитика

Для отслеживания конверсий добавьте:

1. **Google Analytics**:
```html
<!-- Добавьте перед закрывающим тегом </head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

2. **Яндекс.Метрика**:
```html
<!-- Добавьте перед закрывающим тегом </head> -->
<script type="text/javascript">
   (function(m,e,t,r,i,k,a){m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};
   m[i].l=1*new Date();k=e.createElement(t),a=e.getElementsByTagName(t)[0],k.async=1,k.src=r,a.parentNode.insertBefore(k,a)})
   (window, document, "script", "https://mc.yandex.ru/metrika/tag.js", "ym");

   ym(COUNTER_ID, "init", {
        clickmap:true,
        trackLinks:true,
        accurateTrackBounce:true
   });
</script>
```

## Оптимизация

Страница уже оптимизирована для:
- ✅ Мобильных устройств (responsive design)
- ✅ Быстрой загрузки (встроенные стили)
- ✅ SEO (мета-теги, семантическая разметка)
- ✅ Доступности (правильная структура HTML)

## Поддержка

По вопросам обращайтесь: support@quickradiology.com

