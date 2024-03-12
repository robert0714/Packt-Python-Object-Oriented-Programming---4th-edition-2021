
# Case Study for Chapter 2, Objects in Python

## Logical View
![fig_2.png](./fig_2.png)
![fig_2.png](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robert0714/Packt-Python-Object-Oriented-Programming---4th-edition-2021/main/ch_02/docs/fig_2.uml)
## Samples and Their States

## Sample State Transitions
![fig_3.png](./fig_3.png)
![fig_3.png](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/robert0714/Packt-Python-Object-Oriented-Programming---4th-edition-2021/main/ch_02/docs/fig_3.uml)
```python
>>> from model import Sample
>>> s2 = Sample(
...     sepal_length=5.1, sepal_width=3.5, petal_length=1.4, petal_width=0.2, species="Iris-setosa")
>>> s2
KnownSample(sepal_length=5.1, sepal_width=3.5, petal_length=1.4, petal_width=0.2, species='Iris-setosa')
>>> s2.classification = "wrong"
>>> s2
KnownSample(sepal_length=5.1, sepal_width=3.5, petal_length=1.4, petal_width=0.2, species='Iris-setosa', classification='wrong')

```

## The Hyperparameter Class

```python

>>> class TrainingData:
...     pass
>>> td_1 = TrainingData()

```

```python

>>> from weakref import ref
>>> b = ref(td_1)

```

```python

>>> type(b)
<class 'weakref'>

```

```python

>>> b() == td_1
True

```

```python

>>> del td_1
>>> b() is None
True

```

## Responsibilities

## The Training Data Class

