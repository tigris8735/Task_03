# Task_03
# Практическое задание: Создание Python-приложения с тестированием

## 📋 Обзор задания
**Формат:** Работа в командах по 2 человека  

## 🎯 Цели обучения
- Освоить создание консольных приложений на Python
- Научиться работать с файлами различных форматов
- Освоить основы тестирования с помощью PyTest
- Научиться писать интеграционные тесты
- Практиковать работу с Git в командной разработке

---

## ⚙️ ПОДГОТОВИТЕЛЬНЫЙ ЭТАП (20 минут)

### Шаг 1.1: Создание и клонирование репозитория GitHub

**Инструкция выполнения:**
1. Один из участников команды создает новый репозиторий на GitHub:
   - Описание: "Python приложение для приветствия студентов с метриками"
   - Public repository
   - **Добавить README.md** (это важно!)
   - Добавить .gitignore: Python

2. Второй участник получает приглашение в collaborators:
   - Settings → Collaborators → Add people
   - Ввести username второго участника
   - Отправить приглашение

3. Оба участника клонируют репозиторий:
```bash
git clone https://github.com/ВАШ_USERNAME/student-greeting-app.git
cd student-greeting-app
```
### Настройка окружения Python

```bash
python -m venv student_app_env
```

```bash
# Для Windows:
student_app_env\Scripts\activate
# Для Mac/Linux:
source student_app_env/bin/activate
```
** Установка Pytest **
```
pip install pytest
```

## Первоначальная настройка Git

**Настройте глобальные параметры Git (если не сделано ранее):**
```bash
git config --global user.name "Ваше Имя Фамилия"
git config --global user.email "ваш_email@example.com"
```
**Проверьте подключение к удаленному репозиторию:**
```bash
git remote -v
```
**Создайте первую ветку для разработки:**
```
git checkout -b development
```

## Создание структуры файлов проекта 
** Необходимые файлы проекта **
```bash
touch app.py
touch students.md
touch test_app.py
touch requirements.txt
```
# Student Greeting App

Python приложение для приветствия студентов с отображением их метрик.

## Команда разработки
- [Имя студента 1]
- [Имя студента 2]

## Описание
Приложение ожидает ввод ФИО студента и выводит приветствие с метриками (рост, вес, возраст, ИМТ). Если студент не найден, предлагает зарегистрироваться.

## Установка
```bash
git clone https://github.com/[USERNAME]/student-greeting-app.git
cd student-greeting-app
python -m venv student_app_env
# Активируйте виртуальное окружение
pip install -r requirements.txt
```

**Запуск** 
```bash
python app.py
```
** Тестирование **
```bash
pytest test_app.py -v
```

**Проверка:**
- [ ] Все файлы созданы
- [ ] README.md обновлен
- [ ] Структура проекта готова к разработке

---

# База данных студентов

## Список студентов

| ФИО | Группа | Колледж | Год поступления | Курс |
|-----|--------|---------|-----------------|------|
| Иванов Иван Иванович | ТВ-101 | Технический колледж | 2023 | 2 |
| Петрова Анна Сергеевна | ИС-102 | Колледж информационных систем | 2022 | 3 |
| Сидоров Алексей Петрович | ЭК-103 | Экономический колледж | 2023 | 2 |
| Козлова Мария Дмитриевна | ДЗ-104 | Колледж дизайна | 2024 | 1 |
| Николаев Денис Сергеевич | МТ-105 | Механико-технологический колледж | 2022 | 3 |

## Формат данных
- **ФИО**: Полное имя студента
- **Группа**: Учебная группа
- **Колледж**: Название учебного заведения
- **Год поступления**: Год начала обучения
- **Курс**: Текущий курс обучения

## Ссылка для скачивания
[📥 Скачать базу данных студентов](./students.md)

#Создание requirements.txt

```bash
pip freeze > requirements.txt
```

## РАЗРАБОТКА ПРИЛОЖЕНИЯ (45 минут)
** Создание основной логики приложения с новыми метриками**
Инструкция выполнения:

Распределение задач:

Студент 1: Функции чтения данных и поиска студентов

Студент 2: Функции приветствия и регистрации с новыми метриками

Создайте ветки для разработки

Реализуйте функции в app.py:

