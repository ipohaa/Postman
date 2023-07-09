### Домашнее задание #1 Postman
+ [Запрос #1](https://github.com/ipohaa/postman/tree/main/homework1#запрос-1)
+ [Запрос #2](https://github.com/ipohaa/postman/tree/main/homework1#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/postman/tree/main/homework1#запрос-3-1)
+ [Запрос #4](https://github.com/ipohaa/postman/tree/main/homework1#запрос-4-1)
+ [Запрос #5](https://github.com/ipohaa/postman/tree/main/homework1#запрос-5-1)
+ [Запрос #6](https://github.com/ipohaa/postman/tree/main/homework1#запрос-6-1)
+ [Запрос #7](https://github.com/ipohaa/postman/tree/main/homework1#запрос-7-1)

## Запрос #1
### Запрос
Method: GET  
EndPoint: [/get_method]()  
Request url params:  
```js
name: str
age: int
```
### Ответ
```js
[
    "Nikita",
    "19"
]
```
## Запрос #2
### Запрос
Method: POST  
EndPoint: [/user_info_3]()  
Request form data:  
```
name: str
age: int
salary: int
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
## Запрос #3
### Запрос
Method: GET  
EndPoint: [/object_info_1]()  
Request url params:  
```
name: str
age: int
weight: int
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
## Запрос #4
### Запрос
Method: GET  
EndPoint: [/object_info_2]()  
Request url params:  
```
name: str
age: int
salary: int
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
        "u_salary_5_years": 1680.0
    },
    "qa_salary_after_1.5_year": 1320.0,
    "qa_salary_after_12_months": 1080.0,
    "qa_salary_after_3.5_years": 1520.0,
    "qa_salary_after_6_months": 800,
    "start_qa_salary": 400
}
```
## Запрос #5
### Запрос
Method: GET  
EndPoint: [/object_info_3]()  
Request url params:  
```
name: str
age: int
salary: int
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
        "pets": {
            "cat": {
                "age": 3,
                "name": "Sunny"
            },
            "dog": {
                "age": 4,
                "name": "Luky"
            }
        },
        "u_salary_1_5_year": 1600
    },
    "name": "Nikita",
    "salary": 400
}
```
## Запрос #6
### Запрос
Method: GET  
EndPoint: [/object_info_4]()  
Request url params:  
```
name: str
age: int
salary: int
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
## Запрос #7
### Запрос
Method: POST  
EndPoint: [/user_info_2]()  
Request form data:  
```
name: str
age: int
salary: int
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
        "u_salary_5_years": 1680.0
    },
    "qa_salary_after_1.5_year": 1320.0,
    "qa_salary_after_12_months": 1080.0,
    "qa_salary_after_3.5_years": 1520.0,
    "qa_salary_after_6_months": 800,
    "start_qa_salary": 400
}
```
