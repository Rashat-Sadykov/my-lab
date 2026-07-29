# 🛡️ Лабораторные кейсы по ИБ и администрированию

Здесь собраны графические подтверждения выполнения практических и лабораторных работ.

---

## 🔒 1. СЗИ Secret Net Studio

### Кейс А: Администрирование СЗИ в автономном режиме
*Описание:* Настройка локальных политик безопасности, механизмов разграничения доступа и контроля устройств рабочей станции.

<details>
  <summary><b>🖼️ Нажмите, чтобы посмотреть скриншоты настройки (12 фото)</b></summary>
  <div style="margin-top: 10px;">
    <img src="sns-autonomous-admin2.png" alt="Шаг 1" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin2-1.png" alt="Шаг 2" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3.png" alt="Шаг 3" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3-1.png" alt="Шаг 4" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3-2.png" alt="Шаг 5" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3-3.png" alt="Шаг 6" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3-4.png" alt="Шаг 7" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin3-5.png" alt="Шаг 8" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin4.png" alt="Шаг 9" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin4-1.png" alt="Шаг 10" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin4-2.png" alt="Шаг 11" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-autonomous-admin4-3.png" alt="Шаг 12" style="max-width:100%; display:block; margin-bottom:10px;">
  </div>
</details>

### Кейс Б: Настройка и администрирование IDS/IPS системы

*Описание:* Развертывание системы обнаружения и предотвращения вторжений, настройка правил фильтрации трафика и сигнатур для защиты сетевого периметра от несанкционированной активности. В качестве СЗИ используется **Secret Net Studio (SNS)** (модуль «Сетевая защита»). Тестирование производится из Linux-системы с помощью утилиты генерации пакетов `hping3`.

📥 **Нажмите, чтобы посмотреть скриншоты настройки IDS/IPS**

---

#### 1. Практическое тестирование детектора сканирования портов
* **Конфигурация в SNS:** Перейдите в параметры модуля **«Сетевая защита» → «Обнаружение вторжений»**, откройте вкладку **«Детекторы»** и выберите пункт **«Сканирование портов»**.
* **Исследование чувствительности:**
  * При значении порога интенсивности **950** детектор успешно фиксирует аномальную сетевую активность и генерирует событие атаки.
  * При увеличении порога до **1000** (шаг настройки — 50) чувствительность снижается, интенсивности генерации пакетов становится недостаточно, и детектор перестает срабатывать.
* **Используемые команды тестирования (`hping3`):**
  * `sudo hping3 -S -F` — сканирование пакетами с некорректным сочетанием флагов **SYN** и **FIN** (в легитимном TCP-трафике они исключают друг друга).
  * `sudo hping3 -F -P -U` — сканирование типа **Xmas Tree** с одновременным выставлением флагов **FIN**, **PUSH** и **URG**.

#### 2. Практическое тестирование детектора DoS/DDoS-атак
* **Конфигурация в SNS:** В той же вкладке **«Детекторы»** перейдите в пункт **«DDoS»** для настройки лимитов пакетов/сессий с одного IP-адреса.
* **Исследование чувствительности:**
  * При значении порога **10** Secret Net Studio своевременно идентифицирует флуд, переходит в режим IPS и **предотвращает вторжение**, временно блокируя адрес отправителя.
  * При изменении порога до **12** (шаг настройки — 2) лимит оказывается завышенным, трафик от утилиты укладывается в рамки правил, и средство защиты не срабатывает.

---

#### 📊 Ожидаемые события в Журнале безопасности Secret Net Studio
При успешном срабатывании детекторов (параметры 950 и 10) в центральном или локальном журнале SNS регистрируются следующие критические события:
1. **«Обнаружена сетевая атака: Сканирование портов»** — с указанием IP-адреса атакующего хоста Linux и типа зафиксированных TCP-флагов.
2. **«Обнаружена сетевая атака: DoS/DDoS»** — с последующей записью **«Блокировка сетевого адреса»**, подтверждающей автоматическую изоляцию нарушителя модулем IPS.

#### 📝 Заключение по кейсу
В ходе выполнения кейса была экспериментально доказана критическая важность точной настройки порогов (чувствительности) в Secret Net Studio. Слишком высокие значения параметров (1000 для сканирования и 12 для DDoS) приводят к пропуску атак (ложноотрицательный результат), в то время как оптимально подобранные параметры (950 и 10) обеспечивают гарантированную защиту сетевого периметра хоста.

<details>
  <summary><b>🖼️ Нажмите, чтобы посмотреть скриншоты настройки IDS/IPS</b></summary>
  <div style="margin-top: 10px;">
    <img src="IDS_IPS.png" alt="Настройка правил и сигнатур" style="max-width:100%; display:block; margin-bottom:10px;">
  </div>
</details>