**Для студента 1 (функции работы с данными): **
```python
import re

def read_students_from_md(filename):
    """
    Чтение данных студентов из markdown файла с новыми метриками
    Возвращает список словарей с информацией о студентах
    """
    students = []
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            content = file.read()
            
        # Регулярное выражение для поиска строк таблицы с новыми полями
        table_pattern = r'\|\s*(.+?)\s*\|\s*(.+?)\s*\|\s*(.+?)\s*\|\s*(\d{4})\s*\|\s*(\d+)\s*\|'
        matches = re.findall(table_pattern, content)
        
        for match in matches:
            full_name, group, college, admission_year, course = match
            if full_name.lower() != 'фио':  # Пропускаем заголовок
                students.append({
                    'full_name': full_name.strip(),
                    'group': group.strip(),
                    'college': college.strip(),
                    'admission_year': int(admission_year),
                    'course': int(course)
                })
                
    except FileNotFoundError:
        print(f"Ошибка: Файл {filename} не найден")
    except Exception as e:
        print(f"Ошибка при чтении файла: {e}")
        
    return students

def find_student(students, full_name):
    """
    Поиск студента по ФИО (регистронезависимый)
    """
    for student in students:
        if student['full_name'].lower() == full_name.lower():
            return student
    return None
```
**Для студента 2 (функции приветствия и регистрации):**
```python
def generate_greeting(student):
    """
    Генерация приветствия с метриками студента
    """
    current_year = 2024  # Можно заменить на получение текущего года
    years_studying = current_year - student['admission_year'] + 1
    
    greeting = f"\n🎓 Добро пожаловать, {student['full_name']}!"
    greeting += f"\n{'='*50}"
    greeting += f"\n📊 Ваши образовательные метрики:"
    greeting += f"\n├─ 🏫 Колледж: {student['college']}"
    greeting += f"\n├─ 👥 Группа: {student['group']}"
    greeting += f"\n├─ 📅 Год поступления: {student['admission_year']}"
    greeting += f"\n├─ 📚 Текущий курс: {student['course']}"
    greeting += f"\n└─ ⏱️ Лет обучения: {years_studying}"
    
    # Дополнительная информация на основе метрик
    if student['course'] == 1:
        greeting += f"\n\n🌟 Вы только начинаете свой образовательный путь!"
    elif student['course'] >= 3:
        greeting += f"\n\n🎯 Вы уже опытный студент! Скоро диплом!"
    
    return greeting

def display_student_info(student):
    """
    Отображение информации о студенте с приветствием
    """
    greeting = generate_greeting(student)
    print(greeting)

def register_new_student(full_name, students, filename):
    """
    Регистрация нового студента с запросом новых метрик
    """
    print(f"\n❌ Студент {full_name} не найден в базе.")
    response = input("Хотите зарегистрироваться? (да/нет): ").lower().strip()
    
    if response == 'да':
        try:
            print("\n📝 Регистрация нового студента:")
            group = input("Введите учебную группу: ")
            college = input("Введите название колледжа: ")
            admission_year = int(input("Введите год поступления: "))
            course = int(input("Введите текущий курс: "))
            
            # Валидация данных
            if admission_year < 2000 or admission_year > 2024:
                print("❌ Ошибка: Некорректный год поступления")
                return None
            if course < 1 or course > 6:
                print("❌ Ошибка: Некорректный номер курса")
                return None
            
            new_student = {
                'full_name': full_name,
                'group': group,
                'college': college,
                'admission_year': admission_year,
                'course': course
            }
            students.append(new_student)
            
            # Добавляем в файл
            with open(filename, 'a', encoding='utf-8') as file:
                file.write(f"\n| {full_name} | {group} | {college} | {admission_year} | {course} |")
            
            print("✅ Студент успешно зарегистрирован!")
            return new_student
            
        except ValueError:
            print("❌ Ошибка: Введите корректные числовые значения")
        except Exception as e:
            print(f"❌ Ошибка при регистрации: {e}")
    
    return None
```
#Проверка:

Все новые метрики реализованы

Приветствие включает все 5 полей

Регистрация запрашивает все метрики

