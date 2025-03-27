# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР[in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
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
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться моделью нейросети перцептрон и реализовать эту модель в Unity

## Задание 1: в проекте Unity реализовать перцептрон, который умеет производить вычисления:
- OR | дать комментарии о корректности работы
- AND | дать комментарии о корректности работы
- NAND | дать комментарии о корректности работы
- XOR | дать комментарии о корректности работы

| Операция | Результат | Корректность |
| ------ | ------ | ------ |
| OR | ✓ | Полная |
| AND | ✓ | Полная |
| NAND | ✓ | Полная |
| XOR | × | Не работает |

  # Наблюдения:
- Для OR/AND/NAND ошибка снижается до 0 за 5-10 эпох
- Для XOR ошибка не сходится
- Learning rate существенно влияет на скорость обучения

## Задание 2: Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.
- У меня не получилось сделать нормальный график из-за оформления таблицы(другого я не могу придумать)

Ссылка на таблицу: https://docs.google.com/spreadsheets/d/1d5MEp56iUblSWiyF3362KLuLWcUzUj7UiiUi2BYmQBc/edit?gid=0#gid=0

## Задание 3
### Для удобства добавил:
- возможность указывать кол-во эпох, скорость обучения и данные для обучения прямо в инспекторе.
- так же добавил спавн и изменение 4-х кубов для визуализации обучения
- вывод текста с результатами(текст накладывается друг на друга(т.к костыль, который в итоге работает не так как я хотел) и возможно будет не понятно, в конце скринкаст)
### Скрины тестов(20 эпох, на каждый по 5):
![Скриншот 28-03-2025 013732](https://github.com/user-attachments/assets/d0b85196-1206-4f76-9111-0b679291540b)
![Скриншот 28-03-2025 013743](https://github.com/user-attachments/assets/1619806b-1c5b-48bb-9851-9c011d78225b)
![Скриншот 28-03-2025 013749](https://github.com/user-attachments/assets/fee69a8c-ada7-43d5-a2c6-af86f4852105)


```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class Perceptron : MonoBehaviour
{
    [System.Serializable]
    public class TrainingSet
    {
        public double[] input;
        public double output;
    }

    [Header("Настройки обучения")]
    public int epochs = 20;
    public float learningRate = 0.1f;
    public bool realtimeVisualization = true;

    [Header("Данные для обучения")]
    public TrainingSet[] tsOR;
    public TrainingSet[] tsAND;
    public TrainingSet[] tsNAND;
    public TrainingSet[] tsXOR;

    [Header("Визуализация")]
    public GameObject cubePrefab;
    public Transform visualizationParent;

    private double[] weights = { 0, 0 };
    private double bias = 0;
    private List<float> errors = new List<float>();

    void Start()
    {
        InitializeTrainingData();
        CreateVisualization();
        StartCoroutine(RunTests());
    }

    void InitializeTrainingData()
    {
        tsOR = new TrainingSet[]
        {
            new TrainingSet(){input = new double[]{0,0}, output = 0},
            new TrainingSet(){input = new double[]{0,1}, output = 1},
            new TrainingSet(){input = new double[]{1,0}, output = 1},
            new TrainingSet(){input = new double[]{1,1}, output = 1}
        };

        tsAND = new TrainingSet[]
        {
            new TrainingSet(){input = new double[]{0,0}, output = 0},
            new TrainingSet(){input = new double[]{0,1}, output = 0},
            new TrainingSet(){input = new double[]{1,0}, output = 0},
            new TrainingSet(){input = new double[]{1,1}, output = 1}
        };

        tsNAND = new TrainingSet[]
        {
            new TrainingSet(){input = new double[]{0,0}, output = 1},
            new TrainingSet(){input = new double[]{0,1}, output = 1},
            new TrainingSet(){input = new double[]{1,0}, output = 1},
            new TrainingSet(){input = new double[]{1,1}, output = 0}
        };

        tsXOR = new TrainingSet[]
        {
            new TrainingSet(){input = new double[]{0,0}, output = 0},
            new TrainingSet(){input = new double[]{0,1}, output = 1},
            new TrainingSet(){input = new double[]{1,0}, output = 1},
            new TrainingSet(){input = new double[]{1,1}, output = 0}
        };
    }

    IEnumerator RunTests()
    {
        yield return new WaitForSeconds(1);
        TestOperation("OR", tsOR);
        yield return new WaitForSeconds(2);
        TestOperation("AND", tsAND);
        yield return new WaitForSeconds(2);
        TestOperation("NAND", tsNAND);
        yield return new WaitForSeconds(2);
        TestOperation("XOR", tsXOR);
    }

    void TestOperation(string operationName, TrainingSet[] trainingData)
    {
        Debug.Log($"=== Тестирование {operationName} ===");
        StartCoroutine(TrainAndVisualize(operationName, trainingData));
    }

    IEnumerator TrainAndVisualize(string operationName, TrainingSet[] trainingData)
    {
        InitializeWeights();
        errors.Clear();

        for (int e = 0; e < epochs; e++)
        {
            float totalError = 0;

            foreach (var set in trainingData)
            {
                double error = set.output - CalculateOutput(set.input);
                totalError += Mathf.Abs((float)error);

                for (int i = 0; i < weights.Length; i++)
                {
                    weights[i] += learningRate * error * set.input[i];
                }
                bias += learningRate * error;

                if (realtimeVisualization)
                    UpdateVisualization();

                yield return new WaitForSeconds(0.1f);
            }

            errors.Add(totalError);
            Debug.Log($"{operationName} - Эпоха {e + 1}/{epochs}, Ошибка: {totalError}");
        }

        foreach (var set in trainingData)
        {
            double result = CalculateOutput(set.input);
            Debug.Log($"{operationName} {set.input[0]} {set.input[1]} = {result} (ожидалось: {set.output})");
        }
    }

    void InitializeWeights()
    {
        for (int i = 0; i < weights.Length; i++)
        {
            weights[i] = Random.Range(-1.0f, 1.0f);
        }
        bias = Random.Range(-1.0f, 1.0f);
    }

    double CalculateOutput(double[] inputs)
    {
        double sum = bias;
        for (int i = 0; i < weights.Length; i++)
        {
            sum += weights[i] * inputs[i];
        }
        return sum > 0 ? 1 : 0;
    }

    void CreateVisualization()
    {
        foreach (Transform child in visualizationParent)
        {
            Destroy(child.gameObject);
        }

        for (int x = 0; x <= 1; x++)
        {
            for (int y = 0; y <= 1; y++)
            {
                GameObject cube = Instantiate(
                    cubePrefab,
                    new Vector3(x * 2, 0, y * 2),
                    Quaternion.identity,
                    visualizationParent
                );
                cube.name = $"Cube_{x}_{y}";
                UpdateCubeVisual(cube, new double[] { x, y });
            }
        }
    }

    void UpdateVisualization()
    {
        foreach (Transform child in visualizationParent)
        {
            string[] parts = child.name.Split('_');
            if (parts.Length == 3)
            {
                double x = double.Parse(parts[1]);
                double y = double.Parse(parts[2]);
                UpdateCubeVisual(child.gameObject, new double[] { x, y });
            }
        }
    }

    void UpdateCubeVisual(GameObject cube, double[] inputs)
    {
        double result = CalculateOutput(inputs);
        cube.GetComponent<Renderer>().material.color = result > 0.5 ? Color.green : Color.red;
        cube.GetComponentInChildren<TMP_Text>().text = $"{inputs[0]},{inputs[1]} → {result}";
    }
}
```
## Скринкаст обучения в реальном времени
https://drive.google.com/file/d/1tceBOpSKiF9u__En07MXIV4q5Y9hYDz6/view?usp=drive_link
### Система отображает работу перцептрона в реальном времени с помощью 4 кубов, каждый из которых соответствует одной из входных комбинаций:

- (0,0) – левый нижний куб
- (0,1) – левый верхний
- (1,0) – правый нижний
- (1,1) – правый верхний

#### Каждый куб меняет цвет в зависимости от предсказания перцептрона.