### Кейс В: Расследование инцидента внутренней информационной безопасности
*Описание:* Фиксация и анализ инцидента (факт несанкционированного копирования/кражи конфиденциального файла сотрудника другим пользователем системы) через журнал аудит-безопасности СЗИ.

<details>
  <summary><b>🖼️ Нажмите, чтобы посмотреть скриншоты расследования инцидента</b></summary>
  <div style="margin-top: 10px;">
    <img src="sns-incident-theft1.png" alt="Шаг 1" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-incident-theft2.png" alt="Шаг 2" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-incident-theft3.png" alt="Шаг 3" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-incident-theft4.png" alt="Шаг 4" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-incident-theft5.png" alt="Шаг 5" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sns-incident-theft.png" alt="Шаг 6" style="max-width:100%; display:block; margin-bottom:10px;">
  </div>
</details>

---

## 🦠 2. Kaspersky Security Center

### Кейс А: Централизованное управление антивирусной защитой
*Описание:* Развертывание и настройка серверов администрирования, создание и применение групповых политик безопасности.

<details>
  <summary><b>🖼️ Нажмите, чтобы посмотреть скриншоты настройки Kaspersky Security Center</b></summary>
  <div style="margin-top: 10px;">
    <img src="ksc-antivirus-policies1-1.png" alt="Шаг 2" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies1-2.png" alt="Шаг 3" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies1-3.png" alt="Шаг 4" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies2.png" alt="Шаг 5" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies2-1.png" alt="Шаг 5-1" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies3.png" alt="Шаг 6" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ksc-antivirus-policies4.png" alt="Шаг 7" style="max-width:100%; display:block; margin-bottom:10px;">
  </div>
</details>

## 3. Metasploitable

*Описание:* Выявление уязвимостей, сканирование, диагностика SYN, FIN, Xmas, UDP.

<details>
  <summary><b>🖼️ Нажмите, чтобы посмотреть скриншоты Metasploitable</b></summary>
  <div style="margin-top: 10px;">
    <img src="Metasploitable.png" style="max-width:100%; display:block; margin-bottom:10px;">
  </div>
</details>

## 4. Настройка Active Directory
*Описание:* Создание пользователей и групп, добавление рабочих станций в домен, а также разграничение прав доступа в сетевой инфраструктуре.

<details>
<summary><b>📂 Нажмите, чтобы посмотреть скриншоты настройки</b></summary>
<div style="margin-top: 10px;">
    <img src="sozdpolz.png" alt="Шаг 1: Создание пользователя" style="max-width:100%; display:block; margin-bottom:10px;">
  <img src="sozdpolz2.png" alt="Шаг 1: Создание пользователя" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="sozdgrup.png" alt="Шаг 2: Создание групп" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="dobavdomen.png" alt="Шаг 3: Добавление в домен" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="prava.png" alt="Шаг 4: Разграничение прав" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="prava2.png" alt="Шаг 4: Разграничение прав" style="max-width:100%; display:block; margin-bottom:10px;">
</div>
</details>


## 5. Аудит и сканирование инфраструктуры Active Directory

*Описание:* Проверка работоспособности контроллеров домена, репликации и конфигурации служб с помощью утилиты DCDiag, а также формирование отчетов.
<details>
<summary><b>📂 Нажмите, чтобы посмотреть скриншоты DCDiag</b></summary>
<div style="margin-top: 10px;">
    <img src="DCdiag.png" alt="Сканирование DCDiag" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="Dcdiag1.png" alt="Сканирование Dcdiag1" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="DCdiagotchet.png" alt="Отчет DCDiag" style="max-width:100%; display:block; margin-bottom:10px;">
</div>
</details>

## 2. Анализ конфигурации безопасности (AD DCL Scanner)
*Описание:* Сканирование настроек безопасности домена, выявление уязвимостей в конфигурации и генерация отчетов через AD DCL Scanner.
<details>
<summary><b>📂 Нажмите, чтобы посмотреть скриншоты AD DCL Scanner</b></summary>
<div style="margin-top: 10px;">
    <img src="ADACL.png" alt="Сканирование AD DCL Scanner" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ACLREPORT.png" alt="Отчет AD DCL Scanner" style="max-width:100%; display:block; margin-bottom:10px;">
    <img src="ACLOtchet.png" alt="Отчет AD DCL Scanner" style="max-width:100%; display:block; margin-bottom:10px;">
</div>
</details>

## 3. Построение векторов атак (BloodHound)
*Описание:* Сбор данных из Active Directory, построение графов и выгрузка отчетов по опасным векторам атак в BloodHound.
<details>
<summary><b>📂 Нажмите, чтобы посмотреть скриншоты BloodHound</b></summary>
<div style="margin-top: 10px;">
       <img src="Bloodotchet.png" alt="Отчет BloodHound" style="max-width:100%; display:block; margin-bottom:10px;">
</div>
</details>


