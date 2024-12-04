<p align="center"><b>МОНУ НТУУ КПІ ім. Ігоря Сікорського ФПМ СПіСКС</b></p>

<p align="center">
<b>Звіт з лабораторної роботи 2</b><br/>
"Рекурсія"<br/>
дисципліни "Вступ до функціонального програмування"
</p>

<p align="right"><b>Студентка</b>: Петрук Ольга Сергіївна КВ-11</p>
<p align="right"><b>Рік</b>: 2024</p>

## Загальне завдання

Реалізуйте дві рекурсивні функції, що виконують деякі дії з вхідним(и) списком(-ами), за можливості/необхідності використовуючи різні види рекурсії. Функції, які необхідно реалізувати, задаються варіантом (п. 2.1.1). Вимоги до функцій:

1. Зміна списку згідно із завданням має відбуватись за рахунок конструювання нового списку, а не зміни наявного (вхідного).
2. Не допускається використання функцій вищого порядку чи стандартних функцій для роботи зі списками, що не наведені в четвертому розділі навчального посібника.
3. Реалізована функція не має бути функцією вищого порядку, тобто приймати функції в якості аргументів.
4. Не допускається використання псевдофункцій (деструктивного підходу).
5. Не допускається використання циклів. Кожна реалізована функція має бути протестована для різних тестових наборів. Тести мають бути оформленні у вигляді модульних тестів

## Варіант 4

1. Написати функцію remove-seconds-and-reverse, яка видаляє зі списку кожен другий елемент і обертає результат у зворотному порядку:

```lisp
CL-USER> (remove-seconds-and-reverse '(1 2 a b 3 4 d))
(D 3 A 1)
```

2. Написати функцію list-set-difference , яка визначає різницю двох множин, заданих списками атомів:

```lisp
CL-USER> (list-set-difference '(1 2 3 4) '(3 4 5 6))
(1 2) ; порядок може відрізнятись
```

## Лістинг функції remove-seconds-and-reverse

```lisp
(defun remove-seconds (lst count)
  (cond
    ((null lst) nil)
    ((= (mod count 2) 0)
     (remove-seconds (cdr lst) (+ count 1)))
    (t
     (cons (car lst)
           (remove-seconds (cdr lst) (+ count 1))))))

(defun reverse-list (lst)
  (if (null lst)
      nil
      (append (reverse-list (cdr lst)) (list (car lst)))))

(defun remove-seconds-and-reverse (lst)
  (reverse-list (remove-seconds lst 1)))
```

### Тестові набори

```lisp
(defun check-remove-seconds-and-reverse (test-name list expected)
  (let ((result (remove-seconds-and-reverse list)))
    (if (equal result expected)
        (format t "Passed: ~a~%" test-name)
        (format t "Failed: ~a~%" test-name))))d

(defun test-remove-seconds-and-reverse ()
  (check-remove-seconds-and-reverse "Test 1" '(1 2 a b 3 4 d) '(d 3 a 1))
  (check-remove-seconds-and-reverse "Test 2" '(a b c d e f) '(e c a))
  (check-remove-seconds-and-reverse "Test 3" '(1 2 3) '(3 1))
  (check-remove-seconds-and-reverse "Test 4" nil nil)
  (check-remove-seconds-and-reverse "Test 5" '(1) '(1)))

(test-remove-seconds-and-reverse)
```

### Тестування

```
Passed: Test 1
Passed: Test 2
Passed: Test 3
Passed: Test 4
Passed: Test 5
```

## Лістинг функції list-set-difference

```lisp
(defun contains (keys value)
  (cond
    ((null keys) nil)
    ((equal (first keys) value) t)
    (t (contains (cdr keys) value))))

(defun list-set-difference (first-lst second-lst)
  (cond
    ((null first-lst) nil)
    ((contains second-lst (first first-lst))
     (list-set-difference (cdr first-lst) second-lst))
    (t (cons (first first-lst)
             (list-set-difference (cdr first-lst) second-lst)))))
```

### Тестові набори

```lisp
(defun check-list-set-difference (test-name first-lst second-lst expected)
  (let ((result (list-set-difference first-lst second-lst)))
    (if (equal result expected)
        (format t "Passed: ~a~%" test-name)
        (format t "Failed: ~a~%" test-name))))

(defun test-list-set-difference ()
  (check-list-set-difference "Test 1" '(1 2 3 4) '(3 4 5 6) '(1 2))
  (check-list-set-difference "Test 2" '(a b c) '(d e f) '(a b c))
  (check-list-set-difference "Test 3" '(1 2 3) '(1 2 3) nil)
  (check-list-set-difference "Test 4" nil '(1 2 3) nil)
  (check-list-set-difference "Test 5" '(1 2 3) nil '(1 2 3)))

(test-list-set-difference)
```

### Тестування

```
Passed: Test 1
Passed: Test 2
Passed: Test 3
Passed: Test 4
Passed: Test 5
```
