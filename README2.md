# Применение регулярных выражений при работе с данными в Cultural Studies


**Выполнили:** Хорольская Александра и Васильева Юлия


**Библиотека:** re 


![140631977_2778641432398914_4821678329643134246_n](https://user-images.githubusercontent.com/90963236/138100732-6beb58c9-2fca-4cc9-9d6e-97433e16a8dc.png)


## Чем могут помочь регулярные выражения при выполнении задач в области культурологии? 

*«Регулярками»* называются шаблоны, которые используются для поиска соответствующего фрагмента текста и сопоставления символов.

Шаблон — это набор символов, который проверяет строку на соответствие заданному правилу.

| Спец.символ  | Зачем нужен |
| ------------- | ------------- |
| . | Задает **один** произвольный символ |
| []  | Заменяет символ из квадратных скобок |
| - | Задает один символ, которого не должно быть в скобках |
| [^ ]  | Задает один символ из **не** содержащихся в квадратных скобках |
| ^ | Обозначает начало последовательности |
| $ | Обозначает окончание строки |
| * | Обозначает повторение символа >= 0 количество раз |
| ? | Обозначает строго одно повторение символа |
| + | Обозначает повторение символа >= 1 количество раз |
| I | (вертикальная черта) | Логическое **ИЛИ** |
| \ | Экранирование. Для использования метасимволов в качестве обычных|
| () | Группирует символы внутри |
| {} | Указывает число повторений предыдущего символа |

**Поиск информации**

Регулярные выражения помогают находить нужные детали в больших текстах и базах данных. 

Есть какой-то текст, где нужно найти что-то конкретное. Для этого сначала прописывается название библиотеки, затем метод поиска, а в скобках сам шаблон и через запятую указывается строчку, в которой хотим искать.

**_основные функции:_**

[ссылка на код](https://colab.research.google.com/drive/15Hhv1Ep_3UtyjOAAbg9siKqUIiCjNz9C#scrollTo=3eP7-kSPAtJm)

**_ищет совпадение в первом слове в строке_**
```{python}
import re
result = re.match(r'\bпут\w+', text)
print('re.match:', result)

```
```{python}
re.match: None
```

**_ищет первое совпадение_**

```{python}
result2 = re.search(r'\bпут\w+', text)
print('re.search:', result2.group())
```
```{python}
re.search: путь
```

**_ищет все совпадения в тексте_**

```{python}
result3 = re.findall(r'\bпут\w+', text)
print('re.findall:', result3)
```

```{python}
re.findall: ['путь', 'пути']
```
**Анализ информации**

Регулярные выражения помогают выявить паттерны употребления символов в разных контекстах и блоках информации. 

**Замена шаблонов**

Можно заменить найденный шаблон на что-то другое. Чем это полезно? 

### Пример
 
Допустим, вам нужно проанилизровать чей-то твиттер. 
1. Вы скачиваете твиты нужного вам пользователя. Например, на [AllMyTweets](https://www.allmytweets.net/connect/)

Выглядит это так: 
 
 ![Снимок экрана 2021-10-20 в 17 49 15](https://user-images.githubusercontent.com/90963236/138120071-a18fe50a-d5c9-45b7-a314-a0cb67f2c764.png)

2. Загружаем текст. 

```{python}
text = ''' CleanTechnica | @cleantechnica : US Tesla sales were up 67% in Q3 2021 vs. Q3 2020, while overall US auto sales were down 13%. Important trend? https://t.co/l5JKels0iE https://t.co/lX9pr2zPJs Oct 20, 2021 
Tesla Silicon Valley Club | @teslaownersSV : #FSDBeta visualization is artwork @elonmusk https://t.co/PjAHlsg80o Oct 20, 2021 
SPACE.com | @SPACEdotcom : SpaceX fires up SN20 Starship prototype for 1st time https://t.co/Wk64eihfOJ https://t.co/hyKHihFuvY Oct 19, 2021 
NASA Watch | @NASAWatch : #Starship vs Saturn V Credit - u/hellraiserl33t on Reddit https://t.co/eDjahwULSj Oct 19, 2021 
cnunezimages | @cnunezimages : Setting Crescent - @elonmusk @SpaceX #BocaChicaToMars #iCANimagine LLC https://t.co/7O3nQ56tw8 @SpaceIntellige3 - Image Taken: October 8, 2021 - https://t.co/UZnBPzlUgF Oct 19, 2021 
```
 Выглядит это все пугающе - тут и ссылки, и упоминания, и хэштеги.  
 
![imgonline-com-ua-Resize-96QIrTaCAaY3](https://user-images.githubusercontent.com/90963236/138121868-ba15238e-aa3f-41a9-a011-0bfbb6f64c0e.jpg)

Тут нам поможет метод **re.sub:**

re.sub позволяет заменить один шаблон на другой 

[ссылка на код](https://colab.research.google.com/drive/15Hhv1Ep_3UtyjOAAbg9siKqUIiCjNz9C#scrollTo=3eP7-kSPAtJm)

```{python}
def clean_tweet(text):  
	text = re.sub('http\S+\s*', '', text) # удалит URL  
	text = re.sub('RT|cc', '', text) # удалит RT и cc  
	#text = re.sub('#\S+', '', text) # удалит хештеги  
	text = re.sub('@\S+', '', text) # удалит упоминания 
```

Теперь выглядит симпатичнее: 

```{python}
CleanTechnica |  : US Tesla sales were up 67% in Q3 2021 vs. Q3 2020, while overall US auto sales were down 13%. Important trend? Oct 20, 2021
Tesla Silicon Valley Club |  :  visualization is artwork  Oct 20, 2021 
SPACE.com |  : SpaceX fires up SN20 Starship prototype for 1st time Oct 19, 2021 
NASA Watch |  :  vs Saturn V Credit - u/hellraiserl33t on Reddit Oct 19, 2021 
cnunezimages |  : Setting Crescent -     LLC  - Image Taken: October 8, 2021 - Oct 19, 2021
```

Чистить текст можно до тех пор, пока не останется только нужное. 

А если надо что-то найти? 

_Как часто Илон Маск упоминает Tesla?_

```{python}
result = re.findall(r'\bTesla\w*', text)
print(result)
['Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Teslas', 'Teslaconomics', 'Teslaconomics', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'Tesla', 'TeslaCharging', 'Tesla', 'Tesla', 'Tesla', 'Tesla']
```
_Какие хэштеги использует/лайкает Илон Маск?_

```{python}
result = re.findall(r'#\S+', text)
print('хэштеги:', result)
хэштеги: ['#FSDBeta', '#Starship', '#BocaChicaToMars', '#iCANimagine', '#1', '#SpaceX', '#Starship', '#launch', '#3danimation', '#colaboration', '#Tesla', '#Mars', '#Falcon9', '#Starbase', '#tsla', '#dogecoin', '#tesla', '#doge', '#dogecoin', '#dogearmy', '#Crypto', '#respect', '#1', '#Tesla', '#FSDBeta', '#Shenzhou13']
```

_Посмотреть даты твитов?_

```{python}
result = re.findall(r'\w+\s[0-9]{2}\,\s[0-9]{4}', text)
print(result)
['Oct 20, 2021', 'Oct 20, 2021', 'Oct 19, 2021', 'Oct 19, 2021', 'Oct 19, 2021', 'Oct 19, 2021', 'Oct 19, 2021', 'Oct 18, 2021', 'Oct 18, 2021']
```
![EDkQVY5WkAEV0Pi](https://user-images.githubusercontent.com/90963236/138132828-f9004581-11ec-44d1-bf92-d8198d64f23c.jpg)

**Вывод:**
Таким образом, мы выяснили, что мы можем применять регулярные выражения для решения различных культурологических задач. В частности, с помощью регулярных выражений мы можем провести анализ цифровой среды на наличие частотности употребления каких-либо слов, фраз, мемов - культурных единиц; отследить популярность тех или иных хэштегов, имен, культурных феноменов. Анализ культурной повестки.
