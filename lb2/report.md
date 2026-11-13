
# Лабораторна робота №2

## Проєктування тестів. Checklist, Test Cases та Decision Table

### 1. Test Basis

Для тестування форми Login у SauceDemo використано надані вимоги:

- **TB-01. Обов'язковий Username** — порожнє поле Username повинно викликати повідомлення `Epic sadface: Username is required`.
- **TB-02. Обов'язковий Password** — якщо Username заповнений, а Password порожній, система повинна показати повідомлення `Epic sadface: Password is required`.
- **TB-03. Валідні облікові дані** — допустимі usernames: `standard_user`, `locked_out_user`, `problem_user`, `performance_glitch_user`, `error_user`, `visual_user`; пароль — `secret_sauce`.
- **TB-04. Успішний Login** — валідний незаблокований користувач повинен перейти на сторінку Products.
- **TB-05. Заблокований користувач** — авторизація `locked_out_user` повинна бути відхилена з повідомленням `Epic sadface: Sorry, this user has been locked out`.
- **TB-06. Невалідні credentials** — неправильні Username або Password повинні викликати повідомлення `Epic sadface: Username and password do not match any user in this service`.

### 2. Test Conditions

1. **TCND-01** — успішна авторизація валідного користувача.
2. **TCND-02** — авторизація з неправильним Username.
3. **TCND-03** — авторизація з неправильним Password.
4. **TCND-04** — авторизація з порожнім Username.
5. **TCND-05** — авторизація з порожнім Password.
6. **TCND-06** — авторизація заблокованого користувача.

### 3. Checklist

- [] Перевірити успішну авторизацію з валідними обліковими даними.
- [] Перевірити авторизацію з неправильним Username.
- []  Перевірити авторизацію з неправильним Password.
- []  Перевірити авторизацію з порожнім Username.
- [] Перевірити авторизацію з порожнім Password.
- [] Перевірити відхилення авторизації заблокованого користувача.

### 4. Test Cases

#### TC-LOGIN-01 — Успішна авторизація standard_user

- **ID:** TC-LOGIN-01
- **Title:** Успішна авторизація standard_user.
- **Type:** Positive.
- **Preconditions:**
  - Відкрита сторінка Login SauceDemo.
  - Користувач не авторизований.
- **Test Data:**
  - Username: `standard_user`
  - Password: `secret_sauce`
- **Steps:**
  1. У поле Username ввести `standard_user`.
  2. У поле Password ввести `secret_sauce`.
  3. Натиснути кнопку Login.
- **Expected Result:** Після введення правильних облікових даних та натискання Login користувач успішно авторизується і переходить на сторінку Products.
- **Actual Result:** Після введення `standard_user` і правильного пароля та натискання Login відкрилася сторінка Products.
- **Result:** Pass.

### TC-LOGIN-02 — Авторизація з неправильним Password

- **ID:** TC-LOGIN-02
- **Title:** Авторизація з неправильним Password.
- **Type:** Negative.
- **Preconditions:**
  - Відкрита сторінка Login SauceDemo.
  - Користувач не авторизований.
- **Test Data:**
  - Username: `standard_user`
  - Password: `wrong_password`
- **Steps:**
  1. У поле Username ввести `standard_user`.
  2. У поле Password ввести `wrong_password`.
  3. Натиснути кнопку Login.
- **Expected Result:** Авторизація не виконується, відображається повідомлення `Epic sadface: Username and password do not match any user in this service`.
- **Actual Result:** Після введення `standard_user` і неправильного пароля та натискання Login з'явилося повідомлення `Epic sadface: Username and password do not match any user in this service`.
- **Result:** Pass.

### TC-LOGIN-03 — Авторизація заблокованого користувача

- **ID:** TC-LOGIN-03
- **Title:** Авторизація заблокованого користувача.
- **Type:** Negative.
- **Preconditions:**
  - Відкрита сторінка Login SauceDemo.
  - Користувач не авторизований.
- **Test Data:**
  - Username: `locked_out_user`
  - Password: `secret_sauce`
- **Steps:**
  1. У поле Username ввести `locked_out_user`.
  2. У поле Password ввести `secret_sauce`.
  3. Натиснути кнопку Login.
- **Expected Result:** Авторизація не виконується, відображається повідомлення `Epic sadface: Sorry, this user has been locked out`.
- **Actual Result:** Після введення `locked_out_user` і правильного пароля та натискання Login з'явилося повідомлення `Epic sadface: Sorry, this user has been locked out`.
- **Result:** Pass.