#Реализация основного цикла программы
-Добавьте основную функцию в app.py:
```python
def main():
    """
    Основная функция приложения
    """
    filename = 'students.md'
    students = read_students_from_md(filename)
    
    print("🎓 Система управления студентами")
    print("=" * 50)
    print("Доступные команды:")
    print("• Введите ФИО студента для поиска")
    print("• 'список' - показать всех студентов")
    print("• 'выход' - завершить программу")
    
    while True:
        print("\n" + "-" * 30)
        user_input = input("Введите команду: ").strip()
        
        if user_input.lower() == 'выход':
            print("👋 До свидания!")
            break
        elif user_input.lower() == 'список':
            print("\n📋 Список всех студентов:")
            for i, student in enumerate(students, 1):
                print(f"{i}. {student['full_name']} - {student['group']}")
        else:
            student = find_student(students, user_input)
            
            if student:
                display_student_info(student)
            else:
                new_student = register_new_student(user_input, students, filename)
                if new_student:
                    display_student_info(new_student)

if __name__ == "__main__":
    main()
```
**Протестируйте работу приложения**
Проверка:

Основной цикл работает корректно

Все команды обрабатываются

Приветствие выводит все метрики


##ЭТАП 3: НАПИСАНИЕ ТЕСТОВ (45 минут)

**Создание unit-тестов для новых метрик**


Инструкция выполнения:

Распределение задач:

Студент 1: Тесты для функций работы с данными

Студент 2: Тесты для функций приветствия и валидации

Создайте ветки для тестирования

Реализуйте тесты в test_app.py:

**Для студента 1 (тесты данных):**
```python
import pytest
import os
from app import read_students_from_md, find_student

class TestStudentData:
    """Тесты для работы с данными студентов"""
    
    def test_read_students_with_new_metrics(self):
        """Тест чтения студентов с новыми метриками"""
        students = read_students_from_md('students.md')
        
        assert len(students) > 0
        student = students[0]
        
        # Проверяем наличие всех новых полей
        assert 'full_name' in student
        assert 'group' in student
        assert 'college' in student
        assert 'admission_year' in student
        assert 'course' in student
        
        # Проверяем типы данных
        assert isinstance(student['full_name'], str)
        assert isinstance(student['group'], str)
        assert isinstance(student['college'], str)
        assert isinstance(student['admission_year'], int)
        assert isinstance(student['course'], int)
    
    def test_find_student_by_full_name(self):
        """Тест поиска студента по ФИО"""
        test_students = [
            {
                'full_name': 'Иванов Иван Иванович',
                'group': 'ТВ-101',
                'college': 'Технический колледж',
                'admission_year': 2023,
                'course': 2
            }
        ]
        
        student = find_student(test_students, 'Иванов Иван Иванович')
        assert student is not None
        assert student['group'] == 'ТВ-101'
        assert student['college'] == 'Технический колледж'
    
    def test_student_metrics_validation(self):
        """Тест валидности метрик студентов"""
        students = read_students_from_md('students.md')
        
        for student in students:
            # Проверяем корректность года поступления
            assert 2000 <= student['admission_year'] <= 2024
            # Проверяем корректность курса
            assert 1 <= student['course'] <= 6
            # Проверяем, что все поля заполнены
            assert student['full_name'] != ''
            assert student['group'] != ''
            assert student['college'] != ''
```
Для студента 2 (тесты приветствия и логики):
```python
import pytest
from app import generate_greeting, register_new_student
import io
import sys

class TestGreetingLogic:
    """Тесты для логики приветствия и регистрации"""
    
    def test_generate_greeting_contains_all_metrics(self):
        """Тест, что приветствие содержит все метрики"""
        test_student = {
            'full_name': 'Тестовый Студент',
            'group': 'ТЕСТ-101',
            'college': 'Тестовый колледж',
            'admission_year': 2023,
            'course': 2
        }
        
        greeting = generate_greeting(test_student)
        
        # Проверяем наличие всех метрик в приветствии
        assert test_student['full_name'] in greeting
        assert test_student['group'] in greeting
        assert test_student['college'] in greeting
        assert str(test_student['admission_year']) in greeting
        assert str(test_student['course']) in greeting
        
        # Проверяем дополнительные вычисляемые метрики
        assert 'Лет обучения' in greeting
    
    def test_greeting_course_specific_messages(self):
        """Тест специальных сообщений для разных курсов"""
        # Тест для первого курса
        first_year_student = {
            'full_name': 'Студент 1 курс',
            'group': 'ГР-101',
            'college': 'Колледж',
            'admission_year': 2024,
            'course': 1
        }
        
        greeting_1 = generate_greeting(first_year_student)
        assert 'начинаете' in greeting_1.lower()
        
        # Тест для старших курсов
        senior_student = {
            'full_name': 'Студент 3 курс',
            'group': 'ГР-301',
            'college': 'Колледж',
            'admission_year': 2022,
            'course': 3
        }
        
        greeting_3 = generate_greeting(senior_student)
        assert 'опытный' in greeting_3.lower() or 'диплом' in greeting_3.lower()
    
    def test_student_years_calculation(self):
        """Тест расчета лет обучения"""
        test_student = {
            'full_name': 'Тестовый Студент',
            'group': 'ТЕСТ-101',
            'college': 'Тестовый колледж',
            'admission_year': 2022,
            'course': 3
        }
        
        greeting = generate_greeting(test_student)
        # Должно быть 3 года обучения (2024-2022+1)
        assert '3' in greeting  # Лет обучения
```
**Создание интеграционных тестов**
Инструкция выполнения:

