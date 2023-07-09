### Домашнее задание #2 Postman
Ссылка на коллекцию [Link>>]()
+ [Запрос #1](https://github.com/ipohaa/postman/tree/main/homework2#запрос-1-1)
+ [Запрос #2](https://github.com/ipohaa/postman/tree/main/homework2#запрос-2)
+ [Запрос #3](https://github.com/ipohaa/postman/tree/main/homework2#запрос-3-1)
+ [Запрос #4](https://github.com/ipohaa/postman/tree/main/homework2#запрос-4-1)
+ [Запрос #5](https://github.com/ipohaa/postman/tree/main/homework2#запрос-5-1)

### Запрос #1
### Запрос
Method: GET  
http://162.55.220.72:5005/first
### Ответ
```
This is the first responce from server!
```
### Тесты
1. Отправить запрос.
2. Статус код 200
```js
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
3. Проверить, что в body приходит правильный string.
```js
pm.test("Body is correct", function () {
    pm.response.to.have.body("This is the first responce from server!");
});
```
## Запрос #2
### Запрос
Method: POST  
http://162.55.220.72:5005/user_info_3
```js
name: string
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
### Тесты
1. Отправить запрос.
2. Статус код 200
```js
pm.test("2. Статус код 200", function () {
    pm.response.to.have.status(200);
});
```
3. Спарсить response body в json.
```js
let responseData = pm.response.json(); // парсинг ответа
```
4. Проверить, что name в ответе равно name s request (name вбить руками.)
```js
pm.test("4. Проверка имени response/request ", function(){
    pm.expect(responseData.name).to.eql("Nikita");
});
```
5. Проверить, что age в ответе равно age s request (age вбить руками.)
```js
pm.test("5. Проверка возраста response/request ", function(){
    pm.expect(+responseData.age).to.eql(19);
});
```
6. Проверить, что salary в ответе равно salary s request (salary вбить руками.)
```js
pm.test("6. Проверка зарплаты response/request ", function(){
    pm.expect(+responseData.salary).to.eql(400);
});
```
7. Спарсить request.
```js
let requestData = request.data; // парсинг запроса
```
8. Проверить, что name в ответе равно name s request (name забрать из request.)
```js
pm.test("8. Проверка имени response/request ", function(){
    pm.expect(responseData.name).to.eql(requestData.name);
});
```
9. Проверить, что age в ответе равно age s request (age забрать из request.)
```js
pm.test("9. Проверка возраста response/request ", function(){
    pm.expect(responseData.age).to.eql(requestData.age);
});
```
10. Проверить, что salary в ответе равно salary s request (salary забрать из request.)
```js
pm.test("10. Проверка зарплаты response/request ", function(){
    pm.expect(responseData.salary).to.eql(+requestData.salary);
});
```
11. Вывести в консоль параметр family из response.
```js
console.log(responseData.family)
```
12. Проверить что u_salary_1_5_year в ответе равно salary*4 (salary забрать из request)
```js
pm.test("12. Проверка умножения", function(){
    pm.expect(responseData.family.u_salary_1_5_year).to.eql(requestData.salary*4);
});
```
## Запрос #3
### Запрос
Method: GET  
http://162.55.220.72:5005/object_info_3
```js
name: string
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
### Тесты
1. Отправить запрос.
2. Статус код 200
```js
pm.test("2. Статус код 200", function () {
    pm.response.to.have.status(200);
});
```
3. Спарсить response body в json
```js
let responseData = pm.response.json(); // парсинг ответа
```
4. Спарсить request.
```js
let requestData = pm.request.url.query.toObject(); // парсинг GET запроса
```
5. Проверить, что name в ответе равно name s request (name забрать из request.)
```js
pm.test("5. Проверка имени response/request ", function(){
    pm.expect(responseData.name).to.eql(requestData.name);
});
```
6. Проверить, что age в ответе равно age s request (age забрать из request.)
```js
pm.test("6. Проверка возраста response/request ", function(){
    pm.expect(responseData.age).to.eql(requestData.age);
});
```
7. Проверить, что salary в ответе равно salary s request (salary забрать из request.)
```js
pm.test("7. Проверка возраста response/request ", function(){
    pm.expect(responseData.salary).to.eql(+requestData.salary);
});
```
8. Вывести в консоль параметр family из response.
```js
console.log(responseData.family)
```
9. Проверить, что у параметра dog есть параметры name.
```js
pm.test("9. Проверка что параметр dog имеет параметр name ", function(){
    pm.expect(responseData.family.pets.dog).to.have.property("name");
});
```
10. Проверить, что у параметра dog есть параметры age.
```js
pm.test("10. Проверка что параметр dog имеет параметр age ", function(){
    pm.expect(responseData.family.pets.dog).to.have.property("age");
});
```
11. Проверить, что параметр name имеет значение Luky.
```js
pm.test("11. Проверка что параметр dog.name имеет параметр Luky ", function(){
    pm.expect(responseData.family.pets.dog.name).to.eql("Luky");
});
```
12. Проверить, что параметр age имеет значение 4.
```js
pm.test("12. Проверка что параметр dog.age имеет параметр 4", function(){
    pm.expect(responseData.family.pets.dog.age).to.eql(4);
});
```
## Запрос #4
### Запрос
Method: GET  
http://162.55.220.72:5005/object_info_4
```js
name: string
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
### Тесты
1. Отправить запрос.
2. Статус код 200
```js
pm.test("2. Статус код 200", function () {
    pm.response.to.have.status(200);
});
```
3. Спарсить response body в json.
```js
let responseData = pm.response.json(); // парсинг ответа
```
4. Спарсить request.
```js
let requestData = pm.request.url.query.toObject(); // парсинг GET запроса
```
5. Проверить, что name в ответе равно name s request (name забрать из request.)
```js
pm.test("5. Проверка имени response/request ", function(){
    pm.expect(responseData.name).to.eql(requestData.name);
});
```
6. Проверить, что age в ответе равно age из request (age забрать из request.)
```js
pm.test("6. Проверка возраста response/request ", function(){
    pm.expect(responseData.age).to.eql(+requestData.age);
});
```
7. Вывести в консоль параметр salary из request.
```js
console.log("requestData.salary ",requestData.salary);
```
8. Вывести в консоль параметр salary из response.
```js
console.log("responseData.salary ",responseData.salary);
```
9. Вывести в консоль 0-й элемент параметра salary из response.
```js
console.log("0 параметр responseData.salary[0]",responseData.salary[0]);
```
10. Вывести в консоль 1-й элемент параметра salary параметр salary из response.
```js
console.log("1 параметр responseData.salary[1]",responseData.salary[1]);
```
11. Вывести в консоль 2-й элемент параметра salary параметр salary из response.
```js
console.log("2 параметр responseData.salary[2]",responseData.salary[2]);
```
12. Проверить, что 0-й элемент параметра salary равен salary из request (salary забрать из request.)
```js
pm.test("12. Проверка зарплаты response/request ", function(){
    pm.expect(responseData.salary[0]).to.eql(+requestData.salary);
});
```
13. Проверить, что 1-й элемент параметра salary равен salary*2 из request (salary забрать из request.)
```js
pm.test("13. Проверка зарплаты response/request ", function(){
    pm.expect(+responseData.salary[1]).to.eql(requestData.salary*2);
});
```
14. Проверить, что 2-й элемент параметра salary равен salary*3 из request (salary забрать из request.)
```js
pm.test("14. Проверка зарплаты response/request ", function(){
    pm.expect(+responseData.salary[2]).to.eql(requestData.salary*3);
});
```
15. Создать в окружении переменную name и передать значение
```js
pm.environment.set("name", requestData.name)
```
16. Создать в окружении переменную age и передать значение
```js
pm.environment.set("age", requestData.age)
```
17. Создать в окружении переменную salary
```js
pm.environment.set("salary", requestData.salary)
```
18. Написать цикл который выведет в консоль по порядку элементы списка из параметра salary.
```js
for (let i in responseData.salary){
    console.log(`Параметр: ${i} значение: ${responseData.salary[i]}`)
}
```
## Запрос #5
### Запрос
Method: POST  
http://162.55.220.72:5005/user_info_2
```js
name: string
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
## Тесты
1. Вставить параметр salary из окружения в request
2. Вставить параметр age из окружения в age
3. Вставить параметр name из окружения в name
4. Отправить запрос.








5. Статус код 200
```js
pm.test("5. Статус код 200", function () {
    pm.response.to.have.status(200);
});
```
6. Спарсить response body в json.
```js
let responseData = pm.response.json();
```
7. Спарсить request.
```js
let requestData = request.data;
```
8. Проверить, что json response имеет параметр start_qa_salary
```js
pm.test("8. Проверка, что response имеет параметр start_qa_salary", function(){
    pm.expect(responseData).to.have.property("start_qa_salary");
});
```
9. Проверить, что json response имеет параметр qa_salary_after_6_months
```js
pm.test("9. Проверка, что response имеет параметр qa_salary_after_6_months", function(){
    pm.expect(responseData).to.have.property("qa_salary_after_6_months");
});
```
10. Проверить, что json response имеет параметр qa_salary_after_12_months
```js
pm.test("10. Проверка, что response имеет параметр qa_salary_after_12_months", function(){
    pm.expect(responseData).to.have.property("qa_salary_after_12_months");
});
```
11. Проверить, что json response имеет параметр qa_salary_after_1.5_year
```js
pm.test("11. Проверка, что response имеет параметр start_qa_salary", function(){
    pm.expect(responseData).to.have.property("qa_salary_after_1.5_year");
});
```
12. Проверить, что json response имеет параметр qa_salary_after_3.5_years
```js
pm.test("12. Проверка, что response имеет параметр qa_salary_after_3_5_years", function(){
    pm.expect(responseData).to.have.property("qa_salary_after_3.5_years");
});
```
13. Проверить, что json response имеет параметр person
```js
pm.test("13. Проверка, что response имеет параметр person", function(){
    pm.expect(responseData).to.have.property("person");
});
```
14. Проверить, что параметр start_qa_salary равен salary из request (salary забрать из request.)
```js
pm.test("14. Проверка, что параметр start_qa_salary равен salary из request", function(){
    pm.expect(+responseData.start_qa_salary).to.eql(+requestData.salary);
});
```
15. Проверить, что параметр qa_salary_after_6_months равен salary*2 из request (salary забрать из request.)
```js
pm.test("15. Проверка, что параметр qa_salary_after_6_months равен salary*2 из request", function(){
    pm.expect(+responseData.qa_salary_after_6_months).to.eql(requestData.salary*2);
});
```
16. Проверить, что параметр qa_salary_after_12_months равен salary*2.7 из request (salary забрать из request.)
```js
pm.test("16. Проверка, что параметр qa_salary_after_12_months равен salary*2.7 из request", function(){
    pm.expect(+responseData.qa_salary_after_12_months).to.eql(requestData.salary*2.7);
});
```
17. Проверить, что параметр qa_salary_after_1.5_year равен salary*3.3 из request (salary забрать из request.)
```js
pm.test("17. Проверка, что параметр qa_salary_after_1.5_year равен salary из request", function(){
    pm.expect(+responseData["qa_salary_after_1.5_year"]).to.eql(requestData.salary*3.3);
});
```
18. Проверить, что параметр qa_salary_after_3.5_years равен salary*3.8 из request (salary забрать из request.)
```js
pm.test("18. Проверка, что параметр qa_salary_after_3.5_years равен salary*3.8 из request", function(){
    pm.expect(+responseData["qa_salary_after_3.5_years"]).to.eql(requestData.salary*3.8);
});
```
19. Проверить, что в параметре person, 1-й элемент из u_name равен salary из request (salary забрать из request.)
```js
pm.test("19. Проверка, что в параметре person, 1-й элемент из u_name равен salary из request", function(){
    pm.expect(+responseData.person.u_name[1]).to.eql(+requestData.salary);
});
```
20. Проверить, что что параметр u_age равен age из request (age забрать из request.)
```js
pm.test("20. Проверка, что что параметр u_age равен age из request", function(){
    pm.expect(+responseData.person.u_age).to.eql(+requestData.age);
});
```
21. Проверить, что параметр u_salary_5_years равен salary*4.2 из request (salary забрать из request.)
```js
pm.test("21. Проверка, что параметр u_salary_5_years равен salary*4.2 из request", function(){
    pm.expect(+responseData.person.u_salary_5_years).to.eql(requestData.salary*4.2);
});
```
22. ***Написать цикл который выведет в консоль по порядку элементы списка из параметра person.
```js
for (let i in responseData.person){
    console.log(`Параметр: ${i} значение: ${responseData.person[i]}`)
}
```

