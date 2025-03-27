# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР[in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Самошин Илья Васильевич
- РИ-230918
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | ? |
| Задание 2 | * | ? |
| Задание 3 | * | ? |

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
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

# Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python. Также научиться анализировать игровые переменные, описывать их изменение и поведение

# Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
## Ход работы:
Переменная "здоровье" (hp) отвечает за состояние игрового персонажа, показывая, насколько он жив. Кроме того, ее значение определяет условия завершения игры.
Изменение здоровья:
 - Уменьшается при получении урона.
 - Восстанавливается, если игрок выбрал соответствующие навыки во время прокачки.
 - Допустимый диапазон: от 0 до установленного в игре максимума.

Схема экономической модели ресурса:

![ForLab2](https://github.com/user-attachments/assets/d38d444f-d3ff-40e2-aab3-5f778a56548b)

## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.
Ход работы:
- Для начала нужно определить как меняется выбранная мной игровая переменная:
Здоровье игрока меняется в зависимсоти от получения им урона, это значит, что изменение этой переменной зависит от игрока и его навыка. Поскольку есть множество вариантов изменения этого параметра, я, для простоты, взял ситуацию когда игрок просто получает урон с некоторым промежутком времени.
- Из-за простоты выбранной ситуации график изменения переменной выглядит как обычная прямая (В более сложных ситуациях график может иметь более сложное поведене):
![Скриншот 27-03-2025 222533](https://github.com/user-attachments/assets/af883aa3-0a1f-4d89-a81b-4cf347c4703c)

Ссылка на таблицу: [https://docs.google.com/spreadsheets/d/1-TBYwyDcrPz_f4pHD6Jv8I1gbH_UNFmD-HplJS98Nkk/edit?gid=0#gid=0](https://docs.google.com/spreadsheets/d/1sONm1NLdqv88rz78jZCw10VEfvIbb1_S334q4q9qT2M/edit?gid=0#gid=0)
- Из графика видно что величина меняться от 30 (лимит здоровья) до 0 (конец игры), уменьшение величины происходит когда игрок получает урон (в данном примере -10),   а увеличение когда игрок либо "вампириться", либо заканчивает волну. Недостатков в реализации величины нету, посколько она изменяется довольно логично и просто
- Заполнение таблицы было выполненно с помощью кода на Python:
```Python
import gspread
import random
from google.oauth2.service_account import Credentials

# Настройка подключения
gc = gspread.service_account(filename = 'unityad-lab2-7beec262e3ca.json')
sh = gc.open("ForLab2AD")
worksheet = sh.get_worksheet(0)

# Генерация данных с элементами случайности
current_hp = 30
wave_number = 1
damage_variations = [8, 10, 12]  # Вариативный урон

worksheet.clear()
worksheet.update('A1', 'Wave')
worksheet.update('B1', 'Health')

while current_hp > 0:
    damage = random.choice(damage_variations)
    worksheet.update(f'A{wave_number+1}', wave_number)
    worksheet.update(f'B{wave_number+1}', current_hp)
    
    print(f"Wave {wave_number}: Health {current_hp}")
    current_hp = max(0, current_hp - damage)
    wave_number += 1

# Добавляем последнюю запись (0 HP)
worksheet.update(f'A{wave_number+1}', wave_number)
worksheet.update(f'B{wave_number+1}', 0)
```
## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
- Для реализации данного задания я выделил правильно по которым описывается динамика изменения здоровья:
    - Если хп около лимита (в моем случае hp >= 30), то воспроизводится звук "хороший"
    - Если хп около среднего значения (в моем случае 30 > hp >= 20), то воспроизводится звук "средний"
    - Если хп около минимума (в моем случае hp <= 0), то воспроизводится звук "плохой"
- Для отображения этой динамики в Unity я создал компонент с таким скриптом (Это скрипт из методички, с небольшими изменениями):
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;
    private bool isDataLoaded = false;

    void Start()
    {
        selectAudio = GetComponent<AudioSource>();
        StartCoroutine(GoogleSheets());
    }

    void Update()
    {
        if (!isDataLoaded || statusStart) return;

        string currentKey = "Mon_" + i;
        if (!dataSet.ContainsKey(currentKey)) return;

        float currentValue = dataSet[currentKey];

        if (currentValue >= 30)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(currentValue + " - Проигрывается звук 'Horosho'");
        }
        else if (currentValue >= 20)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(currentValue + " - Проигрывается звук 'Sredne'");
        }
        else // Все что ниже 20
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(currentValue + " - Проигрывается звук 'Ploho'");
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1sONm1NLdqv88rz78jZCw10VEfvIbb1_S334q4q9qT2M/values/Лист1?key=AIzaSyCoh3Ilq7gK_XHmafsjZRYm1gQfK1OL8ho");
        yield return curentResp.SendWebRequest();

        if (curentResp.result != UnityWebRequest.Result.Success)
        {
            Debug.LogError("Ошибка загрузки: " + curentResp.error);
            yield break;
        }

        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);

        if (rawJson == null || !rawJson.HasKey("values"))
        {
            Debug.LogError("Неверный формат данных");
            yield break;
        }

        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            if (selectRow.Count >= 2 && float.TryParse(selectRow[1], out float value))
            {
                dataSet["Mon_" + selectRow[0]] = value;
            }
        }

        isDataLoaded = true;
        Debug.Log("Данные загружены. Всего записей: " + dataSet.Count);
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
- Как видно на изображении ниже, скрипт работает без проблем и в Unity наглядно изображена динамика изменения здоровья
![Скриншот 27-03-2025 224047](https://github.com/user-attachments/assets/45c7ba76-c023-449f-b671-4a3511003994)
# Вывод
Я научилися работать с данными из Google Sheets в Unity, анализировать изменения игровых переменных (здоровье), воспроизводить звуки в зависимости от значений и выявлять логические ошибки в условиях.

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
