## Summary

У useExecutableEffect логика идентичная useEffect за исключением того, что
не происходит вызова callback'а если один из параметров в массиве зависимостей имеет
falsy значениe.

## Basic example

```js
import { useEffect, useState } from 'react'

function Component(props) {
  const {
    sourceOneData: { valueOne } // Данные приходят из первого сервиса (GA)
    sourceTwoData: { valueTwo } // Данные приходят из второго сервиса (dadata)
  } = props;

  useExecutableEffect(() => {
    /* it could be setState or dataLayer.push() or something useful */
    doSomethingUseful({ valueOne, valueTwo });
  }, [valueOne, valueTwo]);

  return <>Some JSX</>
}
```

## Motivation: Избежать страшных if'ов, избавится от дублирования if'ов в useEffect

Часто при монтировании компонента, мы не получаем все нужные данные для правильного
вызова useEffect. Из за этого приходится городить страшные if'ы c кучей проверок.
Например получение userId у google analytics.

## Detailed design

```js
import { useEffect, useState } from 'react'

/* Максимально грязная реализация */
function useExecutableEffect(fn, deps) {
	if (deps.map((item) => Boolean(item).includes(false))) {
		return;
	} else {
		fn()
	}

}
```
## Unresolved questions
Продумать нейминг.
Продумать cleanup поведение


