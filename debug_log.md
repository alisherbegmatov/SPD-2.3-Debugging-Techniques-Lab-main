# Debug Log

**Explain how you used the techniques covered (Trace Forward, Trace Backward, Divide & Conquer) to uncover the bugs in each exercise. Be specific!**

In your explanations, you may want to answer:

- What is the expected vs. actual output?
- If there is a stack trace, what useful information does it contain?
- Which technique did you use, on which line numbers?
- What assumptions did you have about each line of code, and which ones were proven to be wrong?

_Example: I noticed that the program should show pizza orders once a new order is made and that it wasn't showing any. So, I used the trace forward technique starting on line 13. I discovered the bug on line 27 was caused by a typo of 'pzza' instead of 'pizza'._

_Then I noticed another bug ..._

## Exercise 1

1st bug

Error: "TypeError: 'topping' is an invalid keyword argument for PizzaTopping"
Debugging technique: Trace backward
Solution: Change line 78

```python
for topping_str in ToppingType:
```
to

```python
for topping_str in toppings_list:
```

2nd bug

Error: "TypeError: 'NoneType' object is not iterable"
Debugging technique: Trace backward
Solution: Change line 70

```python
toppings_list = request.form.get('toppings')
```

to

```python
toppings_list = request.form.get('toppings') if request.form.get('toppings') else []
```

3rd bug

Error: "werkzeug.routing.BuildError: Could not build url for endpoint '/'. Did you mean 'fulfill_order' instead?"
Debugging technique: Trace backward
Solution: Change line 85

```python
return redirect(url_for('/'))
```

to 

```python
return redirect(url_for('home'))
```

4th bug

Error: The pizza order is not being displayed.
Debugging technique: Trace forward
Solution: Change lines 67, 68, 70

```python
    order_name = request.form.get('name')
    pizza_size_str = request.form.get('size')
    crust_type_str = request.form.get('crust_type')
    toppings_list = request.form.get('toppings') if request.form.get('toppings') else []
```

to 

```python
    order_name = request.form.get('order_name')
    pizza_size_str = request.form.get('pizza_size')
    crust_type_str = request.form.get('crust_type')
    toppings_list = request.form.getlist('toppings') if request.form.getlist('toppings') else []
```

5th bug

Error: The pizza order is still not being displayed.
Debugging technique: Trace forward
Solution: Add this line after line 81 `db.session.commit()`

6th bug

Error: "sqlalchemy.exc.IntegrityError: (sqlite3.IntegrityError) NOT NULL constraint failed: pizza.size [SQL: INSERT INTO pizza (order_name, size, crust_type, fulfilled) VALUES (?, ?, ?, ?)] [parameters: ('', None, None, 0)] (Background on this error at: http://sqlalche.me/e/13/gkpj)"
Debugging technique: Trace backward
Solution: Add this line after line 70 `if order_name and pizza_size_str and crust_type_str and toppings_list:`

## Exercise 2

1st bug

Error: "KeyError: 'name'"
Debugging technique: Trace backward
Solution: It shows that there is a problem accessing the field in result_json object. We print out the result_json object and it shows that we are not passing the correct parameters.
Solution:  Change Line 38, 39

```python
    city = request.args.get('users_city')
    units = request.args.get('requested_units')
```

to 

```python
    city = request.args.get('city')
    units = request.args.get('units')
```

2nd bug

Error: "KeyError: 'name'"
Debugging technique: Trace backward
Solution: We still could not get the correct result_json. We got a response of `{'cod': '400', 'message': 'Nothing to geocode'}` We checked the Open Weather API documentation and discovered that the param name of the city should be `q`

Change line 44 

`'city' : city`

to

`'q': city`

3rd bug

Error: KeyError: 'temperature'
Debugging technique: Trace backward
Solution: In the result_json object, we received:
`{'coord': {'lon': -122.4194, 'lat': 37.7749}, 'weather': [{'id': 800, 'main': 'Clear', 'description': 'clear sky', 'icon': '01d'}], 'base': 'stations', 'main': {'temp': 17.59, 'feels_like': 17.05, 'temp_min': 14.9, 'temp_max': 20.89, 'pressure': 1025, 'humidity': 63}, 'visibility': 10000, 'wind': {'speed': 0.45, 'deg': 97, 'gust': 1.34}, 'clouds': {'all': 3}, 'dt': 1642716065, 'sys': {'type': 2, 'id': 2017837, 'country': 'US', 'sunrise': 1642692100, 'sunset': 1642727968}, 'timezone': -28800, 'id': 5391959, 'name': 'San Francisco', 'cod': 200}`

It shows that the temperature value is stored as `temp`

Change line 53

`'temp': result_json['main']['temperature']`

to 

`'temp': result_json['main']['temp']`

## Exercise 3

1st bug

Error: "IndexError: list index out of range"
Debugging technique: Trace backward
Solution: Change line 37 

`arr[k] = right_side[i]`

to

`arr[k] = right_side[j]`

2nd bug

Error: The sorted list is expected to be `[9, 7, 6, 5, 4, 3, 2, 1]` but the actual result is `[9, 7, 6, 5, 4, 3, 2, 2]`
Debugging technique: Trace forward
Solution: Add this line to `k += 1` line 35, 40

3rd bug

Error: "TypeError: list indices must be integers or slices, not float"
Debugging technique: Trace backward
Solution: Change line 50

`mid = (high + low) / 2`

to 

`mid = (high + low) // 2`
