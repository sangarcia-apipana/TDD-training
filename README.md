# TDD Workshop: Norwegian ID (Fødselsnummer) Validator

Exercise to implement a **Norwegian ID (Fødselsnummer) validator**. To do this, you first need to understand how it is structured and what validation rules apply.

---

## 1. Structure of the Norwegian ID

A **Fødselsnummer** consists of **11 digits** distributed as follows:
1. **DD (D1, D2):** Day of birth (2 digits)
2. **MM (D3, D4):** Month of birth (2 digits)
3. **YY (D5, D6):** Year of birth (last 2 digits)
4. **III (D7, D8, D9):** “Individnummer” (3 digits)
5. **K1 (D10):** First control digit
6. **K2 (D11):** Second control digit

> **Note on the “Individnummer” (D7, D8, D9):**
> - Among other things, it helps differentiate centuries or indicates gender (the last digit is even for women and odd for men).
> - For this workshop, simply **do not invalidate it** unless you want to add optional validations.

---

## 2. Validation Requirements

1. **Length:** The ID must have **11 digits**.
2. **Numeric format:** All characters must be digits (0-9).
3. **Date validity (DDMMYY):**
   - The day and month must correspond to a real date.
   - Optionally, you can decide how to handle the century (19xx, 20xx) according to the “Individnummer”. In this case, we will use the range 000 - 499 corresponding to those born between 1900 and 1999.
4. **Control digits calculation (K1 and K2):**
   - A **mod 11** calculation with predefined weights is used.
   - **K1** is calculated based on the first 9 digits (D1-D9) and **K2** is calculated based on the first 9 digits + K1.
   - The standard procedure is:

     ```
     K1 = 11 - ( (3×D1) + (7×D2) + (6×D3) + (1×D4) + (8×D5) + (9×D6) + (4×D7) + (5×D8) + (2×D9) ) mod 11
     ```
      - If the result is 11, then K1 = 0.
      - If the result is 10, **the ID is invalid** (there is no K1 = 10).
      - Otherwise, K1 is the result.

     ```
     K2 = 11 - ( (5×D1) + (4×D2) + (3×D3) + (2×D4) + (7×D5) + (6×D6) + (5×D7) + (4×D8) + (3×D9) + (2×K1) ) mod 11
     ```
      - If the result is 11, then K2 = 0.
      - If the result is 10, **the ID is invalid**.
      - Otherwise, K2 is the result.

   - **Final validation:** D10 must match K1, and D11 must match K2.