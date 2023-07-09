### Домашнее задание #3 Postman
Ссылка на коллекцию [Link>>]()
+ [Итерируемый запрос внутри Postman](https://github.com/ipohaa/Postman1/tree/main#написать-скрипт-отсылающий-запрос-на-сервер-каждую-итерацию)
+ [Запрос #1](https://github.com/ipohaa/Postman1/tree/main#запрос-1)
+ [Запрос #2](https://github.com/ipohaa/Postman1/tree/main#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/Postman1/tree/main#запрос-3)
+ [Запрос #4](https://github.com/ipohaa/Postman1/tree/main#запрос-4)
+ [Запрос #5](https://github.com/ipohaa/Postman1/tree/main#запрос-5)
+ [Запрос #6](https://github.com/ipohaa/Postman1/tree/main#запрос-6)
+ [Запрос #7](https://github.com/ipohaa/Postman1/tree/main#запрос-7)

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
## Запрос #1
### Запрос
Method: POST  
http://162.55.220.72:5005/login
```js
login : string
password : string
```
### Ответ
```js
{
    "token": "/s34lfgbj/None/jjd909/23861kjkWpqc1823None161782evny"
}
```
### Тесты
Приходящий токен необходимо передать во все остальные запросы.
```js
// парсинг ответа
let auth = pm.response.json()
// создание переменной окружения и запись токена
pm.environment.set("auth_token", auth.token);
```

## Запрос #2
### Запрос
Method: POST  
http://162.55.220.72:5005/user_info

```js
age: int
salary: int
name: str
auth_token
```
### Ответ
```js
{
    "person": {
        "u_age": 19,
        "u_name": [
            "Nikita",
            400,
            19
        ],
        "u_salary_1_5_year": 1600
    },
    "qa_salary_after_12_months": 1160.0,
    "qa_salary_after_6_months": 800,
    "start_qa_salary": 400
}
```
### Тесты
1) Статус код 200  
2) Проверка структуры JSON в ответе  
3) В ответе указаны коэффициенты умножения salary, написать тесты по проверке правильности результата перемножения
4) Достать значение из поля 'u_salary_1_5_year' и передать в поле salary запроса http://162.55.220.72:5005/get_test_user

```js
let requestData = JSON.parse(request.data); // парсинг запроса
let responseData = pm.response.json(); // парсинг ответа
let schema = { // создание схемы правильного ответа
    "required": [
        "start_qa_salary",
        "qa_salary_after_6_months",
        "qa_salary_after_12_months",
        "person"
    ],
    "properties":{
        "start_qa_salary": {
            "type": "integer"
        },
        "qa_salary_after_6_months": {
            "type": "integer"
        },
        "qa_salary_after_12_months": {
            "type": "integer"
        },
        "person": {
            "type": "object",
            "required": [
                "u_name",
                "u_age",
                "u_salary_1_5_year"
            ],
            "properties": {
                "u_name": {
                    "type": "array",
                    "items": {
                        "anyOf": [
                                {
                                "type":"string"
                                },
                                {
                                "type": "integer"
                                }
                        ]
                    }
                },
                "u_age":{
                    "type": "integer"
                },
                "u_salary_1_5_year":{
                    "type": "integer"
                }
            }
        }
    }
};


pm.test("1. Статус код 200", function () {
    pm.response.to.have.status(200);
});


pm.test('2. Проверка структуры JSON в ответе', function () {
    pm.response.to.have.jsonSchema(schema)
});


pm.test("3. Проверка увеличения коэффициента ", function(){
    pm.expect(responseData.qa_salary_after_12_months).to.eql(requestData.salary*2.9);
    pm.expect(responseData.qa_salary_after_6_months).to.eql(requestData.salary*2);
    pm.expect(responseData.person.u_salary_1_5_year).to.eql(requestData.salary*4);
});

pm.environment.set("salary_1_5_year", responseData.person.u_salary_1_5_year);


```

## Запрос #3
### Запрос
Method: POST  
http://162.55.220.72:5005/new_data
```js
age: int
salary: int
name: str
auth_token
```
### Ответ
```js
{
    "age": 19,
    "name": "Nikita",
    "salary": [
        400,
        "800",
        "1200"
    ]
}
```
### Тесты
1) Статус код 200
2) Проверка структуры json в ответе.
3) В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения.
4) проверить, что 2-й элемент массива salary больше 1-го и 0-го
```js
let responseData = pm.response.json(); // парсинг ответа
let requestData = request.data; // парсинг запроса
let schema = { // схема правильного ответа
    "required": [
        "age",
        "name",
        "salary"
    ],
    "properties":{
        "age":{
            "type":"integer",
        },
        "name":{
            "type":"string",
        },
        "salary":{
            "type":"array",
            "items":{
                "anyOf": [
                    {
                        "type":"string"
                    },
                    {
                        "type": "integer"
                    }
                ]
            }
        }

    }
}

pm.test("1. Статус код 200", function () {
    pm.response.to.have.status(200);
});

pm.test("2. Проверка структуры JSON в ответе", function () {
    pm.response.to.have.jsonSchema(schema)
});

pm.test("3. Проверка умножения на коэффициент", function(){
    pm.expect(responseData.salary[0]).to.eql(requestData.salary*1);
    pm.expect(+responseData.salary[1]).to.eql(requestData.salary*2);
    pm.expect(+responseData.salary[2]).to.eql(requestData.salary*3);
});

pm.test("4. Проверка, что 2-й элемент массива salary больше 1-го и 0-го", function () {
    pm.expect(+responseData.salary[2]).to.be.above(+responseData.salary[1]);
    pm.expect(+responseData.salary[2]).to.be.above(responseData.salary[0]);
});
```
## Запрос #4
### Запрос
Method: POST  
http://162.55.220.72:5005/test_pet_info
```js
age: int
weight: int
name: str
auth_token
```
### Ответ
```js
{
    "age": 7,
    "daily_food": 0.24,
    "daily_sleep": 50.0,
    "name": "Ron"
}
```

