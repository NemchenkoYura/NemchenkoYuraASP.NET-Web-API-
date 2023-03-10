# NemchenkoYuraASP.NET-Web-API-
Немченко Ю.В. ТЗ ASP.NET Web API
Розробити ASP.NET Web API додаток

1.	Додаток повинен мати 2 ендпоінти:
-	/api/v1/check-person POST 
request: 
{ 
 "personName": "str", 
 "episodeName": "str"
}
response:
true - якщо вказаний персонаж був у вказаному епізоді
false - якщо не був
404 not found - якщо або імені персонажа, або назви епізоду не     існує.
-	/api/v1/person?name=person GET
request: 
GET parameter name
response:
{
 	"name": "str",
 "status": "str",
 "species": "str",
"type": "str",
 "gender": "str",
 "origin": {
"name": "str",
               "type": "str",
	"dimension": "str"
 },
}
404 - якщо ім'я персонажа не знайдено
2.	Додаток повинен мати Dockerfile
3.	Джерело даних https://rickandmortyapi.com/documentation

Критерії оцінювання:
●	Коректна робота програми (на запущеному docker контейнері будуть виконані тести)
●	Чистота коду
●	Архітектурні рішення
Додаткові бали:
●	Реалізувати збереження запитів, що повторюються, без звернення до зовнішньої системи.



Рішення Немченко Юрій Володимирович

Для першої кінцевої точки необхідно створити маршрут для обробки POST-запиту і контролер для обробки логіки. POST-запит має приймати два параметри, ім'я персонажа та ім'я епізоду, а контролер має використовувати цю інформацію для запиту джерела даних (Rick and Morty API) для отримання відповідної інформації. Якщо персонаж і епізод знайдено, контролер має повернути true, а якщо ні ім'я персонажа, ні назву епізоду не знайдено, контролер має повернути 404.

Для другої кінцевої точки необхідно створити маршрут для обробки GET-запиту і контролер для обробки логіки. Запит GET повинен приймати параметр name, а контролер повинен використовувати цю інформацію для запиту джерела даних (Rick and Morty API) для отримання відповідної інформації. Потім контролер повинен повернути інформацію про персонажа в зазначеному форматі або видати помилку 404, якщо ім'я персонажа не знайдено.

Для кінцевої точки POST:

[HttpPost]
[Route("/api/v1/check-person")].
public IActionResult CheckPerson([FromBody] CheckPersonRequest request)
{
    if (string.IsNullOrWhiteSpace(request.PersonName) 
        || string.IsNullOrWhiteSpace(request.EpisodeName))
    {
        return BadRequest();
    }
    
    var characters = _rickAndMortyService.GetCharacters();
    var character = characters.FirstOrDefault(c => c.Name == request.PersonName);
    if (character == null)
    {
        return NotFound();
    }
    
    var episodes = _rickAndMortyService.GetEpisodes();
    var episode = episodes.FirstOrDefault(e => e.Name == request.EpisodeName);
    if (episode == null)
    {
        return NotFound();
    }
    
    return Ok(character.Appearances.Contains(episode.Id))
}
Для кінцевої точки GET:

[HttpGet]
[Route("/api/v1/person")]
public IActionResult GetPerson([

База даних
1.	Спроектувати базу даних для бізнесу у сфері авіаперевезень (верхньорівнево на 5-7 таблиць)
2.	Написати запити
●	Список квитків з даними клієнта.
●	Останні 5 проданих квитків
●	Топ 3 клієнтів за частотою польотів


1. Щоб спроектувати базу даних для бізнесу з авіаперевезень, вам потрібно створити 7 таблиць. Ці таблиці повинні включати в себе

Аеропорти: Ця таблиця повинна зберігати інформацію про аеропорти, таку як назва, адреса та контактна інформація.
Рейси: Ця таблиця повинна зберігати інформацію про рейси, таку як дати вильоту і прибуття, номери рейсів і ціни на квитки.
Літаки: Ця таблиця повинна зберігати інформацію про літаки, таку як номер моделі, виробник і кількість місць.
Пілоти: Ця таблиця повинна зберігати інформацію про пілотів, таку як ім'я, номер ліцензії та рейтинг.
Пасажири: Ця таблиця повинна зберігати інформацію про пасажирів, таку як ім'я, контактні дані та вподобання щодо подорожей.
Багаж: Ця таблиця повинна зберігати інформацію про багаж, таку як вага, розмір і тип.
Призначення на рейс: Ця таблиця повинна зберігати інформацію про призначення пілотів на рейси, таку як ім'я пілота, номер рейсу і час вильоту.

2
Щоб вивести список квитків з даними клієнтів,  можна написати запит, який поверне інформацію про клієнта, пов'язану з кожним квитком.
SELECT t.id, t.date, t.price, c.name, c.email FROM tickets t JOIN customers c ON t.customer_id = c.id

Щоб переглянути останні 5 проданих квитків,  можна написати запит, який поверне 5 останніх проданих квитків.
SELECT t.id, t.date, t.price, c.name, c.email FROM tickets t JOIN customers c ON t.customer_id = c.id ORDER BY t.date DESC LIMIT 5

Щоб знайти 3 найкращих клієнтів за частотою польотів, можна написати запит, який поверне 3 найкращих клієнтів на основі кількості рейсів, які вони здійснили.

SELECT c.name, COUNT(*) AS numflights FROM tickets t JOIN customers c ON t.customerid = c.id GROUP BY c.name ORDER BY num_flights DESC LIMIT 3



