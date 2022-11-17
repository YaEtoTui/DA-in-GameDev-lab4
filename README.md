# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил:
- Сажин Егор Алексеевич
- РИ210946
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
Познакомиться с Перцептроном.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления:
 - OR | дать комментарии о корректности работы
 - AND | дать комментарии о корректности работы
 - NAND | дать комментарии о корректности работы
 - XOR | дать комментарии о корректности работы


#### OR
 Данные совпадают с теми, которые мы хотели найти.
 
![OR](https://user-images.githubusercontent.com/102538132/202405188-5ca4daea-c774-487c-8902-b7cdbcd6a1fe.png)


#### AND
 Данные совпадают с теми, которые мы хотели найти.
 
![AND](https://user-images.githubusercontent.com/102538132/202405193-e21898f9-5068-4013-b251-f3db4c1887ef.png)


#### NAND
 Данные совпадают с теми, которые мы хотели найти.
 
![NAND](https://user-images.githubusercontent.com/102538132/202407592-5280f6e5-67d5-4e8e-b6a0-306b5377f616.png)


#### XOR
Если внимательно рассмотрим скриншот снизу, то заметим, что данные, которые мы хотим получить и данные, которые мы получили, сильно отличаются. 

![XOR_1](https://user-images.githubusercontent.com/102538132/202407599-28819409-c4c7-450f-8327-677f5347121a.png)

Попробуем поменять эпохи из 8 на 20. Итог, тот же ответ, что и сверху на скрине. Вывод: XOR нелинейноотделимый, то есть с помощью 1 перцентрона невозможно найти данные.

![XOR_2](https://user-images.githubusercontent.com/102538132/202407612-c6343a65-0f1c-443b-91af-d0ee6e5389cf.png)

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
 
 
Построены графики зависимости количества эпох от ошибки обучения для каждой логической операции(OR, AND, NAND и XOR)
 
 ![graphic](https://user-images.githubusercontent.com/102538132/202430586-ff87ef22-cfb9-47f9-a9a2-8328215ccf50.png)

Вывод: необходимое количество эпох обучения зависит от bias - смещение и weights - веса.


## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.


Визуализацию модели работы перцептрона сделана через вычисления логической операции OR(можно и другие, но на примере я решил сделать через нее), то есть
0 | 0 -> 0, 0 | 1 -> 1, 1 | 0 -> 0, 1 | 1 -> 1
Если результатом перцептрона будет 0, то цилиндр меняет цвет на белый, 1 - черный.
Весь ход мыслей можно увидеть на следующих скринах ниже. Справа есть значения Number_1 и Number_2, они сделаны, чтобы я мог в метод CalcOutput() подставлять значения:
![0_0(1)](https://user-images.githubusercontent.com/102538132/202447632-7dc521ee-ff84-48ea-ace6-30914fa39590.png)
![0_0](https://user-images.githubusercontent.com/102538132/202447639-3203adf9-58cc-479c-9af5-9e50d7c3c35c.png)
![0_1](https://user-images.githubusercontent.com/102538132/202447649-80702653-78d5-484d-9c8f-1b44db18c1cd.png)

Код к 3 заданию:

Perceptron
```py
[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public int number_1;
	public int number_2;

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	public double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		var value = CalcOutput(number_1, number_2);
		ChangeColor.OnSearchValue(value);
		Debug.Log("Test 0 0: " + CalcOutput(number_1, number_2));	
	}
	
```

ChangeColor
```py
public class ChangeColor : MonoBehaviour
{
    static double value = 0;

    public void OnTriggerEnter(Collider other) {
        if (value == 0)
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
        else
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
    }

    public static void OnSearchValue(double answer) {
        value = answer;
    }
}

```

## Выводы
Выполняя эту лабораторную работу, я впервые поработал с Перцептроном: рассмотрел код перцетрона. Произвел вычисления с помощью логических операций. Узнал, что с помощью 1 перцептрона нельхя найти данные через XOR, так как XOR - нелинейноделимая. Научился визуализировать модель работы перцептрона на сцене Unity.


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