Добавьте интеграционные тесты в test_app.py:
```python
class TestIntegration:
    """Интеграционные тесты всего приложения"""
    
    def test_complete_student_workflow(self):
        """Тест полного workflow приложения"""
        from app import read_students_from_md, find_student, generate_greeting
        
        # Чтение данных
        students = read_students_from_md('students.md')
        assert len(students) > 0
        
        # Поиск существующего студента
        if students:
            existing_student = find_student(students, students[0]['full_name'])
            assert existing_student is not None
            
            # Генерация приветствия
            greeting = generate_greeting(existing_student)
            assert existing_student['full_name'] in greeting
    
    def test_data_consistency(self):
        """Тест согласованности данных"""
        students = read_students_from_md('students.md')
        
        for student in students:
            # Проверяем, что курс соответствует году поступления
            expected_min_course = 2024 - student['admission_year'] + 1
            # Допускаем погрешность в 1 курс
            assert abs(student['course'] - expected_min_course) <= 1
    
    def test_file_modification_integration(self):
        """Тест интеграции с файловой системой"""
        import tempfile
        import os
        from app import read_students_from_md
        
        # Создаем временный файл для тестирования
        with tempfile.NamedTemporaryFile(mode='w', suffix='.md', delete=False) as f:
            f.write("""# Test Students
| ФИО | Группа | Колледж | Год поступления | Курс |
|-----|--------|---------|-----------------|------|
| Тест Студент | Т-001 | Тест колледж | 2023 | 2 |""")
            temp_filename = f.name
        
        try:
            students = read_students_from_md(temp_filename)
            assert len(students) == 1
            assert students[0]['full_name'] == 'Тест Студент'
        finally:
            os.unlink(temp_filename)
```
Запустите все тесты:

```bash
pytest test_app.py -v
pytest --cov=app test_app.py
```
Проверка:

Все тесты проходят

Покрытие новых метрик 100%

Интеграционные тесты работают

## ЭТАП 4: СЛИЯНИЕ ИЗМЕНЕНИЙ (30 минут)
**Создание PR, Code Review и слияние**
Инструкция выполнения:

Создайте Pull Request для каждой ветки

Проведите взаимный code review

Слейте изменения в main ветку

Убедитесь, что приложение работает с новыми метриками

Проверка:

Все PR созданы и approved

Код соответствует стандартам

Приложение работает корректно

Все тесты проходят
### КРИТЕРИИ УСПЕШНОГО ВЫПОЛНЕНИЯ
Репозиторий создан на GitHub и доступен обоим участникам

Приложение использует все 5 новых метрик (ФИО, Группа, Колледж, Год поступления, Курс)

Приветствие содержит все метрики в читаемом формате

Регистрация новых студентов запрашивает все метрики

Unit-тесты покрывают все новые функции и метрики

Интеграционные тесты проверяют полный workflow

Все тесты проходят

Работа с Git организована через ветки и PR