### Тесты
1) Статус код 200
2) Проверка структуры json в ответе.
3) В ответе указаны коэффициенты умножения weight, напишите тесты по проверке правильности результата перемножения.
```js
let responseData = pm.response.json(); // парсинг ответа
let requestData = request.data; // парсинг запроса
let schema = { // схема правильного ответа
    "required": [
        "age",
        "name",
        "daily_food",
        "daily_sleep"
    ],
    "properties":{
        "age":{
            "type":"integer",
        },
        "daily_food":{
            "type":"number",
        },
        "daily_sleep":{
            "type":"number",
        },
        "name":{
            "type":"string",
        }        
    }
}

console.log(typeof(responseData.daily_food))

pm.test("1. Статус код 200", function () {
    pm.response.to.have.status(200);
});

pm.test('2. Проверка структуры JSON в ответе', function () {
    pm.response.to.have.jsonSchema(schema)
});

pm.test("3. Проверка умножения на коэффициент", function(){
    let resultFood = requestData.weight*0.012
    let resultSleep = requestData.weight*2.5
    pm.expect(responseData.daily_food).to.eql(+resultFood.toFixed(2));
    pm.expect(responseData.daily_sleep).to.eql(resultSleep);
});
```
## Запрос #5
### Запрос
Method: POST  
http://162.55.220.72:5005/get_test_user
```js
age: int
salary: int
name: str
auth_token
```
### Ответ
```js
{
    "age": "19",
    "family": {
        "children": [
            [
                "Alex",
                24
            ],
            [
                "Kate",
                12
            ]
        ],
        "u_salary_1_5_year": 1600
    },
    "name": "Nikita",
    "salary": 400
}
```
### Тесты
1) Статус код 200
2) Проверка структуры json в ответе.
3) Проверить что занчение поля name = значению переменной name из окружения
4) Проверить что занчение поля age в ответе соответсвует отправленному в запросе значению поля age

```js
let responseData = pm.response.json(); // парсинг ответа
let requestData = request.data; // парсинг запроса
let schema = { //схема правильного ответа
    "required": [
        "age",
        "name",
        "family",
        "salary"
    ],
    "properties":{
        "age":{
            "type":"string",
        },
        "name":{
            "type":"string",
        },
        "family":{
            "type":"object",
            "required":[
                "children"
            ],
            "properties":{
                "children":{
                    "type":"array",
                    "items":[
                        {
                            "type":"array",
                            "items":[
                                {
                                    "type":"string"
                                },
                                {
                                    "type": "integer"
                                }
                            ]
                        }
                    ]
                }
            }
        }
    }
}

pm.test("1. Статус код 200", function () {
    pm.response.to.have.status(200);
});

pm.test('2. Проверка структуры JSON в ответе', function () {
    pm.response.to.have.jsonSchema(schema)
});

pm.test("3. Проверить, что значение response name = значению environment name из окружения ", function(){
    pm.expect(responseData.name).to.eql(pm.environment.get("name"));
});

pm.test('4. Проверить, что значение response age соответсвует значению request age', function () {
    pm.expect(responseData.name).to.eql(requestData.name);
});

```
## Запрос #6
### Запрос
Method: POST  
http://162.55.220.72:5005/currency
```js
auth_token
```
### Ответ
Передаётся список массив объектов
```js
{
    "Cur_Abbreviation": string,
    "Cur_ID": 0,
    "Cur_Name": string
}
...
{
    "Cur_Abbreviation": string,
    "Cur_ID": 120,
    "Cur_Name": string
}
```
### Тесты
Взять любой объект из присланного списка, используя js random и передать через окружение в следующий запрос.
```js
let random_number = Math.floor(Math.random() * (118)) + 1
pm.environment.set("random_number", random_number)
console.log(random_number)
```


## Запрос #7
### Запрос
Method: POST  
http://162.55.220.72:5005/curr_byn
```js
auth_token
curr_code: int
```
### Ответ
```js
{
    "Cur_Abbreviation": "BHD",
    "Cur_ID": 13,
    "Cur_Name": "Bahraini dinars",
    "Cur_OfficialRate": 5.30444291332015,
    "Cur_Scale": 48,
    "Date": "2023-07-09"
}
```
### Тесты
1) Статус код 200
2) Проверка структуры json в ответе.
```js
let schema = { // схема правильного ответа
    "required": [
        "Cur_Abbreviation",
        "Cur_ID",
        "Cur_Name",
        "Cur_OfficialRate",
        "Cur_Scale",
        "Date"
    ],
    "properties":{
        "Cur_Abbreviation":{
            "type":"string"
        },
        "Cur_ID":{
            "type":"integer"
        },
        "Cur_Name":{
            "type":"string"
        },
        "Cur_OfficialRate":{
            "type":"number"
        },
        "Cur_Scale":{
            "type":"integer"
        },
        "Date":{
            "type":"string"
        }
    }
}

pm.test("1. Статус код 200", function () {
    pm.response.to.have.status(200);
});

pm.test("2. Проверка структуры JSON в ответе", function () {
    pm.response.to.have.jsonSchema(schema)
});
```

