# Игра угадай слово #

Мини-игра 'Виселица', где нужно угадать слово записанное в текстовом файле, подбором букв. Дается 6 попыток. Слова и буквы приводятся к нижнему регистру.

## Функция get_rand_word(filename) ##

Открываем файл, читаем с него загаданное слово.
```commandline
def get_rand_word(filename):
    with open(filename, 'r') as f:
        return choice([x.strip() for x in f]).lower()
```

## Функция print_game(wrong, word, letters): ## 

Проверяем угадана ли буква, выводим пробел если нет, и показываем букву, если угадал.
```commandline
def print_game(wrong, word, letters):
    print('\nWrong guess:', ', '.join(wrong))
    for i in letters:
        if i is None:
            print(' ', end=' ')
        else:
            print(i, end=' ')
```

Заменяем каждую букву слова на '-'.
```commandline
for i in word:
    if i != ' ':
        print('-', end=' ')
    else:
        print(' ', end=' ')
print()
```

## Функция get_letter(attempt, guess) ##
Запрашиваем букву у пользователя, и проверяем это.
```commandline
def get_letter(attempt, guess):
    user_input = input(f'> Guess a letter ({attempt} attempts left): ').lower()
    if len(user_input) != 1 or user_input not in ascii_lowercase:
        print("> Please input a letter!")
```
Проверяем не вводил ли раньше пользователь эту же букву.
```
elif user_input in guess:
        print(f"> You have guessed '{user_input}' before")
    else:
        return user_input
```        


## Функция play() ##

Проверка. Если буква угадана, она добавляется в список верных букв. 
```
for i in word:
    i = None if i != ' ' else i
    letters.append(i)
```


Выводим количество неверных попыток, угаданные буквы в слове на своих местах. Если все буквы отгаданы, вы победили.
```
while attempt > 0:
        print_game(guess_wrong, word, letters)
        if None not in letters:
            print('> You win!')
            break
```
Если введенная буква пользователя, есть в слове, ставим ее на место.
```
user_letter = get_letter(attempt, guess)
        if user_letter is not None:
            for i in range(len(word)):
                if user_letter == word[i]:
                    letters[i] = user_letter                   
```
Обновляем статус попыток и список угаданных букв
```
guess.append(user_letter)
            if user_letter not in letters:
                guess_wrong.append(user_letter)
                attempt -= 1
```
После истечения попыток, выводится сообщение о проигрыше и ответ.
```commandline
else:
        print_game(guess_wrong, word, letters)
        print('> You lose...')
        print(f'> Answer: {word.capitalize()}')
```