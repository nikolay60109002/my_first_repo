def load_data(filename):
  """Загружает данные из текстового файла."""
  data = {}
  try:
    with open(filename, "r") as f:
      for line in f:
        parts = line.strip().split(",")
        if len(parts) == 4:
          surname, name, patronymic, phone = parts
          data[surname] = {
            "surname": surname,
            "name": name,
            "patronymic": patronymic,
            "phone": phone
          }
  except FileNotFoundError:
    print(f"Файл {filename} не найден. Создан новый файл.")
  return data


def save_data(data, filename):
  """Сохраняет данные в текстовый файл."""
  with open(filename, "w") as f:
    for surname, info in data.items():
      f.write(f"{info['surname']},{info['name']},{info['patronymic']},{info['phone']}\n")


def print_data(data):
  """Выводит данные в удобочитаемом формате."""
  for surname, info in data.items():
    print(f"Фамилия: {info['surname']}")
    print(f"Имя: {info['name']}")
    print(f"Отчество: {info['patronymic']}")
    print(f"Номер телефона: {info['phone']}\n")


def search_data(data, key):
  """Ищет записи по имени или фамилии."""
  results = []
  for surname, info in data.items():
    if key.lower() in info['surname'].lower() or key.lower() in info['name'].lower():
      results.append(info)
  return results


def update_data(data, key, new_info):
  """Обновляет данные для заданной записи."""
  results = search_data(data, key)
  if results:
    for info in results:
      data[info['surname']] = new_info
    return True
  return False


def delete_data(data, key):
  """Удаляет записи по имени или фамилии."""
  results = search_data(data, key)
  if results:
    for info in results:
      del data[info['surname']]
    return True
  return False


def main():
  """Основная функция для управления телефонным справочником."""
  filename = "phonebook.txt"
  data = load_data(filename)

  while True:
    print("\nМеню:")
    print("1. Вывести все записи")
    print("2. Добавить новую запись")
    print("3. Изменить запись")
    print("4. Удалить запись")
    print("5. Поиск по имени или фамилии")
    print("6. Сохранить изменения")
    print("7. Выход")

    choice = input("Введите номер действия: ")

    if choice == "1":
      print_data(data)
    elif choice == "2":
      surname = input("Введите фамилию: ")
      name = input("Введите имя: ")
      patronymic = input("Введите отчество: ")
      phone = input("Введите номер телефона: ")
      data[surname] = {
        "surname": surname,
        "name": name,
        "patronymic": patronymic,
        "phone": phone
      }
    elif choice == "3":
      key = input("Введите фамилию или имя: ")
      update_data(data, key, input("Введите новую информацию: "))
    elif choice == "4":
      key = input("Введите фамилию или имя: ")
      delete_data(data, key)
    elif choice == "5":
      key = input("Введите фамилию или имя для поиска: ")
      results = search_data(data, key)
      if results:
        print_data(results)
      else:
        print("Записи с таким именем или фамилией не найдены.")
    elif choice == "6":
      save_data(data, filename)
    elif choice == "7":
      break
    else:
      print("Неверный выбор.")


if __name__ == "__main__":
  main()
