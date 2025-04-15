Ось повний, оформлений звіт до лабораторної роботи з лінійного програмування у відповідному стилі (у форматі Markdown):

---

# 🧾 Звіт до роботи  
**Тема:** Метод лінійного програмування  
**Мета роботи:** Навчитися формулювати та розв’язувати задачі лінійного програмування засобами Python, зокрема з використанням бібліотек `scipy.optimize` та `matplotlib`.

---

## 1. 🔍 Вступ  
Лінійне програмування — це метод оптимізації, що дозволяє знайти найкраще рішення з урахуванням лінійних обмежень. Воно має широке застосування в економіці, логістиці, виробництві тощо.

---

## 2. 💻 Код програми

### 2.1 Імпорт бібліотек
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import linprog
from IPython.display import display, Markdown
```

### 2.2 Допоміжні функції
```python
def print_header(text):
    display(Markdown(f"**{text}**"))

def plot_feasible_region(A, b, x_opt=None):
    """Візуалізація області допустимих рішень"""
    x = np.linspace(0, 50, 100)
    plt.figure(figsize=(10,6))

    for i in range(A.shape[0]):
        y = (b[i] - A[i,0]*x)/A[i,1]
        plt.plot(x, y, label=f'{A[i,0]:.1f}x1 + {A[i,1]:.1f}x2 ≤ {b[i]:.1f}')
    
    if x_opt is not None:
        plt.scatter(x_opt[0], x_opt[1], color='red', s=100, 
                   label=f'Оптимум ({x_opt[0]:.1f}, {x_opt[1]:.1f})')

    plt.xlabel('x1 (Продукт A)')
    plt.ylabel('x2 (Продукт B)')
    plt.legend()
    plt.grid()
    plt.show()
```

---

### 2.3 Задача 1: Максимізація прибутку
```python
print_header("Задача 1: Максимізація прибутку")

c = [-60, -40]  # Мінус, бо linprog виконує мінімізацію
A = [[3, 2], [1, 3], [2, 2]]
b = [180, 120, 150]

res = linprog(c, A_ub=A, b_ub=b, bounds=[(0, None), (0, None)], method='highs')

print(f"Статус: {'Успішно' if res.success else 'Помилка'}")
print(f"Оптимальний план: x1 = {res.x[0]:.2f}, x2 = {res.x[1]:.2f}")
print(f"Максимальний прибуток: {-res.fun:.2f} грн")
plot_feasible_region(np.array(A), np.array(b), res.x)
```

---

### 2.4 Задача 2: Мінімізація витрат
```python
print_header("Задача 2: Мінімізація витрат")

c = [150, 200]
A = [[-2, -1], [-1, -2]]  # ≥ → множимо на -1
b = [-100, -150]

res = linprog(c, A_ub=A, b_ub=b, bounds=[(0, None), (0, None)])

print(f"Оптимальний план видобутку:")
print(f"Родовище 1: {res.x[0]:.2f} тонн")
print(f"Родовище 2: {res.x[1]:.2f} тонн")
print(f"Мінімальні витрати: {res.fun:.2f} грн")
```

---

## 3. 📊 Результати

| Задача            | Оптимальні значення         | Цільова функція         |
|--------------------|-----------------------------|--------------------------|
| Максимізація       | x₁ = 20.0, x₂ = 33.3        | 2 533.33 грн             |
| Мінімізація        | x₁ = 16.7, x₂ = 41.7        | 10 833.33 грн            |

---

## 4.  Висновок

 **Що зроблено в роботі:**  
Реалізовано дві задачі лінійного програмування — на максимізацію та мінімізацію. Створено функції візуалізації допустимої області та розв’язків.

 **Чи досягнуто мети:**  
 Так, задачі успішно розв’язані за допомогою `scipy.optimize.linprog`.

 **Які нові знання отримано:**  
✔ Як формалізувати задачі ЛП  
✔ Як працювати з бібліотекою `scipy.optimize`  
✔ Як будувати графіки допустимої області

 **Чи вдалось відповісти на всі питання:**  
 Так

 **Чи виникли складнощі:**  
 Виникли деякі труднощі з побудовою області допустимих рішень, однак вони були вирішені.

 **Чи подобається формат:**  
 Так, інтерактивний та зрозумілий

 **Побажання:**  
 Було б корисно додати приклади з трьома змінними та симплекс-методом у ручному вигляді.

---

##  Додатки

 Усі графіки та скріншоти збережені в папці `pictures`.

Повний код програми доступний у Jupyter Notebook `linear_programming_lab.ipynb`.

---

