## Домашнее задание #1 / postman/[homework1](https://github.com/ipohaa/postman/tree/main/homework1)
Ссылка на коллекцию [Link>>](https://github.com/ipohaa/postman/blob/main/homework1/Postman_HW1.postman_collection.json)
+ [Запрос #1](https://github.com/ipohaa/postman/tree/main/homework1#запрос-1)
+ [Запрос #2](https://github.com/ipohaa/postman/tree/main/homework1#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/postman/tree/main/homework1#запрос-3-1)
+ [Запрос #4](https://github.com/ipohaa/postman/tree/main/homework1#запрос-4-1)
+ [Запрос #5](https://github.com/ipohaa/postman/tree/main/homework1#запрос-5-1)
+ [Запрос #6](https://github.com/ipohaa/postman/tree/main/homework1#запрос-6-1)
+ [Запрос #7](https://github.com/ipohaa/postman/tree/main/homework1#запрос-7-1)


## Домашнее задание #2 / postman/[homework2](https://github.com/ipohaa/postman/tree/main/homework2)
Ссылка на коллекцию [Link>>](https://github.com/ipohaa/postman/blob/main/homework2/Postman_HW2.postman_collection.json)
+ [Запрос #1](https://github.com/ipohaa/postman/tree/main/homework2#запрос-1-1)
+ [Запрос #2](https://github.com/ipohaa/postman/tree/main/homework2#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/postman/tree/main/homework2#запрос-3-1)
+ [Запрос #4](https://github.com/ipohaa/postman/tree/main/homework2#запрос-4-1)
+ [Запрос #5](https://github.com/ipohaa/postman/tree/main/homework2#запрос-5-1)

## Домашнее задание #3 / postman/[homework3](https://github.com/ipohaa/postman/tree/main/homework3)
Ссылка на коллекцию [Link>>](https://github.com/ipohaa/postman/blob/main/homework3/Postman_HW3.postman_collection.json)
+ [Итерируемый запрос внутри Postman](https://github.com/ipohaa/postman/tree/main#написать-скрипт-отсылающий-запрос-на-сервер-каждую-итерацию)
+ [Запрос #1](https://github.com/ipohaa/postman/tree/main#запрос-1)
+ [Запрос #2](https://github.com/ipohaa/postman/tree/main#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/postman/tree/main#запрос-3-1)
+ [Запрос #4](https://github.com/ipohaa/postman/tree/main#запрос-4-1)
+ [Запрос #5](https://github.com/ipohaa/postman/tree/main#запрос-5-1)
+ [Запрос #6](https://github.com/ipohaa/postman/tree/main#запрос-6-1)
+ [Запрос #7](https://github.com/ipohaa/postman/tree/main#запрос-7-1)

### Написать скрипт отсылающий запрос на сервер каждую итерацию
### Тесты
1. Получить список валют
2. Итерировать список валют
3. В каждой итерации отправлять запрос на сервер для получения курса каждой валюты
4. Если возвращается 500 код, то переходим к следующей итреации
5. Если получаем 200 код, то проверяем response JSON на наличие поля "Cur_OfficialRate"
6. Если поле есть, то пишем в консоль информацию про фалюту в виде response
```js
{
    "Cur_Abbreviation": str
    "Cur_ID": int,
    "Cur_Name": str,
    "Cur_OfficialRate": float,
    "Cur_Scale": int,
    "Date": str
}
```
7. Переход к следующей итерации
### Код
```js
let responseData = pm.response.json(); //Парсинг ответа
let iterator = +pm.environment.get("iterator") //Берём элемент для цикла из окружения
//console.clear()

if (iterator <= 118){ // если итератор в границах списка делать
    if(pm.response.code === 200){ // если статус код равен 200, то печатаем текущий номер валюты, абревиатуру и текущий курс
        console.log(`Валюта номер ${iterator} -> ${responseData.Cur_Abbreviation}`) 
        console.log(`Курс ${responseData.Cur_Abbreviation} = ${responseData.Cur_OfficialRate}`)
        if(responseData.Cur_OfficialRate > 0){ // если поле есть то выводим полную информацию ответа
            console.log('Полная информация ', responseData)
        }
        iterator++ // увеличиваем итератор 
        pm.environment.set("iterator", iterator) // передаём текущее значение итератора
    } 
    else if (pm.response.code === 500){ // если статус код равен 500, то пропускаем текущий запрос
        iterator++ // увеличиваем итератор 
        pm.environment.set("iterator", iterator) // передаём текущее значение итератора
        console.log('От этого запроса сервак вмер') // консольное сообщение об ошибке
    }
    if (iterator > 118){ // проверка конца списка, если итератор за границами массива, то обнуляем его
        pm.environment.set("iterator", 0);
    }
}

```