### 5. Decision Table

Для перевірки комбінацій Username та Password використано метод Decision Table Testing.

**Умови:**
- Condition 1 — Username входить до списку допустимих?
- Condition 2 — Password правильний?
- Condition 3 — Користувач заблокований?

Позначення:
- **T** — True (так).
- **F** — False (ні).
- **-** — значення не впливає на результат.

**Можливі дії:**
- **A1** — перехід на сторінку Products.
- **A2** — повідомлення про блокування.
- **A3** — повідомлення про неправильні credentials.

**Decision Table:**

| Умови та дії | R1 | R2 | R3 | R4 | R5 |
|---|:---:|:---:|:---:|:---:|:---:|
| Username допустимий? | T | T | F | T | F |
| Password правильний? | T | T | T | F | F |
| Користувач заблокований? | F | T | - | - | - |
| A1: Products | X | | | | |
| A2: Locked message | | X | | | |
| A3: Invalid credentials | | | X | X | X |

**Пояснення правил:**

- **R1:** правильний Username, правильний Password, користувач не заблокований — перехід на Products.
- **R2:** правильний Username, правильний Password, користувач заблокований — відмова в авторизації.
- **R3:** неправильний Username, правильний Password — повідомлення про неправильні credentials.
- **R4:** правильний Username, неправильний Password — повідомлення про неправильні credentials.
- **R5:** неправильний Username і неправильний Password — повідомлення про неправильні credentials.

**Покриття Decision Table тестовими випадками:**

| Test Case | Правило |
|---|---|
| TC-LOGIN-01 | R1 |
| TC-LOGIN-02 | R4 |
| TC-LOGIN-03 | R2 |

Правила **R3 та R5** залишилися без окремих Test Cases. Це показує, що Decision Table допомагає виявити комбінації умов, які могли бути пропущені під час проєктування тестів.


### 6. Результати виконання Test Cases

| Test Case | Type | Result |
|---|---|---|
| TC-LOGIN-01 | Positive | Pass |
| TC-LOGIN-02 | Negative | Pass |
| TC-LOGIN-03 | Negative | Pass |

**Підсумок:** Усі три тест-кейси виконано успішно. Фактичні результати відповідають очікуваним.

**Висновок:** Під час лабораторної роботи було створено Checklist, три Test Cases та Decision Table для перевірки авторизації в SauceDemo. Усі три тест-кейси пройшли успішно. Decision Table допомогла виявити додаткові комбінації умов, які не були охоплені створеними тестами.


### 8. Контрольні питання

**1. Що таке Test Basis і навіщо він потрібний?**

Test Basis - це основа для тестування, яка містить вимоги та іншу інформацію, на основі якої створюються тести.

**2. Чим Test Condition відрізняється від Test Case?**

Test Condition визначає, що потрібно перевірити, а Test Case описує конкретні кроки перевірки та очікуваний результат.

**3. На яке питання відповідає Test Analysis?**

Test Analysis відповідає на питання: «Що потрібно тестувати?».

**4. На яке питання відповідає Test Design?**

Test Design відповідає на питання: «Як саме тестувати?».

**5. Для чого використовується checklist?**

Checklist використовується для швидкої перевірки функціональності за списком пунктів, які потрібно перевірити.

**6. Чому checklist не завжди може замінити детальний test case?**

Тому що checklist містить лише короткий перелік перевірок, а Test Case описує детальні кроки, тестові дані та очікуваний результат.

**7. Чим positive test відрізняється від negative test?**

Positive test перевіряє роботу системи з правильними даними, а negative test - реакцію системи на неправильні або некоректні дані.

**8. Чому negative test може мати результат Pass?**

Тому що Pass означає, що система правильно відреагувала на неправильні дані, наприклад відхилила авторизацію та показала відповідне повідомлення.

**9. Для чого використовується Decision Table?**

Decision Table використовується для перевірки різних комбінацій умов і визначення відповідних дій системи.

**10. Навіщо порівнювати Expected Result та Actual Result?**

Щоб перевірити, чи відповідає фактична поведінка системи очікуваній.

**11. Що означають Pass та Fail?**

Pass означає, що тест пройшов успішно і фактичний результат відповідає очікуваному. Fail означає, що фактичний результат не відповідає очікуваному.
