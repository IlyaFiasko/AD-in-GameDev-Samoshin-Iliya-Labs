# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР[in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Самошин Илья Васильевич
- РИ-230918
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
установить необходимое программное обеспечение, нужное для создания интеллектуальных моделей на Python. Рассмотреть процесс установки Unity для разработки игр.

## Задание 1
### Написать программу Hello World на Python с запуском в Jupiter Notebook.
Ход работы:
- Скачать Anaconda и запустить Anaconda-Navigator.
- Запустить инструмент Jupyter Notebook и создать файл с именем HelloWorld.
- Открыть созданный файл с именем HelloWorld, написать в него команду print() с характерным содержанием и запустить выполнение, нажав Run.


![Скриншот 22-12-2024 071118](https://github.com/user-attachments/assets/32743c31-8db9-4318-a5f6-6883c6181ae9)


## Задание 2
### Написать программу Hello World на C# с запуском на Unity.
Ход работы:
- Скачать Unity и авторизоваться.
- Скачать среду разработки для работы с C# (Visual Studio 2022 Commynity).
- Создать 3D проект.
- Добавить пустой GameObject.
- Написать скрипт компонента, который будет выводить в консоль "Hello World!".

```C#

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Helloworld : MonoBehaviour 
{
    void Start()
    {
        Debug.Log("Hello world!");
    }
}


```
- Повесить скрипт на объект и запустить проект.
  ![image](https://github.com/user-attachments/assets/b563b643-5326-40c2-b353-5d4820c6d2d5)


## Задание 3
### Какую сущность(и) мы бы могли "обучить" ML-Agent-ом для того чтобы создать более качественный игровой опыт?
Сущность может выполнять задачи:
- Дрон/Птица помощник - помечает на время врагов/лут на карте.
- Сущность может атаковать по области смертельной атакой.

## Выводы

Я узнал как скачивать Anaconda и Unity, а также узнал начальные этапы их использования.



| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
