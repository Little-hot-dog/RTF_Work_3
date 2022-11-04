# Сбор, обработка и визуализация тестового набора данных
Отчет по лабораторной работе #2 выполнил(а):
- Сафаргалеев Никита Олегович
- РИ210914
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

## Цель работы
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.
## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
Создаем проект в Unity.
Открываем Anaconda Promt и пишем команды для создания и активации нового ML агента

``` conda create -n MLAgent python=3.6 ```

``` conda activate MLAgent ```

``` pip install mlagents ```

``` pip install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html ```

![](https://github.com/Little-hot-dog/RTF_Work_3/blob/main/1.png)

Далее в консоли переходим в папку с проектом
Создаем объекты плоскости, куба и сферы.
![](https://github.com/Little-hot-dog/RTF_Work_3/blob/main/2.png)

![](https://github.com/Little-hot-dog/RTF_Work_3/blob/main/3.png)

В инспекторе добавялем в нашу сферу RigidBody, C# скрипт
![](https://github.com/Little-hot-dog/RTF_Work_3/blob/main/4.png)

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}

```

Добавялем в корень проекта файл конфигурации нейронной сети, запускаем работу ml агента
После прогона делаем 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запускаем симуляцию сцены и наблюдаем за результатом обучения модели.
![](https://github.com/Little-hot-dog/RTF_Work_3/blob/main/5.png)


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1

```py

```
![]()

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2



## Выводы
При увеличении количества агентов в Unity модель достигает более высоких показателей точности за меньшее время итераций.


## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
