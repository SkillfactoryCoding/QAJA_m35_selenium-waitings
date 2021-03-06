Ожидания
========
В наше время, подавляющая часть веб приложений использует асинхронный подход загрузки контента, например AJAX, сокеты и
т.д. В тот момент, когда браузер загрузил страницу, различный контент на этой странице может загружаться асинхронно,
то есть независимо друг от друга, за разные промежутки времени. Такая ситуация может значительно усложнить 
процесс тестирования: тесты могу превратиться в нестабильные и работать через раз или некоторые элементы могут быть не 
найдены вовсе, что ставить под угрозу миссию QA инженера.

Для решения этой проблемы Selenium Webdriver предоставляет такой замечательный механизм как ожидания.
Ожидания бывают двух видов: **явные (explicit)** - это когда драйвер ждёт до тех пор, пока не выполнится определенное условие
и **неявные (implicit)** - драйвер будет обходить DOM в поиске ~~второго носка~~ элемента в течение указанного времени.

Явные ожидания:
--------------
Как мы уже упомянули выше, веб драйвер можно попросить подождать явно при помощи класса WebDriverWait:

    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10)); 

С первым параметром всё ясен пень, а второй указывает драйверу какую паузу необходимо сделать перед следующей попыткой найти элемент.
Чтобы попросить драйвер ждать до наступления ~~темноты~~ каких-либо условий, существует утильный класс ExpectedConditions, который предоставляет готовый набор функций ожидания
([дока с полным списком методов](https://www.selenium.dev/selenium/docs/api/java/org/openqa/selenium/support/ui/ExpectedConditions.html)).
При необходимости можно самостоятельно реализовать интерфейс ExpectedCondition и передать реализацию в метод **until()** или
просто передать лямбду. Как пример, ждём пока не появится возможность ткнуть на элемент:

    wait.until(ExpectedConditions.elementToBeClickable(By.id("some id")));

Неявные ожидания:
----------------
С неявными ожиданиями всё намного проще — мы устанавливаем время, которое драйвер будет опрашивать DOM пытаясь найти элемент.
Установка неявного ожидания выглядит следующим образом:

    driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

Опция будет активна до тех пор, пока существует объект драйвера.
