# <a name="up" />QA Engineer Portfolio

This portfolio contains projects completed during the [QA Engineer](https://praktikum.yandex.ru/qa-engineer) training program at Yandex.Practicum.

## Test Design
- Test Analysis | Mind Maps | Flowcharts  
- Test Design | Equivalence Classes | Boundary Values  

## Functional Testing
- Manual Testing | Web Applications | Mobile Applications  
- UI/UX Testing | Cross-Browser Testing | Cross-Platform Testing  

## API Testing
- Postman | REST API | JSON & XML  
- API Request Analysis | Response Validation | Charles Proxy  

## Database Testing
- SQL Queries | Data Validation | Backend Testing  

## Bug Reporting
- Bug Reports | Jira | Test Cases & Checklists  

## Projects
- **Yandex Scooter:** Tested rental and delivery functionality via mobile and web applications. Logged over 40 bug reports, 80% of which were successfully fixed.
- **Yandex Routes:** Conducted route planning and pricing validation. Implemented test design using equivalence classes and boundary values. Worked with XML and JSON for backend verification.

## Additional Skills
- HTML | DevTools | Android Studio  
- Functional & Regression Testing  
- Analytical Thinking & Attention to Detail  

---

### Task 1

**1. Visualize the requirements**

Analyze the requirements for the Yandex.Routes service and draw a mindmap.

**2. Identify equivalence classes and boundary values**

Check: "Trip start time", "From where", "To Where".

- Select the test objects.
- Define the equivalence classes. Specify the type of restrictions: a range or a set of values.
- Define the boundary values of each class, if applicable.
- Select the test values that will test each class; and the boundaries, if any.

Don't forget to check out negative scenarios.

<details>
<summary>Requirements for the Yandex.Routes service</summary>

***

Yandex.Routes is a service that builds routes for various types of transport. Calculates the time and cost of the trip.

**Interface**

The interface has the fields "Trip start time", "From where", and "To where". Switches for route modes: optimal, fast and your own, as well as switches for modes of transport: own car, carsharing, taxi, scooter, bicycle and on foot.

The user enters the departure time. To set a route, you need to enter the street and house number in the "From" and "To" fields. There may be spaces at the beginning and end of the address.: they are acceptable, but when you remove the focus, the system will delete them.

**Description of the interface operation**

In the starting state, the fields "Trip start time", "From where" and "To where" are empty. The route modes "Optimal", "Fast" and "Own" are not selected; the panel for switching modes of transport is inactive.

**The logic of the "From" and "To" fields**

If the address fields are filled in correctly, points A and B are displayed on the map. If the "From" field is filled in incorrectly, point A is not displayed. If the "Where To" field is filled in incorrectly, point B is not displayed. If the value is incorrect, the field is highlighted in red; an error message appears.

There are examples of test addresses in the table.

** "Optimal" and "Fast" mode**

If you select the "Optimal" or "Fast" mode, the system will automatically assign the type of transport; the route will be set; the time and cost of the trip will be displayed. You cannot select a transport in these modes — the Transport modes panel is inactive.

**Own mode**

If you select the "Own" mode, the transport modes panel is active — you can switch it. A route is set for each type of transport; the time and cost of the trip are calculated.

If you change the mode of transport or change the value in any field, the route will be rebuilt; the time and cost of the trip will be recalculated.

**Restrictions**

System Elements | Requirements
:----------------|:-----------
The clock input field | 24 hour format. Zeros before a single digit number are required. Only integers from 0 to 23 inclusive are valid. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
The minutes input field | Integers only. Zeros before a single digit number are required. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
Address input field | Only Russian letters, numbers, space, dash, dot, comma. The maximum length is 50 characters. The spaces before and after the address are removed when the focus is removed. If you enter incorrectly, the error "You entered an incorrect address" is highlighted in red.
Mode switches | Optimal, Fast and Custom. The status of each switch is active, selected.
Switches between modes of transport: Walking, scooter, bicycle, carsharing, taxi, own car. The status of each switch is active, inactive, selected.

**Layouts**

![Interface](https://code.s3.yandex.net/qa/schemes/project-interface-fast.png)

![Error Messages](https://code.s3.yandex.net/qa/schemes/project-interface-error.png)

**Calculation logic**

The system receives data about the start of the trip, point A and point B. After that, it calculates the duration and cost of the trip using a specific algorithm.

![Calculation logic](https://code.s3.yandex.net/qa/schemes/project-logics-2.png)

The distance, speed, and cost per minute or kilometer can be obtained from the tables. This data is enough to calculate the time and cost of the trip for each type of transport.

Type of transport | How to calculate the time | Cost per km
:-------|:-----------|:----------
On foot | Average speed 4 km/h | 0 r / km
Scooter sharing | Average speed | 10 km/h | 5.5 r / km
Bike sharing | Average speed | 12 km/h | 3 r / km
Carsharing | see The table "Average car speed" | 9 r / min
Taxi | see The table "Average taxi speed" | 11 r / min
Own a car | see The table "Average car speed" | 20 r / km

Average car speed

Time of day | Average car speed
:----------|:----------|
|00\:01-08:00 | 45 km/h
|08\:01-12:00 | 30 km/h
|12\:01-18:00 | 40 km/h
|18\:01-22:00 | 25 km/h
|22\:01-00:00 | 45 km/h

The average speed of a taxi, taking into account traffic in the designated lanes

Time of day | Average taxi speed
:----------|:----------|
|00\:01-08:00 | 50 km/h
|08\:01-12:00 | 35 km/h
|12\:01-18:00 | 42 km/h
|18\:01-22:00 | 30 km/h
|22\:01-00:00 | 50 km/h

Matrix of distances between addresses for highways, in kilometers

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 1,4 | 1,5 | 0,89 | 2,6 | 2,6 | 2,6
Komsomolsky Prospekt, 18 | 1,4 | 0 | 2,9 | 2,3 | 2,3 | 2,3 | 2,3
Zubovsky Boulevard, 37 | 1,4 | 1,5 | 0 | 1,9 | 3,8 | 3 | 3,3
M. Pirogovskaya, 25 | 1,5 | 3 | 2,4 | 0 | 1,2 | 3,4 | 2,3
Khamovnicheskiy Val, 34 | 1,5 | 3,7 | 3,7 | 1,2 | 0 | 1,7 | 1,7
Frunzenskaya embankment, 46 | 3,2 | 3,9 | 4,7 | 2,7 | 1,7 | 0 | 2,2
3rd Frunzenskaya Street, 12 | 1,4 | 2,4 | 3,5 | 2,3 | 1,4 | 1,3 | 0

The matrix of distances between pedestrian addresses, in kilometers

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 0,96 | 1,4 | 0,91 | 1,4 | 1,7 | 1,1
Komsomolsky Prospekt, 18 | 1 | 0 | 1,3 | 1,9 | 2 | 1,7 | 1,2
Zubovsky Boulevard, 37 | 1,4 | 1,3 | 0 | 1,9 | 2,7 | 2,7 | 2,3
M. Pirogovskaya, 25 | 0,91 | 1,9 | 1,9 | 0 | 0,75 | 1,5 | 1,2
Khamovnicheskiy Val, 34 | 1,4 | 2 | 2,7 | 0,75 | 0 | 1,4 | 1,2
Frunzenskaya embankment, 46 | 1,7 | 1,7 | 2,7 | 1,5 | 1,4 | 0 | 0,57
3rd Frunzenskaya Street, 12 | 1,1 | 1,2 | 2,3 | 1,2 | 1,2 | 0,57 | 0

Please note: to calculate the time and cost of the route, tables with the speed of different modes of transport are available to you. They show the speed of the car at different times of the day. If you take such test values that the trip covers several time intervals, the algorithm selects the speed of the car from the range in which the trip started.

![Time intervals](https://code.s3.yandex.net/qa/schemes/time-intervals.png)

***

</details>

### Solution

**1. The associative map**

![Mindmap](https://i.ibb.co/gJyd1M2/image.png)

[High-resolution associative map](https://ibb.co/m4J0sCj )

**2. Equivalence classes and boundary values**

<details>
<summary>Clock input field</summary>

***

Field length

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:------------------------------|:------------------------|
|2 digits |range | 2 | | 2 Numbers: *12* |
|from 0 to 1 digits|range | 0, 1 | 0 digits: *empty field*           | 1 digit: 0 |
|from 3 digits to +∞ | range | 3 | 50 digits:<br>1234567890123456789012345<br>6789012345678901234567890 | 3 digits: 222 |

Mandatory filling

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:------------------------------|:------------------------|
|Field is filled in |range |      |                        | *~~12~~*     |
|Field is empty |range |      |          | *~~Empty field~~*|

Input values

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:------------------------------|:------------------------|
|Digits from 00 to 23 |range | 00, 23 | *~~12~~* | 00, 01, 22, 23 |
|from -∞ to -01|range | | | |
|from 24 to +∞| range | 24 | 99 | 24 , 25 |
|letter input | set | | five | |
|entering special characters| set | | ~!№~ |  |

*Repeated checks are highlighted in italics, ~~crossed out~~ values - optimization of checks

***

</details>

<details>
<summary>The minutes input field</summary>

***

Field length

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:------------|:---------|:------|:---------------------|:------------------|
|2 digits |range | 2 | | 2 Numbers: *30* |
|from 0 to 1 digits|range | 0, 1 | 0 digits: *empty field*           | 1 digit: 0 |
|from 3 digits to +∞ | range | 3 | 50 digits:<br>1234567890123456789012345<br>6789012345678901234567890 | 3 digits: 123, 4 digits: 1234 |

Mandatory filling

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:-------------------|:------------------|
|Field is filled in |range |      |                        | *~~30~~*     |
|Field is empty |range |      |          | *~~Empty field~~*|

Input values

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:------------------------------|:------------------------|
|Digits from 00 to 59 |range | 00, 59 | *~~30~~* | 00, 01, 58, 59 |
|from -∞ to -01|range | | | |
|from 24 to +∞| range | 60 | 99 | 60 , 61 |
|letter input | set | | five | |
|entering special characters| set | | ~!№~ |  |
*Repeated checks are highlighted in italics, ~~crossed out~~ values - optimization of checks

***

</details>

<details>
<summary>Address input fields (From, To)</summary>

***

Field length

|Class name|Class type|Borders|Test data inside the class|Test data at the borders|
|:--------------|:---------|:------|:------------------------------|:------------------------|
|from 1 to 50 characters |range | 1, 50 | 25 characters: "*12 3rd Frunzenskaya Street*" | 1 character: "U"|
| | | | | 1 symbol: "Y"|
| | | | | 2 Symbol: "Us"|
| | | | | 49 Symbols: "3rd Frunzenskaya Street, 123rd Frunzenskaya Street, 1"|
| | | | | 50 Symbols: "3rd Frunzenskaya Street, 123rd Frunzenskaya Street"|
|from -∞ to 0|range | 0 | | 0 characters: *empty string* |
|from 51 to +∞| range | 51 | 100 characters: "3rd Frunzenskaya Street, 123rd Frunzenskaya Street, 123rd Frunzenskaya Street, 123rd Frunzenskaya Street" | 51 characters: "3rd Frunzenskaya Street, 123rd Frunzenskaya Street, 123" |
| | | | | 52 Symbol: "3rd Frunzenskaya Street, 123rd Frunzenskaya Street, 1234"|

Mandatory filling

|Class name|Class type|Borders|Test data inside the class|Test data at borders|
|:--------------|:---------|:------|:-------------------|:------------------|
|The field is filled in |set | | | "*~~3rd Frunzenskaya Street, 12~~*" |
|Field is empty| set | | | *~~Empty string~~*|

Input values

|Class name|Class type|Borders|Test data inside the class|
|:--------------|:---------|:------|:-------------------|
|String containing Russian letters |set | Correct | "Usacheva"|    
|String containing numbers |set | Correct | "Usacheva3"|
|A line containing a space in the middle of the text|typing | Correct | "46 Frunzenskaya embankment"|
|Line containing a space at the beginning of the text | set | Correct | "Usacheva 3"|
|Line containing a space at the end of the text |set | Correct | "Usacheva 3"|
|Comma-separated line |set | Correct | "Komsomolsky Prospekt, 18"|
|A line containing a dash |set | Correct | "*~~3rd Frunzenskaya Street, 12~~*"|
|A string containing a dot |set | Correct | "M. Pirogovskaya, 25"|
|String containing Latin letters |set |Incorrect | "Usacheva"|
|String containing characters from other languages |set |Incorrect | "通""|
|String containing special characters |set | Incorrect | "%;$#?&*!"|              

*Repeated checks are highlighted in italics, ~~crossed out~~ values - optimization of checks

***

</details>

[Up](#up)

### Task 2

**1. Design time and cost calculation tests**

Choose one type of transport to test: your own car, carsharing or taxi.

- Determine which requirements describe the logic of calculating the time and cost of the selected transport.
- Write a formula that calculates the time and cost of the trip.
- Draw a flowchart that visualizes the algorithm for choosing the speed of transport depending on the start time of the trip.
- Define the equivalence classes. And the boundary values of each class to which it applies.
- Select the test values that will test each class; and the boundaries, if any.
- Write test cases based on the test values. Test cases should check the correctness of the calculation logic.

**2. Write the conclusions**

Reflect on the results of the first sprint:
- What part of the functionality do you think was covered by the tests?
- What new skills do you think allowed you to complete the project?

### Solution

> Choose one type of transport to test: your own car, carsharing or taxi.

Your car.

> Determine which requirements describe the logic of calculating the time and cost of the selected transport.

Fields "Start of trip", "From where", "To where"<br>
Mode switch ("Own")<br>
Mode of transport switch ("Own car")<br>
Average car speed depending on time of day<br>
Cost per km<br>
Distance between addresses for highways

> Write a formula that calculates the time and cost of the trip.

Travel time = distance / speed * 60<br>
Trip cost = distance * cost of 1 km

> Draw a flowchart that visualizes the algorithm for choosing the speed of transport depending on the start time of the trip.

![Block diagram](https://i.ibb.co/CPN6wv2/blockscheme.jpg)

[High-resolution flowchart](https://i.ibb.co/BndGfjN/blockscheme.jpg)

According to the requirements, if the trip covers several time intervals, the algorithm selects the speed of the car from the range in which the trip started. Since the end time of the trip, which depends on the distance between the addresses, is insignificant for the algorithm for selecting the speed of transport, the input of addresses From and To, as well as the calculation of the time and cost of the trip, was not added to the flowchart.

> Define the equivalence classes. And the boundary values of each class to which it applies. Select the test values that will test each class; and the boundaries, if any.

Start time of the trip

|Class name|Class type|Borders|Test data inside the class|Test data at borders|Explanation|
|:--------------|:---------|:------|:-------------------|:-------------------|:-------------------|
|00\:01-08:00 | range | 00\:01, 08\:00 | 04\:00 | 00\:01, 00\:02, 07\:59, 08\:00 | Car speed 45 km/h|
|08\:01-12:00 | range | 08\:01, 12\:00 | 10\:00 | 08\:01, 08\:02, 11\:59, 12\:00 | Car speed is 30 km/h|
|12\:01-18:00 | range | 12\:01, 18\:00 | 15\:00 | 12\:01, 12\:02, 17\:59, 18\:00 | The car's speed is 40 km/h|
|18\:01-22:00 | range | 18\:01, 22\:00 | 20\:00 | 18\:01, 18\:02, 21\:59, 22\:00 | Car speed 25 km/h|
|22\:01-00:00 | range | 22\:01, 00\:00 | 23\:00 | 22\:01, 22\:02, 23\:59, 00\:00 | Car speed 45 km/h|

> Write test cases based on the test values. Test cases should check the correctness of the calculation logic.

Calculating the time and cost of a trip by car:

**Input data**:<br>
Trip start time (25 test values)<br>
&nbsp;&nbsp;&nbsp;Point A (From where): Usacheva St., 3<br>
&nbsp;&nbsp;&nbsp;Point B (Where): Zubovsky Boulevard, 37

**Calculation**:<br>
&nbsp;&nbsp;&nbsp;Travel time, formula: distance (1.5 km) / average car speed * 60<br>
The cost of the trip, formula: distance (1.5 km) * the cost of 1 km (20 rubles)

**Output data**:<br>
&nbsp;&nbsp;&nbsp;Travel time<br>
&nbsp;&nbsp;&nbsp;The cost of the trip

**Test cases**

<details>
<summary>ID-001: Calculation of the time and cost of a trip by car (the trip starts at 04:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 04:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-002: Calculation of the time and cost of driving your car (the trip starts at 00:01)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 00:01.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-003: Calculation of the time and cost of driving your car (the trip starts at 00:02)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 00:02.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-004: Calculation of the time and cost of a trip by car (the trip starts at 07:59)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 07:59.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-005: Calculation of the time and cost of a trip by car (the trip starts at 08:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 08:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-006: Calculation of the time and cost of a trip by car (the trip starts at 10:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 10:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-007: Calculation of the time and cost of a trip by car (the trip starts at 08:01)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 08:01.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-008: Calculation of the time and cost of a trip by car (the trip starts at 08:02)</summary>

*** 

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 08:02.
2. Enter the address in the "From" field: 3 Usacheva St.
3. Enter the address in the "Where to" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-009: Calculation of the time and cost of a trip by car (the trip starts at 11:59)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 11:59.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-010: Calculation of the time and cost of a trip by car (the trip starts at 12:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 12:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-011: Calculation of the time and cost of a trip by car (the trip starts at 15:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 15:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min 15 sec.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-012: Calculation of the time and cost of a trip by car (the trip starts at 12:01)</summary>

*** 

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 12:01.
2. Enter the address in the "From" field: 3 Usacheva St.
3. Enter the address in the "Where" field: 37 Zubovsky Boulevard.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min 15 sec.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-013: Calculation of the time and cost of a trip by car (the trip starts at 12:02)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 12:02.
2. Enter the address in the "From" field: 3 Usacheva St.
3. Enter the address in the "Where to" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min 15 sec.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-014: Calculation of the time and cost of a trip by car (the trip starts at 17:59)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 17:59.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min 15 sec.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-015: Calculation of the time and cost of a trip by car (the trip starts at 18:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 18:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min 15 sec.
- The cost of the trip: 30 rubles.
-Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-016: Calculation of the time and cost of a trip by car (the trip starts at 20:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 20:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min 36 s
. - The cost of the trip: 30 rub.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-017: Calculation of the time and cost of a trip by car (the trip starts at 18:01)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 18:01.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min 36 s
. - The cost of the trip: 30 rub.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-018: Calculation of the time and cost of a trip by car (the trip starts at 18:02)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 18:02.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min 36 sec.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-019: Calculation of the time and cost of a trip by car (the trip starts at 21:59)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 21:59.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min 36 s
. - The cost of the trip: 30 rub.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-020: Calculation of the time and cost of a trip by car (the trip starts at 22:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 22:00.
2. Enter the address in the "From" field: 3 Usacheva St.
3. Enter the address in the "Where" field: 37 Zubovsky Boulevard.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 3 min 36 s
. - The cost of the trip: 30 rub.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-021: Calculation of the time and cost of a trip by car (the trip starts at 23:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 23:00.
2. Enter the address in the "From" field: 3 Usacheva St.
3. Enter the address in the "Where to" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-022: Calculation of the time and cost of a trip by car (the trip starts at 22:01)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 22:01.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-023: Calculation of the time and cost of a trip by car (the trip starts at 22:02)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 22:02.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-024: Calculation of the time and cost of a trip by car (the trip starts at 23:59)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 23:59.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

<details>
<summary>ID-025: Calculation of the time and cost of a trip by car (the trip starts at 00:00)</summary>

***

**Precondition**:
1. Open the Yandex Routes service page.

**Steps**:
1. Enter the start time of the trip: 00:00.
2. Enter the address in the "From" field: Usacheva St., 3.
3. Enter the address in the "Where" field: Zubovsky Boulevard, 37.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The following data is displayed in the result panel:
- Travel time: 2 min.
- The cost of the trip: 30 rubles.
- Action button

**Environment**: Google Chrome version 72 or higher, Yandex.Browser version 18.1.0 or higher

***

</details>

> What part of the functionality do you think was covered by the tests?

In fact, the tests cover only part of the main functionality – the logic of calculating the time and cost of driving your own car. Data has also been prepared for creating test cases that verify the input of time and address fields.

All that remains is to prepare the data and write test cases to test the logic of calculating the time/cost of other modes of transport and modes of transportation: carsharing, taxi, bicycle, scooter, on foot; mode switches and modes of transport; displaying data on the map; zooming the map; and other interface elements: caps, basements.

> What skills do you think allowed you to complete the project?

Test analysis skills: requirements analysis and decomposition, compilation of associative maps and flowcharts.

Test design skills: identification of test objects and elements, equivalence classes, selection of test and boundary values, optimization of checks.

Test documentation writing skills: creating checklists and test cases.

[Up](#up)

## <a name="web-testing" />Testing Web applications

### Task

**1. Requirements analysis**

Review the updated requirements for the Yandex.Routes service. The team managed to make the first version of the product.

**2. Test documentation for the layout**

- Analyze the layout requirements and identify the test objects: These are all visual elements on the layout that need to be checked for their appearance and correct location.
- Write a checklist to test the layout. Don't forget to check: compliance with layouts (visually — not "pixel to pixel"); how the result of calculating the cost and time of the route is displayed with long values; spelling.

Write a checklist in the "Checklist and test results: testing layout and interface logic" table.

**3. Test documentation for interface logic**

Analyze the interface logic requirements and create test documentation.

- Write test cases to display the result of calculating the cost and time for all types of transport. To create tests to display the results, use the "Decision Table" technique. In testing, use only the addresses from the table in the "Requirements" lesson.

Important: the developers have not yet fully implemented the calculation of the cost of the trip on the backend, so the logic of calculating the amount and time may be incorrect. You don't need to test it.

- Create checklists that will help you test: drawing and hiding points on the map; drawing and hiding a route on the map; zooming the map; moving the map. There is a limited list of addresses in the requirements — you can test only on them.

Complete the checklist from task 3. Form the test cases in the table "Test cases: interface logic".

**4. Testing and setting up bug reports**

Test the [Yandex.Routes] service(https://qa-routes.praktikum-services.ru /).

You already have a set of configurations where you need to test the service. But there is not much time left for testing, so test the service on a limited set: in your current operating system in Yandex Browser at 800x600 screen resolution, and in Firefox at 1280x720.

To check long lines in the interface, use DevTools — you can replace HTML in it; or Fiddler/Charles — you can change the response from the server in them.

During the testing process, mark the test results: PASSED or FAILED. If the test has the FAILED status, create a bug report in Yandex.Place the tracker in the BUG queue and enter the ID in the corresponding results table.

Layout testing should be done in both environments. Interface logic tests can be done in only one resolution. In the report, set the status N/A (Not Applicable, not applicable) next to this test.


<details>
<summary>Requirements 2.0</summary>

***

Yandex.Routes is a service that builds routes for various types of transport. Calculates the time and cost of the trip.

**Where the service is supported**

Supported operating systems: Win 7/8/10, macOS 10.14/10.15.

Supported browsers: Chrome, Yandex, Firefox, operating system browsers (Edge / IE, Safari) latest and penultimate versions.

Screen resolutions: 800x600, 1280x720, 1920 x 1080.

**Interface**

The interface has the fields "Trip start time", "From where", and "To where". Switches for route modes: optimal, fast or your own, as well as switches for modes of transport: own car, carsharing, taxi, scooter, bicycle or on foot.

The user enters the departure time. To set a route, you need to enter the start of the route, the street and the house number in the "From" and "To" fields. There may be spaces at the beginning and end of the address.: they are acceptable, but when you remove the focus, the system will delete them.

**Description of the interface operation**

In the starting state, the fields "Trip start time", "From where" and "To where" are empty. The route modes "Optimal", "Fast" and "Own" are not selected; the panel for switching modes of transport is inactive.

**The logic of the "From" and "To" fields**

If the address fields are filled in correctly, points A and B are displayed on the map. If the "From" field is filled in incorrectly, point A is not displayed. If the "Where To" field is filled in incorrectly, point B is not displayed. If the value is incorrect, the field is highlighted in red; an error message appears.

There are examples of test addresses in the table.

** "Optimal" and "Fast" mode**

If you select the "Optimal" or "Fast" mode, the system will automatically assign the type of transport; the route will be set; the time and cost of the trip will be displayed. You cannot select a transport in these modes — the Transport modes panel is inactive.

**Own mode**

If you select the "Own" mode, the transport modes panel is active — you can switch it. A route is set for each type of transport; the time and cost of the trip are calculated.

If you change the mode of transport or change the value in any field, the route will be rebuilt; the time and cost of the trip will be recalculated.

**Restrictions**

System Elements | Requirements
:----------------|:-----------
The clock input field | 24 hour format. Zeros before a single digit number are required. Only integers from 0 to 23 inclusive are valid. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
The minutes input field | Integers only. Zeros before a single digit number are required. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
Address input field | Only Russian letters, numbers, space, dash, dot, comma. The maximum length is 50 characters. The spaces before and after the address are removed when the focus is removed. If you enter incorrectly, the error "You entered an incorrect address" is highlighted in red.
Mode switches | Optimal, Fast and Custom. The status of each switch is active, selected.
Switches between modes of transport: Walking, scooter, bicycle, carsharing, taxi, own car. The status of each switch is active, inactive, selected.

**The map**

The map can be moved by holding down the mouse button or using the touchpad. You can zoom the map using the "+" and "-" buttons, as well as the slider on the map, mouse scroll, or touchpad.

**Route on the map**

The route can only be set using the form on the left. Routes only work "in reading mode" — you cannot move or set waypoints on the map.

**Layouts**

![Interface](https://code.s3.yandex.net/qa/schemes/project-interface-fast.png)

![Error Messages](https://code.s3.yandex.net/qa/schemes/project-interface-error.png)

**Calculation logic**

The system receives data about the start of the trip, point A and point B. After that, it calculates the duration and cost of the trip using a specific algorithm.

![Calculation logic](https://code.s3.yandex.net/qa/schemes/project-logics-2.png)

The distance, speed, and cost per minute or kilometer can be obtained from the tables. This data is enough to calculate the time and cost of the trip for each type of transport.

Type of transport | How to calculate the time | Cost per km
:-------|:-----------|:----------
On foot | Average speed 4 km/h | 0 r / km
Scooter sharing | Average speed | 10 km/h | 5.5 r / km
Bike sharing | Average speed | 12 km/h | 3 r / km
Carsharing | see The table "Average car speed" | 9 r / min
Taxi | see The table "Average taxi speed" | 11 r / min
Own a car | see The table "Average car speed" | 20 r / km

Average car speed

Time of day | Average car speed
:----------|:----------|
|00\:01-08:00 | 45 km/h
|08\:01-12:00 | 30 km/h
|12\:01-18:00 | 40 km/h
|18\:01-22:00 | 25 km/h
|22\:01-00:00 | 45 km/h

The average speed of a taxi, taking into account traffic in the designated lanes

Time of day | Average taxi speed
:----------|:----------|
|00\:01-08:00 | 50 km/h
|08\:01-12:00 | 35 km/h
|12\:01-18:00 | 42 km/h
|18\:01-22:00 | 30 km/h
|22\:01-00:00 | 50 km/h

The matrix of distances between addresses for highways, in kilometers. In testing, use only the addresses from the table.

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 1,4 | 1,5 | 0,89 | 2,6 | 2,6 | 2,6
Komsomolsky Prospekt, 18 | 1,4 | 0 | 2,9 | 2,3 | 2,3 | 2,3 | 2,3
Zubovsky Boulevard, 37 | 1,4 | 1,5 | 0 | 1,9 | 3,8 | 3 | 3,3
M. Pirogovskaya, 25 | 1,5 | 3 | 2,4 | 0 | 1,2 | 3,4 | 2,3
Khamovnicheskiy Val, 34 | 1,5 | 3,7 | 3,7 | 1,2 | 0 | 1,7 | 1,7
Frunzenskaya embankment, 46 | 3,2 | 3,9 | 4,7 | 2,7 | 1,7 | 0 | 2,2
3rd Frunzenskaya Street, 12 | 1,4 | 2,4 | 3,5 | 2,3 | 1,4 | 1,3 | 0

A matrix of distances between pedestrian addresses, in kilometers. In testing, use only the addresses from the table.

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 0,96 | 1,4 | 0,91 | 1,4 | 1,7 | 1,1
Komsomolsky Prospekt, 18 | 1 | 0 | 1,3 | 1,9 | 2 | 1,7 | 1,2
Zubovsky Boulevard, 37 | 1,4 | 1,3 | 0 | 1,9 | 2,7 | 2,7 | 2,3
M. Pirogovskaya, 25 | 0,91 | 1,9 | 1,9 | 0 | 0,75 | 1,5 | 1,2
Khamovnicheskiy Val, 34 | 1,4 | 2 | 2,7 | 0,75 | 0 | 1,4 | 1,2
Frunzenskaya embankment, 46 | 1,7 | 1,7 | 2,7 | 1,5 | 1,4 | 0 | 0,57
3rd Frunzenskaya Street, 12 | 1,1 | 1,2 | 2,3 | 1,2 | 1,2 | 0,57 | 0

Please note: to calculate the time and cost of the route, tables with the speed of different modes of transport are available to you. They show the speed of the car at different times of the day. If you take such test values that the trip covers several time intervals, the algorithm selects the speed of the car from the range in which the trip started.

![Time intervals](https://code.s3.yandex.net/qa/schemes/time-intervals.png)

***

</details>

### Solution


<summary>Requirements 2.0</summary>

***

Yandex.Routes is a service that builds routes for various types of transport. Calculates the time and cost of the trip.

**Where the service is supported**

Supported operating systems: Win 7/8/10, macOS 10.14/10.15.

Supported browsers: Chrome, Yandex, Firefox, operating system browsers (Edge / IE, Safari) latest and penultimate versions.

Screen resolutions: 800x600, 1280x720, 1920 x 1080.

**Interface**

The interface has the fields "Trip start time", "From where", and "To where". Switches for route modes: optimal, fast or your own, as well as switches for modes of transport: own car, carsharing, taxi, scooter, bicycle or on foot.

The user enters the departure time. To set a route, you need to enter the start of the route, the street and the house number in the "From" and "To" fields. There may be spaces at the beginning and end of the address.: they are acceptable, but when you remove the focus, the system will delete them.

**Description of the interface operation**

In the starting state, the fields "Trip start time", "From where" and "To where" are empty. The route modes "Optimal", "Fast" and "Own" are not selected; the panel for switching modes of transport is inactive.

**The logic of the "From" and "To" fields**

If the address fields are filled in correctly, points A and B are displayed on the map. If the "From" field is filled in incorrectly, point A is not displayed. If the "Where To" field is filled in incorrectly, point B is not displayed. If the value is incorrect, the field is highlighted in red; an error message appears.

There are examples of test addresses in the table.

** "Optimal" and "Fast" mode**

If you select the "Optimal" or "Fast" mode, the system will automatically assign the type of transport; the route will be set; the time and cost of the trip will be displayed. You cannot select a transport in these modes — the Transport modes panel is inactive.

**Own mode**

If you select the "Own" mode, the transport modes panel is active — you can switch it. A route is set for each type of transport; the time and cost of the trip are calculated.

If you change the mode of transport or change the value in any field, the route will be rebuilt; the time and cost of the trip will be recalculated.

**Restrictions**

System Elements | Requirements
:----------------|:-----------
The clock input field | 24 hour format. Zeros before a single digit number are required. Only integers from 0 to 23 inclusive are valid. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
The minutes input field | Integers only. Zeros before a single digit number are required. If the input is incorrect, the error "You entered the wrong time" is highlighted in red.
Address input field | Only Russian letters, numbers, space, dash, dot, comma. The maximum length is 50 characters. The spaces before and after the address are removed when the focus is removed. If you enter incorrectly, the error "You entered an incorrect address" is highlighted in red.
Mode switches | Optimal, Fast and Custom. The status of each switch is active, selected.
Switches of modes of transport: On foot, scooter, bicycle, carsharing, taxi, own car. The status of each switch is active, inactive, selected.

**The map**

The map can be moved by holding down the mouse button or using the touchpad. You can zoom the map using the "+" and "-" buttons, as well as the slider on the map, mouse scroll, or touchpad.

**Route on the map**

The route can only be set using the form on the left. Routes only work "in reading mode" — you cannot move or set waypoints on the map.

**Layouts**

![Interface](https://code.s3 .yandex.net/qa/schemes/project-interface-fast.png)

![Error Messages](https://code.s3 .yandex.net/qa/schemes/project-interface-error.png)

**Calculation logic**

The system receives data about the start of the trip, point A and point B. After that, it calculates the duration and cost of the trip using a specific algorithm.

![Calculation logic](https://code.s3 .yandex.net/qa/schemes/project-logics-2.png)

The distance, speed, and cost per minute or kilometer can be obtained from the tables. This data is enough to calculate the time and cost of the trip for each type of transport.

Type of transport | How to calculate the time | Cost per km
:-------|:-----------|:----------
On foot | Average speed 4 km/h | 0 r / km
Scooter sharing | Average speed | 10 km/h | 5.5 r / km
Bike sharing | Average speed | 12 km/h | 3 r / km
Carsharing | see The table "Average car speed" | 9 r / min
Taxi | see The table "Average taxi speed" | 11 r / min
Own a car | see The table "Average car speed" | 20 r / km

Average car speed

Time of day | Average car speed
:----------|:----------|
|00\:01-08:00 | 45 km/h
|08\:01-12:00 | 30 km/h
|12\:01-18:00 | 40 km/h
|18\:01-22:00 | 25 km/h
|22\:01-00:00 | 45 km/h

The average speed of a taxi, taking into account traffic in the designated lanes

Time of day | Average taxi speed
:----------|:----------|
|00\:01-08:00 | 50 km/h
|08\:01-12:00 | 35 km/h
|12\:01-18:00 | 42 km/h
|18\:01-22:00 | 30 km/h
|22\:01-00:00 | 50 km/h

The matrix of distances between addresses for highways, in kilometers. In testing, use only the addresses from the table.

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 1,4 | 1,5 | 0,89 | 2,6 | 2,6 | 2,6
Komsomolsky Prospekt, 18 | 1,4 | 0 | 2,9 | 2,3 | 2,3 | 2,3 | 2,3
Zubovsky Boulevard, 37 | 1,4 | 1,5 | 0 | 1,9 | 3,8 | 3 | 3,3
M. Pirogovskaya, 25 | 1,5 | 3 | 2,4 | 0 | 1,2 | 3,4 | 2,3
Khamovnicheskiy Val, 34 | 1,5 | 3,7 | 3,7 | 1,2 | 0 | 1,7 | 1,7
Frunzenskaya embankment, 46 | 3,2 | 3,9 | 4,7 | 2,7 | 1,7 | 0 | 2,2
3rd Frunzenskaya Street, 12 | 1,4 | 2,4 | 3,5 | 2,3 | 1,4 | 1,3 | 0

A matrix of distances between pedestrian addresses, in kilometers. In testing, use only the addresses from the table.

Address | Usacheva, 3 | Komsomolsky Prospekt, 18 | Zubovsky Boulevard, 37 | Pirogovskaya metro station, 25 | Khamovnicheskiy Val, 34 | Frunzenskaya embankment, 46 | 3rd Frunzenskaya Street, 12
:---|:---|:---|:---|:---|:---|:---|:---|
Usacheva, 3 | 0 | 0,96 | 1,4 | 0,91 | 1,4 | 1,7 | 1,1
Komsomolsky Prospekt, 18 | 1 | 0 | 1,3 | 1,9 | 2 | 1,7 | 1,2
Zubovsky Boulevard, 37 | 1,4 | 1,3 | 0 | 1,9 | 2,7 | 2,7 | 2,3
M. Pirogovskaya, 25 | 0,91 | 1,9 | 1,9 | 0 | 0,75 | 1,5 | 1,2
Khamovnicheskiy Val, 34 | 1,4 | 2 | 2,7 | 0,75 | 0 | 1,4 | 1,2
Frunzenskaya embankment, 46 | 1,7 | 1,7 | 2,7 | 1,5 | 1,4 | 0 | 0,57
3rd Frunzenskaya Street, 12 | 1,1 | 1,2 | 2,3 | 1,2 | 1,2 | 0,57 | 0

Please note: to calculate the time and cost of the route, tables with the speed of different modes of transport are available to you. They show the speed of the car at different times of the day. If you take such test values that the trip covers several time intervals, the algorithm selects the speed of the car from the range in which the trip started.

![Time intervals](https://code.s3.yandex.net/qa/schemes/time-intervals.png)

***

</details>

### Solution

**1. Requirements analysis**

> What has changed in the requirements?

The following information has been added:
- Supported OS, browsers, and screen resolutions;
- ways to move and scale;
- plotting a route on the map: it can only be set using the form on the left, you cannot move or set waypoints on the map. 

> Who is the user?

A person who needs to move from one point of the city to another.

> What tasks and problems does it solve?

Gets information about how to get from one point of the city to another: what kind of transport, how long it will take, and how much it will cost.

> What is the typical user behavior?

The user enters the input data – the start time and the addresses of the beginning and end of the route, selects the mode of travel and mode of transport (where possible) and receives the output data – information about the duration and cost of the trip on the selected transport.

> What does he use?

Computer/laptop running Windows 7/8/10 or macOS 10.14/10.15, Chrome, Yandex, Firefox, Edge/IE or Safari latest and penultimate versions. Computer screen resolutions: 800x600, 1280x720 or 1920 x 1080.

**2. Test documentation for the layout**

Checklist and test results: testing the layout and interface logic

| № | Description of the check | macOS 10.15.2, Yandex 20.3.0, 800x600| macOS 10.15.2, Firefox 75.0, 1280x720 | Bug report ID |
|:--:|:-----------|:-----:|:-------:|:-------:|
|1|After loading the page, there are no errors and warnings in the console| FAILED| FAILED| [BUG-8316](#BUG-8316), [BUG-8317](#BUG-8317)|
|2| The site's favicon is displayed on the browser tab| FAILED| FAILED| [BUG-8245](#BUG-8245)|
|3| The page title is displayed correctly on the tab and in the browser address bar| FAILED |FAILED| [BUG-8243](#BUG-8243)|
|4| The site header is placed according to the layout and is fully displayed; the words "Yandex" and "Routes" in the header are links, clicking on them opens the pages of the corresponding services| FAILED| FAILED| [BUG-8251](#BUG-8251)|
|5| The time input fields are placed according to the layout and are fully displayed| PASSED| PASSED| -|
|6| The address input fields are placed according to the layout and are fully displayed| PASSED |PASSED| -|
|7| The icons to the left of the address fields are placed according to the layout, fully displayed and match in color with the corresponding route points on the map |FAILED |FAILED| [BUG-8331](#BUG-8331)|
|8| The route mode panel is placed according to the layout and is fully displayed |PASSED| PASSED |-|
|9| The transport modes panel is placed according to the layout and is fully displayed| FAILED |FAILED| [BUG-8299](#BUG-8299)|
|10| The result panel for calculating the time and cost of the trip is placed according to the layout and is fully displayed |FAILED |FAILED |[BUG-8351](#BUG-8351), [BUG-8368](#BUG-8368)|
|11| In the starting state, all input fields are empty, route modes are not selected, and the modes of transport panel is inactive| PASSED|PASSED| -|
|12| The fonts of the form match the layout| FAILED| FAILED| [BUG-8373](#BUG-8373)|
|13| All texts do not contain spelling errors| FAILED |FAILED| [BUG-8285](#BUG-8285)|
|14| The map is placed according to the layout and is fully displayed| PASSED| PASSED| -|
|15| The link "Open in Yandex.Maps" is placed according to the layout, it is fully displayed; when clicked, the Yandex page opens.Maps in the new tab| PASSED| PASSED| -|
|16| The Yandex copyright and the "Terms of Use" link are placed according to the layout, fully displayed; clicking on the link in a new tab opens the page of the terms of use of the Yandex.Maps service| PASSED |PASSED |-|
|17| The layout does not break when entering long values in the time and address fields and in the result panel| PASSED| PASSED| -|
|18| Error messages are placed according to the layout and are fully displayed| PASSED| FAILED |[BUG-8371](#BUG-8371)|
|19| When entering incorrect values in the time fields and moving the focus, a prompt error is displayed: "You entered an incorrect time"| PASSED|PASSED| -|
|20| When entering the correct values in the address fields, points A and B are displayed on the map; if the values are incorrect, the points are not displayed| PASSED| PASSED| -|
|21| When entering an incorrect value in the From field and moving the focus, a prompt error is displayed: "You entered an incorrect address"| FAILED|FAILED| [BUG-8370](#BUG-8370)|
|22| When you enter an incorrect value in the Where field and move the focus, a prompt error is displayed: "You entered an incorrect address" |PASSED| PASSED| -|
|23| When selecting the "Optimal" or "Fast" modes, the transport modes panel is inactive |PASSED| PASSED| -|
|24| When selecting the "Own" mode, the transport modes panel becomes active |PASSED| PASSED| -|
|25| When switching modes of transport in the "Own" mode, the route is correctly set for each transport, the time and cost of the trip are calculated| FAILED| FAILED| [BUG-8355](#BUG-8355)|
|26| In the "Own" mode, when changing the mode of transport or values in any field, the route is rebuilt, the time and cost of the trip are recalculated| FAILED |FAILED |[BUG-8367](#BUG-8367)|
|27| Tooltips: the correct label appears for each element; it should not overlap its element|FAILED |FAILED| [BUG-8314](#BUG-8314)|
|28| Context menu: highlighted when the cursor is hovered over; navigating to the specified location or performing an action when selecting a menu item| PASSED| FAILED| [BUG-8384](#BUG-8384)|
|29| Input fields: the form fields must be filled in; the ability to edit the entered data; the name of the fields corresponds to their logic; the format of the input fields meets the requirements| FAILED| FAILED| [BUG-8369](#BUG-8369), [BUG-8372](#BUG-8372)|
|30| Scrollbars: there are none if the content fits on the page; you can move along the scrollbar with the mouse, using the "Page up" and "Page down", "Home" and "End" buttons; all scrollbars of the form must be the same size and color| PASSED |FAILED |[BUG-8393](#BUG-8393)|
|31| Buttons (Optimal, Fast, Own, icons of modes of transport, action button in the result panel): the name of the button corresponds to its logical design, when pressed, the appearance of the button should change; pressing the button causes one action; pressing it again does not cause a repeat action; buttons that are not available for pressing are not hidden, but "dried"; the mouse cursor does not change when hovering over inactive buttons; the space between the buttons does not cause any actions| FAILED| FAILED| [BUG-8351](#BUG-8351), [BUG-8397](#BUG-8397)|
|32| Links: clicking on a link triggers the action described in the requirements; when you hover the cursor over the link, its appearance changes, as does the cursor; links are the same in design| PASSED| PASSED| -|
|33| Placeholders: all input fields contain placeholders; the placeholder disappears if you bring focus to the field or start entering data; the placeholder appears if you clear the field or remove focus from it; the placeholder in the field differs in appearance from the data that the user enters; all placeholders of the form look the same| FAILED| FAILED |[BUG-8374](#BUG-8374), [BUG-8395](#BUG-8395)|
|34| There are no unnecessary elements on the service page |FAILED| FAILED| [BUG-8254](#BUG-8254)|
|35| The map is scaled using the "+" and "-" buttons, the slider on the map, the mouse scroll and the touchpad| PASSED| N/A| -|
|36| The map is moved using the mouse and touchpad |PASSED| N/A| -|
|37| Address points and route are displayed on the map when correct values are entered in the From and To fields; when values are deleted or incorrect, points and route are hidden| FAILED| N/A| [BUG-8334](#BUG-8334), [BUG-8382](#BUG-8382)|

**3. Test cases: interface logic**

<details>
<summary>ID-1: Display of the result panel for calculating the cost and time of the trip in the "Optimal" mode</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Optimal" mode.

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- <Name of the type of transport> ~ <cost> rub.
- On the way <time> min.
- A blue action button with an inscription corresponding to the type of transport; for example, for a taxi: "Call a taxi"

On the right side of the panel:
- Picture of the type of transport

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-2: Display of the result panel for calculating the cost and time of the trip in the "Fast" mode</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Fast" mode.

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- <Name of the type of transport> ~ <cost> rub.
- On the way <time> min.
- A blue action button with an inscription corresponding to the type of transport; for example, for a taxi: "Call a taxi"

On the right side of the panel:
- Picture of the type of transport

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-3: Display of the result panel for calculating the cost and time of a trip in the "Own" mode for carsharing</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Choose the type of transport "Carsharing".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Carsharing ~ <cost> rub.
- On the way <time> min.
- Blue action button with an inscription corresponding to the type of transport.

On the right side of the panel:
- Carsharing picture

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug Report ID**: [BUG-8351](#BUG-8351), [BUG-8368](#BUG-8368)

***

</details>

<details>
<summary>ID-4: Display of the result panel for calculating the cost and travel time in the pedestrian's Own mode</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Choose the type of transport "On foot".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Pedestrian Is Free.
- On the way <time> min.
- Blue action button with an inscription corresponding to the type of transport.

On the right side of the panel:
- Picture of a pedestrian

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-5: Display of the result panel for calculating the cost and time of a trip in the "Own" mode for a taxi</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Select the type of transport "Taxi".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Taxi ~ <cost> rub.
- On the way <time> min.
- The action button is blue with the inscription "Call a taxi".

On the right side of the panel:
- Taxi picture

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-6: Displays the result panel for calculating the cost and time of a ride in the "Own" mode for a bicycle</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Select the type of transport "Bicycle".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Bicycle ~ <cost> rub.
- On the way <time> min.
- The action button is blue with the inscription "Call a taxi".

On the right side of the panel:
- Bike picture

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-7: Displaying the result panel for calculating the cost and time of a ride in the "Own" mode for a scooter</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Select the type of transport "Scooter".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Scooter ~ <cost> rub.
- On the way <time> min.
- The action button is blue with the inscription "Call a taxi".

On the right side of the panel:
- Picture of the scooter

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

<details>
<summary>ID-8: Displays the panel of the result of calculating the cost and time of the trip in the "Own" mode for your car</summary>

***

**Precondition**:
1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Steps**:
1. Enter the start time of the trip: 12:00.
2. In the "From Where" field, enter the address: Usacheva, 3.
3. In the "Where" field, enter the address: Frunzenskaya Embankment, 46.
4. Select the "Own" mode.
5. Select the type of transport "Your car".

**OR**: The calculation results panel displays information about the trip.

On the left side of the panel:
- Car ~ <cost> rub.
- On the way <time> min.
- The action button is blue with the inscription "Call a taxi".

On the right side of the panel:
- Picture of the car

**Environment**: Yandex.Browser at least 20.0, resolution 800x600; Firefox at least 75.0, resolution 1280x720

**Result**: FAILED

**Bug report ID**: [BUG-8351](#BUG-8351)

***

</details>

**4-5. Testing and setting up bug reports. Summing up the results**

The team tested the layout and logic of the interface. We checked the compliance of the layout with the layout and the implementation of the frontend: the operation of the input fields, mode selection panels and transport, as well as the display of the calculation of the time and cost of the trip in the result panel and the points and lines of the route on the map.

There is not a single blocker among the errors found and there are only two critical bugs. However, there are quite a few errors with low and low priority that relate to the usability of the service and its image. The team recommends correcting errors before handing the product over to users.

In total, we found 23 bugs and made one recommendation to improve the service. Here is a list of **bug reports**:

<a name="BUG-8245" />
<details>
<summary>BUG-8245: The favicon of Yandex.Routes is not displayed in the browser tab</summary>

***

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

Expected result: the favicon is displayed in the browser tab of the open service page.

Actual result: The favicon is not displayed.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/CyFZgF25EuAA6g ]

**Priority**: minor

***

</details>

<a name="BUG-8243" />
<details>
<summary>BUG-8243: Incorrect title of the "Yandex.Routes" page on the popup hint in the browser tab</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Move the mouse cursor over the browser tab.

Expected result: a tooltip appears with the page title: "Yandex.Practicum - Yandex.Routes"

The actual result: a tooltip appears with an incorrect page title: "Yandex.Practicum - Yandex.Routesundefined"

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/d/l7mbS4BL6OPS3A ]

**Priority**: minor

***

</details>

<a name="BUG-8251" />
<details>
<summary>BUG-8251: The words "Yandex" and "Routes" in the header are not links</summary>

***

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

Expected result: the name of the service in the header should consist of two links: the "Yandex" link leads to the Yandex home page, and the "Routes" link leads to the Yandex.Routes service page.

The actual result: the words "Yandex" and "Routes" in the header are not links.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/7LEp6uxb9xar8Q ]

**Priority**: low

***

</details>

</details>

<a name="BUG-8254" />
<details>
<summary>BUG-8254: There is an extra element in the header of the Yandex.Routes page</summary>

***

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

Expected result: only the name of the service is displayed in the header - "Yandex Routes".

The actual result: the words "Training simulator" are displayed in the header to the right of the service name.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/8dZ-azY9_FG0zw ]

**Priority**: minor

***

</details>

<a name="BUG-8285" />
<details>
<summary>BUG-8285: Spelling error in the name of time fields</summary>

***

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

Expected result: all texts do not contain spelling errors.

The actual result is a spelling error in the name of the time fields.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/eLGKnSkcSZXmRQ ]

**Priority**: minor

***

</details>

<a name="BUG-8299" />
<details>
<summary>BUG-8299: Identical icons for carsharing and your car in the transport panel</summary>

***

**Description:**
For the convenience of using the service, the transport mode switch panel should display unique icons for each type of transport. The layout does not display the icon for your car correctly; the layout displays the same icons for carsharing and your car.

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

Expected result: the panel for switching modes of transport displays unique icons for each type of transport.

The actual result: the same icons for carsharing and your car are displayed.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/W-hhJYw7x98uQw ]

**Priority**: low

***

</details>

<a name="BUG-8314" />
<details>
<summary>BUG-8314: Form elements don't have tooltips</summary>

***

**Precondition:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Hover the mouse cursor over the form elements for which popup hints should be provided: input fields, mode buttons, icons on the transport mode selection panel.

Expected result: The elements have tooltips.

The actual result: the elements have no tooltips.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screencast**: [https://yadi.sk/i/CGF0Hgf3MMmz-Q ]

**Priority**: low

***

</details>

<a name="BUG-8316" />
<details>
<summary>BUG-8316: After loading the page, warnings about incorrectly configured cookies appear in the console</summary>

***

**Playback Steps:**

1. Clear the browser cache and cookies.
2. Open Yandex.Routes:[https://qa-routes.praktikum-services.ru].
3. Open DevTools and go to the Console tab.

Expected result: There are no error messages or warnings in the console.

The actual result: there are warnings in the console about incorrectly configured cookies.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

**Screenshot**: [https://yadi.sk/i/2eg7_QcaS9Qpsw ]

**Priority**: minor

***

</details>

<a name="BUG-8317" />
<details>
<summary>BUG-8317: After loading the page, a warning appears in the console about using a non-standard property</summary>

***

**Playback Steps:**

1. Clear the browser cache and cookies.
2. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].
3. Open DevTools and go to the Console tab.

Expected result: There are no error messages or warnings in the console.

The actual result: there is a warning in the console about using a non-standard property.

**Environment:**

macOs 10.15.2

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/RSP4xCZi0zKBvw ]

**Priority**: minor

***

</details>

<a name="BUG-8331" />
<details>
<summary>BUG-8331: The colors of the address field icons differ from the colors of the corresponding route points on the map</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the start time of the trip.
2. Enter the address in the field From Where: Usacheva, 3.
3. Enter the address in the field Where: Frunzenskaya embankment, 46.
4. Select one of the modes: Optimal, Fast, Your own. 
5. If Your Own mode is selected, select the mode of transport. 

Expected result: the colors of the address field icons match the colors of the corresponding route points on the map.

The actual result: the colors are different (mixed up).

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/6M7Iwkg5hskIRA ]

**Priority**: low

***

</details>

<a name="BUG-8334" />
<details>
<summary>BUG-8334: Address points and route on the map are displayed only after filling out the entire form</summary>

***

**Description:**

According to the requirements, if the address fields are filled in correctly, points A and B are displayed on the map. However, in order for the points to be displayed on the map and the route to be set, you have to fill in not only the address fields, but also all the other elements of the form.

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the address in the field From Where: Usacheva, 3.
2. Enter the address in the field Where: Frunzenskaya embankment, 46.

Expected result: Address points and route are displayed on the map.

Actual result: Address points and route are not displayed.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

**Screenshot**: [https://yadi.sk/i/ia9ewomAdJoOWQ ]

**Priority**: minor

***

</details>

<a name="BUG-8351" />
<details>
<summary>BUG-8351: The icon of the mode of transport and the action button are not displayed in the trip calculation result panel</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the start time of the trip.
2. Enter the address in the field From Where: Usacheva, 3.
3. Enter the address in the field Where: Frunzenskaya embankment, 46.
4. Select one of the modes: Optimal, Fast, Your own. 
5. If Your Own mode is selected, select the mode of transport.

Expected result: the trip calculation result panel displays data for each mode: the name of the mode of transport, the cost and time of the trip, a picture of the corresponding mode of transport, and an action button.

The actual result: the picture of the type of transport and the action button are not displayed.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/J4YykF5PZqgJQQ ]

**Priority**: critical

***

</details>

<a name="BUG-8355" />
<details>
<summary>BUG-8355: In the Own-Bike and Own-Scooter modes, an icon with a pedestrian and an incorrect distance between addresses is displayed on the map</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the start time of the trip.
2. Enter the address in the field From Where: Usacheva, 3.
3. Enter the address in the field Where: Frunzenskaya embankment, 46.
4. Select Your mode. 
5. Choose the type of transport Bike.

Expected result: the map displays an icon with the image of the selected mode of transport and the correct distance between the addresses (for the addresses indicated in the steps - 2.6 km).

The actual result: an icon with a pedestrian and an incorrect distance between addresses (1.8 km) is displayed on the map.

6. Choose the type of transport Scooter.

Expected result: the map displays an icon with the image of the selected mode of transport and the correct distance between the addresses (for the addresses indicated in the steps - 2.6 km).

The actual result: an icon with a pedestrian and an incorrect distance between addresses (1.8 km) is displayed on the map.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/qKS4rcmDhDOVAQ ]

**Priority**: medium

***

</details>

<a name="BUG-8367" />
<details>
<summary>BUG-8367: In the Own-On-Foot mode, the points and lines of the route are not displayed on the map and the "Open route" button does not appear</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the start time of the trip.
2. Enter the address in the field From Where: Usacheva, 3.
3. Enter the address in the field Where: Frunzenskaya embankment, 46.
4. Select Your mode. 
5. Choose the type of transport On Foot.

Expected result: the route will be plotted on the map, the button "Open in Yandex.Maps" will change to "Open route".

The actual result: the route is not displayed, the button does not change.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/8JrGL5dJ4JN6hw ]

**Priority**: critical

***

</details>

<a name="BUG-8368" />
<details>
<summary>BUG-8368: The same name of the type of transport for carsharing and your car in the result panel</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the start time of the trip.
2. Enter the address in the field From Where: Usacheva, 3.
3. Enter the address in the field Where: Frunzenskaya embankment, 46.
4. Select Your mode. 
5. Choose the type of transport Carsharing.
5. Choose your vehicle's mode of transport.

Expected result: the name of the selected mode of transport will be displayed in the trip calculation result panel.

The actual result: for different types of transport - Carsharing and Your own car - the same name of the type of transport is displayed - Auto.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshots**: [https://yadi.sk/i/8fTKVNtNCmXwSA ], [https://yadi.sk/i/g-l0KVPBffZFEA ]

**Priority**: low

***

</details>

<a name="BUG-8369" />
<details>
<summary>BUG-8369: Incorrect value "24" can be entered in the clock field</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter the value "24" in the clock field and move the focus.

Expected result: the field will be highlighted in red and an error message will be displayed.

The actual result: the field is not highlighted, the error message does not appear.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/C322eo13h1hSdw ]

**Priority**: medium

***

</details>

<a name="BUG-8370" />
<details>
<summary>BUG-8370: Incorrect error message for the address field "From"</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter an incorrect value in the address field from Where: for example, "Fahrenheit 451".

Expected result: the field is highlighted in red, and the error message "You entered an incorrect address" appears.

The actual result: the field is highlighted in red, and the error message "You entered an incorrect time" appears.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/uogNqswWgzX4sg ]

**Priority**: medium

***

</details>

<a name="BUG-8371" />
<details>
<summary>BUG-8371: Error messages are hitting input fields</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Enter incorrect values in the time fields.
2. Enter incorrect values in the address fields From Where and Where: for example, "Fahrenheit 451".

Expected result: the field is highlighted in red, the error message is displayed according to the layout.

The actual result: the field is highlighted in red, the error message hits the input fields.

**Environment:**

macOs 10.15.2

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshot**: [https://yadi.sk/i/PnPeVE5kuE1EKQ ]

**Priority**: minor

***

</details>

<a name="BUG-8372" />
<details>
<summary>BUG-8372: There are no hints about whether to fill in the input fields and select values</summary>

***

**Description:**

The cost and time of the trip are calculated, as well as the route is plotted on the map, only after filling out the entire form. If the field is not filled in or a value is not selected (mode and mode of transport), nothing happens and the user needs a hint about whether to fill in an empty value.

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Fill in the form with data/select values, leaving one of them empty/not selected:
* Clock field: 07.
* Minutes field: 45.
* Location Field: 3 Usacheva Street.
* Location Field: 46 Frunzenskaya Embankment.
* Choose one of the modes: Optimal, Fast, Your own. 
* If Your Own mode is selected, select the mode of transport.

Expected result: if the field is empty or the value is not selected, a prompt appears indicating that it must be filled in/selected.

The actual result: the prompt about the mandatory filling/selection does not appear.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Priority**: low

***

</details>

<a name="BUG-8373" />
<details>
<summary>BUG-8373: Fonts of the form do not match the layout</summary>

***

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].
2. Compare fonts with the layout (project-interface-fast.png).

Expected result: fonts match the layout.

The actual result: the fonts are slightly different.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screenshots**: [https://yadi.sk/i/R6GNjYSTzYgIlQ ] (layout), [https://yadi.sk/i/80qU8pwNMeluGw ] 

**Priority**: minor

***

</details>

<a name="BUG-8374" />
<details>
<summary>BUG-8374: The text of the input field placeholders looks like a filled-in value</summary>

***

The text of the address field placeholders looks like a filled-in value (see the project-interface-fast.png layout). For the convenience of the user, you can specify that this is an example of filling in:

Example: Usacheva, 3

**Screenshot**: [https://yadi.sk/i/R6GNjYSTzYgIlQ ] (layout)

**Type**: task

**Priority**: minor

***

</details>

<a name="BUG-8382" />
<details>
<summary>BUG-8382: If one address field is empty or incorrect, the points of both addresses are hidden on the map</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Set a route:
* Enter the start time of the trip.
* Enter the address in the From field: 3 Usacheva Street.
* Enter the address in the Where field: 46 Frunzenskaya Embankment.
* Choose one of the modes: Optimal, Fast, Your own. 
* If Your Own mode is selected, select the mode of transport.

2. Delete the value from the From field.

Expected result: point A is hidden on the map.

The actual result: points A and B are hidden on the map.

3.. Enter the correct value in the From field, set a route, and then enter the incorrect value: Fahrenheit 451.

Expected result: point A is hidden on the map.

The actual result: points A and B are hidden on the map.

4. Enter the correct address in the From field: Usacheva 3.
5. Delete the value from the Where field.

Expected result: point B is hidden on the map.

The actual result: points A and B are hidden on the map.

6. Enter the correct value in the Where field, set a route, and then enter the incorrect value: Fahrenheit 451.

Expected result: point B is hidden on the map.

The actual result: points A and B are hidden on the map.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

**Screencast**: [https://yadi.sk/i/Jc658TkoZqJgag ]

**Priority**: medium

***

</details>

<a name="BUG-8384" />
<details>
<summary>BUG-8384: Copy and Paste actions are not performed when selecting the appropriate context menu items</summary>

***

**Prerequisites:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].

**Playback Steps:**

1. Select any text on the form.
2. Open the context menu.
3. Select the Copy menu item.
4. Place the cursor in any input field of the form.
5. Open the context menu.
6. Select the Insert menu item.
7. Paste the copied text anywhere other than the input fields (for example, in the browser's address bar).

Expected result: the text is copied and pasted.

The actual result: the text is not copied; it is impossible to select the Insert context menu item (the item is inactive) in the input fields.

**Environment:**

macOs 10.15.2

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screencast**: [https://yadi.sk/i/sJ1e3iF8ktmWMg ]

**Priority**: medium

***

</details>

<a name="BUG-8397" />
<details>
<summary>BUG-8397: In the inactive transport modes panel, the mouse cursor changes when hovering over the transport icon buttons</summary>

***

**Description:**

In the starting state, as well as in Optimal and Fast modes, the transport modes panel is inactive, but if you hover the mouse cursor over the transport icon buttons, it changes, incorrectly signaling the possibility of performing an action.

**Playback Steps:**

1. Open Yandex.Routes: [https://qa-routes.praktikum-services.ru ].
2. Hover the mouse cursor over the transport icons in the inactive transport modes panel.

Expected result: the mouse cursor does not change.

The actual result: the mouse cursor changes.

3. Set a route:
* Enter the start time of the trip.
* Enter the address in the From field: 3 Usacheva Street.
* Enter the address in the Where field: 46 Frunzenskaya Embankment.
* Choose the Optimal mode.

4. Hover the mouse cursor over the transport icons in the inactive transport modes panel.

Expected result: the mouse cursor does not change.

The actual result: the mouse cursor changes.

5. Select the Fast mode.

6. Hover the mouse cursor over the transport icons in the inactive transport modes panel.

Expected result: the mouse cursor does not change.

The actual result: the mouse cursor changes.

**Environment:**

macOs 10.15.2

Yandex Browser 20.3.0.2220 (64-bit)<br>
Screen resolution: 800x600

Firefox 75.0 (64-bit)<br>
Screen resolution: 1280x720

**Screencast**: [https://yadi.sk/i/yrZck0Ll3xmxKg ]

**Priority**: low

***

</details>

[Up](#up)
## <a name="mobile-testing" />Mobile Application Testing

### Task

1. Analyze the requirements for the Yandex mobile application.Metro.

2. Write a checklist for testing the mobile application for some of the requirements ** highlighted in bold**.
3. Test the mobile application in the emulator using Android Studio or on your Android device and create bug reports. Download the upcoming version of the app [here](https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk).

To check that the update is working correctly, download the previous version [here](https://code.s3.yandex.net/qa/files/yandexmetro-android-v2.1.apk).

4. Write a test report. What can you tell the team about the status of the tested part of the product?

<details>
<summary>Requirements for the Metro mobile app</summary>

***

Yandex.Metro is a service that allows you to navigate the subway using a mobile device. The app has a metro map that helps you set a route and estimate travel time. The app displays up-to-date notifications about metro stations and schedule changes.

**1. List of routes**

1.1. The route card displays:
- Route information - metro logos and metro line numbers, as well as the sequence of transfers (if any).
- The number of transfers (if any).
- Time interval of the route - travel time, departure and arrival time.
- The Close button.
- **Route Details button.**
- Fields From (starting point) and To (destination) (fields must be validated).

![Route card](https://code.s3.yandex.net/qa/schemes/project-3-sprint1.png)

1.2. Reset the route.

- You can only close the route by tapping on the X in the route card. When you close a route, the starting station from the last route is saved in the input field. The input field for Where and the route in the diagram is reset, and the stations are no longer highlighted (except for the starting station).
- **If the current time exceeds the end time of the route, the time interval of the route is updated.**

**2. Station selection**

2.1. There are several ways to select a station.:
- **Tap on the diagram.**
- **By the i icon from different cards. If you select a station from the search by tapping on i and closing the card, you should return to the search screen.**

![Station card](https://code.s3.yandex.net/qa/schemes/project-3-sprint3.png)

- **Find in the search and click on the station.**

2.2. If the station is selected, the following actions are always performed:
- **The station point on the diagram is decreasing.**
- **A pin of the line color or a special pin for a closed station appears at the station point.**
- **The selected station is saved in the history: clicking on the From or To field opens a list containing the stations that the user selected earlier. The list should be saved in the new version of the application.**
- **The font of the station name becomes bold.**

**3. Route Details**

3.1. Going to the card

**Route details can be opened in two ways:**

- **Tap on the Route Details button in the route card.**
- **Swipe up the route list (only for smartphones in portrait orientation).**

3.2. Display

The card displays:

- Time interval of the route:
  - travel time;
  - departure time;
  - arrival time;
  - transfer segments between route sections.
- The Close button.
- Sections of the route separated by transfer messages.
- A message about convenient boarding cars.
- A picture showing convenient carriages.
- Arrival and departure stations.
- Transfer stations.
- Intermediate stations (if there is more than one intermediate station on the site, it is displayed in a collapsed list).
- Next to each station, except for intermediate ones, the "I" button is displayed to go to the station card.
- The station located at the beginning of each section contains the name, line number and service icon.
- An event can be displayed for each station.
- **When the orientation changes from portrait to landscape, the route details are displayed on the left side of the screen.**

3.3. Closing the card

There are also two ways to close a card:

- Tap on the Close button.
- Swipe down.

When you close the details, the list of routes remains open, the position of the list is saved, and the route is not reset.

**4. Error Notification**

- **If there is no internet connection, an error notification appears.**

**5. Logic for landscape orientation**

- **All cards and search fields are displayed on the left side of the screen.**
- Cards open to the full height of the screen.
- Station cards are closed when interacting with the circuit.
- Routes are displayed in the list on the left side of the screen.
- The banner on the route is displayed at the top of the route list if there is enough space available.
- **When changing the orientation of the screen, the scale of the route should not increase or decrease.**
- The list of routes does not collapse when tapping on the route cell. The selected route is highlighted.
- When building a route, the route fits into the free area on the right.
- When tapping on a station in the diagram (with and without a route), the diagram is minimally scrolled to accommodate the pin.
- When a station is selected by the i icon, the circuit is minimally scrolled to accommodate the pin.
- The cards retain their position when switching from portrait to landscape orientation (and vice versa): collapsed ones remain collapsed, open ones remain open, and the average position changes to the average position.
- The Settings card opens in the center of the screen on some devices (iPad and some iPhones)

**6. Long-trip around the station**

- **Clicking on a station with a long-tap opens the station card with the "From Here" buttons/"This way."**

![Longtop](https://code.s3.yandex.net/qa/files/project-3-sprint41.png)

- **The circuit must not shift up/down/left/right when long-stepping through the station.**

**7. Scroll the diagram using a long-stop**

To reproduce the scroll scheme using a long—tap, make a long-tap on the station and hold your finger, shift the focus to other stations.

- **When scrolling with a long-tap, you can select the desired station, while the circuit remains stationary.**
- **When a station point or its name falls into the click area, a pin is placed on the point, the station point decreases, the station name is highlighted in bold, and a station card appears.**
- **The pin at the station and the station is highlighted disappears when it does not fall into the click area.**
- When moving further The header of the station card remains stationary, and the names of stations and services change in it. At the same time, the card retains a minimal state.
- **If the movement ends in an empty area, the station card is closed.**

***

</details>

### Solution

**1-2. The checklist of the mobile application**

| № | Description | Status | Bug Report ID | Requirement |
|:-:|:----------|:-------:|:----------:|:----------:|
|1 | The update from the previous version of the application is correct | PASSED		
|2 | The Route Details button according to the layout is displayed in the route card | PASSED | | 1.1
|3 | If the current time exceeds the end time of the route, the time interval of the route is updated | FAILED | [BUG-10736](#BUG-10736) | 1.2
| |	**Ways to select a station:**| | |2.1|
|4 | Tap on the diagram | PASSED		
|5 | By the i icon from different cards | PASSED	
|6 | If you select a station from the search by tapping on i and closing the card, you should return to the search screen | FAILED | [BUG-10738](#BUG-10738)
|7 | Find in the search and click on the station | PASSED
| | **If the station is selected, the following actions are always performed:**| | | 2.2 |
|8 | The station point on the diagram is decreasing | PASSED
|9 | A pin of the line color or a special pin for a closed station appears at the station point | PASSED		
|10 | The selected station is saved in the history: clicking on the From or To field opens a list containing the stations that the user selected earlier | FAILED | [BUG-10740](#BUG-10740)
|11 | The list of stations in the history should be saved in the new version of the application | PASSED		
|12 | The font of the station name becomes bold | PASSED
| | **Go to the route card. Route details can be opened in two ways:**| | |3.1|
|13 | Tap on the Route Details button in the route card | PASSED		
|14 | Swipe up the route list (only for smartphones in portrait orientation) | PASSED		
|15 | When changing orientation from portrait to landscape, route details in the card are displayed on the left side of the screen | FAILED | [BUG-10929](#BUG-10929) | 3.2
| |	**Error notification:**| | |4 |	
|16 | When there is no internet connection, an error notification appears | FAILED | [BUG-10741](#BUG-10741)
| | **Landscape orientation:**| | |5|
|17 | All cards and search fields are displayed on the left side of the screen | PASSED
|18 | When changing the orientation of the screen, the scale of the route should not increase or decrease | FAILED | [BUG-10743](#BUG-10743)
| | **Long-tap on the station:**| | | 6|
|19 | Clicking on a station with a long tap opens the station card with the "From Here" buttons/"This way" | FAILED | [BUG-10744](#BUG-10744)
|20 | The circuit must not shift up/down/left/right when long-stepping through the station | FAILED | [BUG-10746](#BUG-10746)	
| | **Scroll the scheme using a long tap:** | | |	7|
|21 | When scrolling with a long-tap, you can select the desired station, while the circuit remains stationary | PASSED		
|22 | When a station point or its name falls into the click area, a pin is placed on the dot, the station point decreases, the station name is highlighted in bold, the station card |PASSED
|23 | Pin appears at the station and the station is highlighted when it does not fall into the click area | FAILED | [BUG-10747] (#BUG-10747)	
|24 | If the movement ends in an empty area, the station card is closed | PASSED		

**3-4. Test Report**

The team has tested some of the requirements for the Yandex mobile app.Metro.

We have checked the correctness of the update from the previous version, the display of the "Route Details" button, ways to select stations, perform actions at the selected station, options for going to the route card, notification of an error when there is no Internet connection, saving the list of stations in the history, the behavior of the application when changing the orientation of the screen and when using a long-stop.

Among the errors found, there is one critical one, the rest have medium and low priority. The team recommends correcting errors before handing the product over to users. In total, we found 9 bugs, here is a list of bug reports.:

<a name="BUG-10736" />
<details>
<summary>BUG-10736: The time interval of the route in the route card is not updated if the current time exceeds the end time of the route</summary>

***

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application, and grant all permissions when requested.
3. Select any city, for example: Saint Petersburg.

**Playback Steps:**

1. Tap to select any station, for example: Victory Park.
2. In the station card that appears, click From Here.
3. Tap to select another station, for example: Gorkovskaya.
4. In the station card that appears, click Here.

Expected result: A route card will appear indicating the start and end time of the route.

The actual result: a route card has appeared indicating the start and end time of the route.

5. Wait until the current time exceeds the end time of the route.

Expected result: if the current time exceeds the end time of the route, the time interval of the route is automatically updated.

Actual result: If the current time exceeds the end time of the route, the time interval of the route is not updated.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screenshot:** [https://yadi.sk/i/Bj41vqKK-lpvgw]

**Priority:** average

***

</details>

<a name="BUG-10738" />
<details>
<summary>BUG-10738: When selecting a station in the search by tapping on i and closing the card, it returns to the main screen</summary>

***

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application, and grant all permissions when requested.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Click in the From field.
2. In the search box that appears, type the name of the station, for example: Nevsky Prospekt.
3. Open the station card with a tap on the i.
4. Close the station card with a tap on the x.

Expected result: if you select a station from the search by tapping on i and closing the card, you should return to the search screen.

Actual result: returns to the main screen.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/YQNOsAuS4mRW0A]

**Priority:** low

***

</details>

<a name="BUG-10740" />
<details>
<summary>BUG-10740: The selected station is saved in the history only after the ma was built with it.

  <a name="BUG-10743" />
<details>
<summary>BUG-10743: When the screen orientation changes, the scale of the route changes</summary>

***

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Set a route in the portrait orientation of the screen:
* Tap on any station, for example: Gorkovskaya.
* In the station card that appears, click From Here.
* Tap on any station, for example: Victory Park.
* In the station card that appears, click Here.
2. Change the screen orientation to landscape.
3. Change the screen orientation to portrait.

Expected result: when changing the orientation of the screen, the scale of the route should not increase or decrease.

The actual result: when changing the screen orientation from portrait to landscape, the scale of the route decreases slightly, and increases slightly from landscape to portrait.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/lsnfIF5-T2h3yA]

**Priority:** low

***

</details>

<a name="BUG-10744" />
<details>
<summary>BUG-10744: The station card sometimes opens incorrectly when you long-tap to a station</summary>

***

**Description:** when clicking on a station with a long-step, the station card opens in different ways: it may open correctly; it may partially open (only the station name, number and line name are visible); it may partially open, remain open while holding the finger on the station and close if the finger is released. Sometimes the focus jumps from the selected station to the next/several neighboring stations and the card of the neighboring station opens.

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Long-step to select different stations on the diagram.

Expected result: when clicking on a station using a long-tap, the station card opens with the "From Here" buttons/"This way"

The actual result: when you long-tap to a station, the station card sometimes opens incorrectly.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/5oA-6pbNXQx2ZQ]

**Priority:** critical

***

</details>

<a name="BUG-10746" />
<details>
<summary>BUG-10746: When long-stepping through a station, the scheme sometimes shifts</summary>

***

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Long-step to select different stations on the diagram.

Expected result: the circuit should not shift up/down/left/right when long-stepping through the station.

The actual result: sometimes the circuit is shifted.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/ymC79S_BpaLkdQ]

**Priority:** low

***

</details>

<a name="BUG-10747" />
<details>
<summary>BUG-10747: When scrolling the long-step scheme, the pin on the station and the station selection does not disappear when it does not fall into the click zone</summary>

***

**Description:** when scrolling the long-tap scheme, the pin on the station and the station selection does not disappear when it does not fall into the click zone. To reproduce the scrolling scheme using a long-tap, you need to make a long-tap on the station and, holding your finger, shift the focus to other stations.

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Perform a long-step scroll scheme.

Expected result: the pin is on the station and the station is highlighted when it does not fall within the click area.

The actual result: the pin on the station and the station's highlight does not disappear when it does not fall into the click area.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/hqgm5pfb27mdRg]

**Priority:** low

***

</details>

<a name="BUG-10929" />
<details>
<summary>BUG-10929: When changing orientation from portrait to landscape, the route details card closes</summary>

***

**Prerequisites:**

1. Download the Yandex version.Metro by link [https://code.s3.yandex.net/qa/files/yandexmetro-android-v3.6.apk].
2. Install and launch the application.
3. Select any city (for example, Saint Petersburg).

**Playback Steps:**

1. Set a route in the portrait orientation of the screen:
* Tap on any station, for example: Gorkovskaya.
* In the station card that appears, click From Here.
* Tap on any station, for example: Victory Park.
* In the station card that appears, click Here.
2. Open the route details card.
3. Change the screen orientation to landscape.

Expected result: When the orientation is changed from portrait to landscape, the route details in the card are displayed on the left side of the screen.

The actual result: when the orientation changes from portrait to landscape, the route details card closes.

**Environment:**

Xiaomi Redmi 4X<br>
Android 7.1.2<br>
Yandex.Metro 3.6

**Screencast:** [https://yadi.sk/i/_IELHx4FzryPEA]

**Priority:** average

***

</details>

## <a name="api-testing" />API Testing

### Task

1. Analyze the requirements for the Yandex backend API.Metro.

2. Test the Yandex backend API.Metro:

	2.1. according to the checklist from the template;

2.2. according to the instructions.

<details>
<summary>Instructions for testing the API</summary>

***

- Go to postman.
- Get a list of schemas from the "/list" query.
- Create a table in the third tab of the template, matching each city with its ID.
- Complete the checklist for the "/events" request. Make sure that the response is returned for each id according to the requirements.
- Check the correctness of the "/events" query.

***

</details>

During the testing process, mark the test results: PASSED or FAILED. If the test has the FAILED status, create a bug report in Yandex.Place the tracker in the BUG queue and enter the ID in the corresponding results table.

3. Write a test report. What can you tell the team about the status of the tested part of the product?

<details>
<summary>Requirements for the Metro API</summary>

***

Metro uses the metrokit-service API. This is the API for the MetroKit library.

The functionality of the metrokit service includes:

- Access to the full list of schemes — GET /v1/list/
- Access to the list of current events for the selected scheme — GET /v1/events/

**GET /v1/list/**

GET to the URI: [https://metrokit-service.maps.yandex.net/v1/list ]

The answer is a complete list of schemes for all cities. The response is always in JSON format.

An example of Moscow's response structure.

```bash
        {
"id": "sc34974011",
"name": {
"en": "Moscow",
"ru": "Moscow",
                "tr": "Moscow",
"uk": "Moscow"
            },
            "size": {
                "packed": 369292,
                "unpacked": 3001856
            },
            "tags": [
                "published"
            ],
            "aliases": [
                "moscow"
            ],
            "logoUrl": "https://avatars.mds.yandex.net/get-bunker/128809/6f088274a46aee3df308423d222eb36906825cb7/orig",
            "version": "2e5e649",
            "geoRegion": {
                "delta": {
                    "lat": 0.32158,
                    "lon": 0.439453
                },
                "center": {
                    "lat": 55.743347,
                    "lon": 37.617188
                }
            },
            "countryCode": "RU",
            "defaultAlias": "moscow"
        }
```

**GET /v1/events/**

GET to the URI: [https://metrokit-service.maps.yandex.net/v1/events ]

Parameter: [scheme_id=\<id>]

Example: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc34974011 ]

The response is a list of events for the selected scheme. The response is always in JSON format.

If the event is present, the response has this structure. For example, for Moscow with id = sc34974011:

```bash
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  }
```

If there are no events, the list may be empty, but it must be returned in JSON format. For example, for Helsinki with id = sc38955480:

```bash
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

***

</details>

### Solution

<details>
<summary><strong>1. Schema data</strong></summary>

***

| City | Scheme ID |
|:----------|:----------|
|Novosibirsk | sc02877545 |
|Moscow | sc34974011 |
|Kiev | sc21078879 |
|San Francisco | sc99912 |
|Almaty | sc37730841 |
|Helsinki | sc38955480 |
|Lisbon | sc39559734 |
|Dnipro | sc47063979 |
|Dubai | sc19930717 |
|Bucharest | sc66937172 |
|Athens | sc92836217 |
|Sofia | sc29665623 |
|Kazan | sc63288776 |
|Minsk | sc31709615 |
|Vienna | sc52507030 |
|Kharkiv | sc12039691 |
|Milan | sc999 |
|Budapest | sc04704892 |
|Tashkent | sc19351236 |
|Tbilisi | sc46903964 |
|Warsaw | sc12943371 |
|Volgograd | sc03517743 |
|Saint Petersburg | sc60983525 |
|Yerevan | sc95957238 |
|Prague | sc20559874 |
|Yekaterinburg | sc58473698 |
|Baku | sc54283234 |
|Samara | sc33333931 |
|Istanbul | sc97451070 |
|Nizhny Novgorod | sc77792237 |
|Rome | sc68078330 |

***

</details>

**2. API Checklist**

| № | Description | Status | ID of the bug report |
|:----------:|:----------|:----------:|:----------:|
| | **for the "/list" query** 
|1 | the response to the request returns 200 OK | PASSED
|2 | response to the request in JSON format | PASSED	
|3 | the response structure for Minsk matches the structure in the requirements | PASSED	
|4 | the response structure for Athens matches the structure in the requirements | PASSED	
|5 | the response structure for Kiev matches the structure in the requirements | PASSED	
|6 | the response structure for Kazan matches the structure in the requirements | PASSED
|7 | the response structure for Rome matches the structure in the requirements |PASSED
| | **for the request "/events"** 
8 | the response structure for id=sc02877545 matches the structure in the requirements | PASSED	
9 | the response structure for id=sc34974011 matches the structure in the requirements | PASSED	
10 | the response structure for id=sc21078879 matches the structure in the requirements | PASSED	
11 | the response structure for id=sc99912 matches the structure in the requirements | FAILED | [BUG-10766](#BUG-10766)
12 | the response structure for id=sc37730841 matches the structure in the requirements | PASSED	
13 | the response structure for id=sc38955480 matches the structure in the requirements | PASSED	
14 | the response structure for id=sc39559734 matches the structure in the requirements | PASSED	
15 | the response structure for id=sc47063979 matches the structure in the requirements | PASSED	
16 | the response structure for id=sc19930717 matches the structure in the requirements | PASSED	
17 | the response structure for id=sc66937172 matches the structure in requirements | FAILED | [BUG-10769](#BUG-10769)
18 | the response structure for id=sc92836217 matches the structure in requirements |FAILED | [BUG-10770](#BUG-10770)
19 | the response structure for id=sc29665623 matches the structure in the requirements | FAILED | [BUG-10771](#BUG-10771)
20 | the response structure for id=sc63288776 matches the structure in the requirements | PASSED	
21 | the response structure for id=sc31709615 matches the structure in the requirements | PASSED	
22 | the response structure for id=sc52507030 matches the structure in the requirements | FAILED | [BUG-10773](#BUG-10773)
23 | the response structure for id=sc12039691 matches the structure in the requirements | PASSED	
24 | the response structure for id=sc999 matches the structure in the requirements | FAILED | [BUG-10775](#BUG-10775)
25 | the response structure for id=sc04704892 matches the structure in the requirements | PASSED	
26 | the response structure for id=sc19351236 matches the structure in the requirements | PASSED	
27 | the response structure for id=sc46903964 matches the structure in the requirements | PASSED	
28 | the response structure for id=sc12943371 matches the structure in the requirements | PASSED	
29 | the response structure for id=sc03517743 matches the structure in the requirements | PASSED	
30 | the response structure for id=sc60983525 matches the structure in the requirements | PASSED	
31 | the response structure for id=sc95957238 matches the structure in the requirements | PASSED	
32 | the response structure for id=sc20559874 matches the structure in the requirements | FAILED | [BUG-10776](#BUG-10776)
33 | the response structure for id=sc58473698 matches the structure in the requirements | PASSED	
34 | the response structure for id=sc54283234 matches the structure in the requirements | PASSED	
35 | the response structure for id=sc33333931 matches the structure in the requirements | PASSED	
36 | the response structure for id=sc97451070 matches the structure in the requirements | PASSED	
37 | the response structure for id=sc77792237 matches the structure in the requirements | PASSED	
38 | the response structure for id=sc68078330 matches the structure in the requirements | PASSED	

**3. Test Report**

The team tested the Yandex backend API.Metro.

We have checked the structure of the response to the request/list for some cities and the structure of the response to the request/events for all city schemes.

There are no blockers or critical errors found, all of them have an average priority. The team recommends correcting errors before handing the product over to users. In total, we found 7 bugs, here is a list of bug reports.:

<a name="BUG-10766" />
<details>
<summary>BUG-10766: When requesting a list of events for a schema with id=sc99912, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc99912 ].
2. View the response list of events for the scheme with id=sc99912 (San Francisco).
Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc99912 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/Q39JhIx4tKU-YQ]

**Priority:** average

***

</details>

<a name="BUG-10769" />
<details>
<summary>BUG-10769: When requesting a list of events for a schema with id=sc66937172, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc66937172 ].
2. View the response list of events for the scheme with id=sc66937172 (Bucharest).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc66937172 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/oDdjegyhnIq8Tg]

**Priority:** average

***

</details>

<a name="BUG-10770" />
<details>
<summary>BUG-10770: When requesting a list of events for a schema with id=sc92836217, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc92836217 ].
2. View the response list of events for the scheme with id=sc92836217 (Athens).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc92836217 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/NgCePxnySF7-fg]

**Priority:** average

***

</details>

<a name="BUG-10771" />
<details>
<summary>BUG-10771: When requesting a list of events for a schema with id=sc29665623, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc29665623 ].
2. View the response list of events for the scheme with id=sc29665623 (Sofia).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc29665623 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/8es5sVV7KTEquw]

**Priority:** average

***

</details>

<a name="BUG-10773" />
<details>
<summary>BUG-10773: When requesting a list of events for a schema with id=sc52507030, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc52507030 ].
2. View the response list of events for the circuit with id=sc52507030 (Vienna).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc52507030 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/qm7fZ7gRUBH_1A]

**Priority:** average

***

</details>

<a name="BUG-10775" />
<details>
<summary>BUG-10775: When requesting a list of events for a schema with id=sc999, the response structure does not match the requirements</summary>

***
**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc999 ].
2. View the response list of events for the scheme with id=sc999 (Milan).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc999 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/CTRfVyRBISXlwA]

**Priority:** average

***

</details>

<a name="BUG-10776" />
<details>
<summary>BUG-10776: When requesting a list of events for a schema with id=sc20559874, the response structure does not match the requirements</summary>

***

**Playback Steps:**

1. Create and send a GET request for the URI in Postman: [https://metrokit-service.maps.yandex.net/v1/events?scheme_id=sc20559874 ].
2. View the response list of events for the scheme with id=sc20559874 (Prague).

Expected result:
1. Response status: 200 OK.
2. The response is returned in JSON format, the response structure for id=sc20559874 matches the structure in the requirements.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": [
            {
                "id": "ev-76c50f2d-9c3d-4835-ac73-a0544b1b308e",
                "schedule": {
                    "to": "2019-10-25T23:10:00+03:00",
                    "from": "2019-03-30T01:00:00+03:00",
                    "type": "Interval"
                },
                "title": {
                    "en": "No trains to Kakhovskaya station",
                    "ru": "Trains do not run to Kakhovskaya"
},
                "description": {
                    "en": "Station closure",
                    "ru": "Station closure"
                },
                "changeIds": [
                    "ch-feed3c7d-d35e-4481-87a4-7c141e1c96c0"
                ]
            }
  } 
```

3. If there are no events, the list may be empty, but it must be returned in JSON format.
Example of the response structure:

```
{
    "events": {
        "type": "IndexedCollection",
        "items": []
    },
    "changes": {
        "type": "IndexedCollection",
        "items": []
    }
}
```

The actual result:
1. Response status: 404 Not Found.
2. The response structure does not match the requirements: The response body is empty.
3. The response is returned in Text format.

**Environment:**

macOs 10.15.2<br>
Postman 7.22.1<br>
API metrokit-service

**Screenshot:** [https://yadi.sk/i/_IBj6x2VK8hDTw]

**Priority:** average

***

</details>

[Up](#up)

## <name="database" />Description of the database

### Task 1

Information has been received from the developers: we need to find out how they reacted to the IP address. The IP address consists of four parts, separated by dots. You need addresses that start with "233.201.".

The rules are located on a remote server at logs/2019/12. The date on which the error occurred is unknown.

Your task is to find out which requests have been sent.

In the response, attach:
1. The team that you managed to get the necessary logs.
2. Suitable strings, for example:

`Bashing
184.79.247.161 - - [30/12/2019:21:38:13 +0000] " PUT /HTTP/1.1 alerts" 400 3557
``

### Solution

1. The log receipt command:

``bash
grep ^233.201 /logs/2019/12/* .txt
``

2. Logs:

``bash
233.201.188.154 - - [18/12/2019:21:46:01 +0000] "DELETE /HTTP events/1.1" 403 3971
233.201.182.9 - - [21/12/2019:21:56:20 +0000] " FIX /HTTP/1.1 users" 400 4118
``

[check](#up)

### Task 2

A bug has been found in the system. It appeared on 30.12.2019 and 31.12.2019 from 21:30:00 to 21:39:59. At the same time, errors with numbers 400 and 500 appeared. Your task is to save the logs that were recorded during this period to a separate file. Then these logs should be decomposed into separate files: put logs with the same error in one file.

How to do it:
1. Error 1 appeared in the domain directory in one of the first places.
2. Please leave any questions you have in the above location here.
main.txt 3. Fix error 1 caused by deleting events.
4. Internal broadcasts of events are conducted for participants with numbers 400 and 500. File Namesreferences 400.txt and 500.txt responsibly. There was nothing of the complicity of oybka izzhaila in them. main.txt .

In the response, attach:
1. The codes that are the cause of error elimination 1 and events.
2. The team that you select requests for the specified period. Before you forget who you are... main.txt
3. The songs you call V. Khaila's books 400.txt and 500.txt from main.txt .
4. Tactikhailov 400.txt and 500.txt .

<details>
<summary><strong>Database of dachshund's trip to Chico</strong></summary>

***

The "neighborhood" table — information about the city area:
- 'area identifier' — area code;
- 'name' is the name of the area.
Table "taxi" — information about taxis:
- `cab_id' — autopilot code;
- `vehicle_id' — technical identifier of the vehicle;
- `company_name" — the company that drives the car.
Table "trips" — information about trips:
- "trip_id" is the trip code.;
- `cab_id` — the code of the car in which the trip was made;
- `start_ts` — date and time of the start of the trip (round table time);
- `end_ts` — date and time of the end of the trip (round table time);
- `duration_seconds` — the duration of the trip in seconds.;
- `distance_miles' — the distance between the miles;
- `pickup_location_id` — the code of the paradise mountain in which the trip was specified;
- `dropoff_location_id' is the Ryan Gore code where the ride appeared.
The "Weather records" page — weather information:
- `record ID' is the code for requesting possible changes;
- `c' — date and time of the meeting (time of the round table);
- `temperature" — the temperature for memory;
- `description` — a brief description of the current situation. For example, "light rain" or "scattered clouds".

***

</details>

<details>
<short description><strong>My table</strong></short description>

***

![My DB table](https://code.s3 .yandex.net/qa/schemes/project_4_sprint.png)

***

</details>

At the heart of the data, there is no direct relationship between the `trips` and `weather records' tables. You can contact the timesheets both by the start time of the trip (`trips.start_ts`) and by the time of upcoming events (`weather_records.ts').

### Solution

1. Error codes 1 and event codes:

``bash

mkdir error 1 and events
```

2. The command for selecting requests for the specified period:

``bash
grep "21:3" ~/logs/2019/12/apache_2019-12-30.txt > ~/bug1/main.txt
grep "21:3" ~/logs/2019/12/apache_2019-12-31.txt >> ~/bug1/main.txt
```

3. Novels of Lair composition in files 400.txt and 500.txt from main.txt:

``bash
grep -w "400" ~/bug1/main.txt > ~/bug1/events/400.txt
grep -w "500" ~/bug1/main.txt > ~/bug1/events/500.txt
```

<details>
<short description>4. 400.txt </short description>

***

`bash
80.57.170.51 - - [12/30/2019:21:35:12 +0000] " DELETE /HTTP/1.1 users" 400 3623
204.235.176.118 - - [30/12/2019:21:35:13 +0000] " MESSAGE /HTTP/1.1 users" 400 4704
82.95.203.67 - - [30/12/2019:21:35:19 +0000] " DELETE /lists HTTP/1.1" 400 3737
155.242.215.46 - - [30/12/2019:21:35:38 +0000] " RECORDING /collections of plays HTTP/1.1" 400 4450
189.176.85.0 - - [30/12/2019:21:35:39 +0000] " HTTP/1.1 FIXES/alerts" 400 2732
13.108.71.71 - - [30/12/2019:21:35:43 +0000] " CORRECTION /HTTP events/1.1" 400 3410
195.213.133.182 - - [30/12/2019:21:35:46 +0000] " PUT /HTTP/1.1 clients" 400 3085
235.243.133.78 - - [30/12/2019:21:35:47 +0000] " FIX /HTTP/1.1 clients" 400 3264
192.57.115.49 - - [30/12/2019:21:35:55 +0000] " POST / HTTP analyzers/1.1" 400 2457
71.0.49.244 - - [30/12/2019:21:35:55 +0000] " PUT /HTTP/1.1 collectors" 400 2785
224.159.206.126 - - [30/12/2019:21:36:02 +0000] " DELETE /HTTP clients/1.1" 400 4569
131.35.106.246 - - [30/12/2019:21:36:04 +0000] " DELETE /lists HTTP/1.1" 400 2578
216.24.42.208 - - [30/12/2019:21:36:06 +0000] " GET /HTTP/1.1 analyzers" 400 4597
123.53.150.160 - - [30/12/2019:21:37:19 +0000] " CORRECTION /HTTP events/1.1" 400 4379
61.129.127.103 - - [30/12/2019:21:37:32 +0000] " RECEIVE /list HTTP/1.1" 400 2575
90.216.4.78 - - [30/12/2019:21:37:34 +0000] " HTTP FIXES /lists/1.1" 400 3899
204.250.214.208 - - [30/12/2019:21:37:36 +0000] " HTTP PATCH /analyzers/1.1" 400 4742
79.214.240.98 - - [30/12/2019:21:37:37 +0000] " POST /HTTP/1.1 field sets" 400 4441
65.47.42.12 - - [30/12/2019:21:37:39 +0000] " FIX /HTTP/1.1 clients" 400 2500
251.118.141.34 - - [30/12/2019:21:37:41 +0000] " MESSAGE /HTTP/1.1 clients" 400 3519
205.20.166.196 - - [30/12/2019:21:37:51 +0000] " MESSAGE / HTTP users/1.1" 400 4032
156.217.3.46 - - [30/12/2019:21:37:52 +0000] " HTTP/1.1 PATCH/analyzers" 400 2020
48.240.198.167 - - [30/12/2019:21:37:57 +0000] " PATCH / game collections HTTP/1.1" 400 4100
101.255.159.211 - - [30/12/2019:21:37:59 +0000] " GET HTTP/1.1 authorization" 400 2324
80.76.98.203 - - [30/12/2019:21:38:00 +0000] " RECORDINGS /collections of plays HTTP/1.1" 400 3045
85.64.63.255 - - [30/12/2019:21:38:13 +0000] " HTTP/1.1 PATCH/collections" 400 2291
184.79.247.161 - - [30/12/2019:21:38:13 +0000] " PUT /HTTP alerts/1.1" 400 3557
93.2.134.22 - - [30/12/2019:21:39:39 +0000] " DELETE /HTTP/1.1 alerts" 400 3701
86.34.86.182 - - [31/12/2019:21:35:10 +0000] " HTTP MESSAGE /authorization/1.1" 400 3626
167.37.16.117 - - [31/12/2019:21:35:17 +0000] " FIX /HTTP/1.1 clients" 400 3294
199.128.92.19 - - [31/12/2019:21:35:43 +0000] " PUT /HTTP users/1.1" 400 4180
162.152.99.143 - - [31/12/2019:21:35:59 +0000] " PUT /HTTP/1.1 users" 400 4606
83.115.59.224 - - [31/12/2019:21:37:26 +0000] " RECEIVE /NOTIFY HTTP/1.1" 400 3489
194.10.97.226 - - [31/12/2019:21:37:31 +0000] " DELETE /lists HTTP/1.1" 400 2447
180.99.214.40 - - [31/12/2019:21:37:44 +0000] " DELETE /HTTP/1.1 alerts" 400 2077
154.152.205.4 - - [31/12/2019:21:37:50 +0000] " GET /playbooks HTTP/1.1" 400 3324
197.82.125.54 - - [31/12/2019:21:37:52 +0000] " PUT /HTTP/1.1 field sets" 400 4365
115.89.87.219 - - [31/12/2019:21:38:06 +0000] " INSTALL /play books HTTP/1.1" 400 2589
100.77.15.14 - - [31/12/2019:21:38:07 +0000] " GET /fieldsets HTTP/1.1" 400 4911
22.33.159.242 - - [31/12/2019:21:38:07 +0000] " GET /playbooks HTTP/1.1" 400 3955
149.148.229.11 - - [31/12/2019:21:39:16 +0000] " RECEIVE /users HTTP/1.1" 400 2071
236.107.64.192 - - [31/12/2019:21:39:17 +0000] " FIX /HTTP/1.1 users" 400 2791
24.156.105.39 - - [31/12/2019:21:39:23 +0000] " RECEIVE /list HTTP/1.1" 400 2902
193.50.164.254 - - [31/12/2019:21:39:23 +0000] " INSTALL /play books HTTP/1.1" 400 3296
18.123.104.91 - - [31/12/2019:21:39:52 +0000] " GET / collectors HTTP/1.1" 400 4372
234.218.148.4 - - [31/12/2019:21:39:54 +0000] " ADD /users HTTP/1.1" 400 2509
```

***

</details>

<details>
<summary>5. 500.txt </summary>

***

```bash
64.250.112.189 - - [30/12/2019:21:35:13 +0000] "PUT /parsers HTTP/1.1" 500 4639
193.253.101.180 - - [30/12/2019:21:35:31 +0000] "PATCH /alerts HTTP/1.1" 500 2944
197.106.117.194 - - [30/12/2019:21:35:31 +0000] "PATCH /parsers HTTP/1.1" 500 3519
247.124.71.67 - - [30/12/2019:21:35:45 +0000] "PUT /alerts HTTP/1.1" 500 2746
62.88.204.119 - - [30/12/2019:21:35:51 +0000] "PUT /auth HTTP/1.1" 500 2666
125.156.142.26 - - [30/12/2019:21:36:01 +0000] "PATCH /events HTTP/1.1" 500 3460
144.170.212.70 - - [30/12/2019:21:36:04 +0000] "DELETE /parsers HTTP/1.1" 500 2599
59.39.200.252 - - [30/12/2019:21:36:05 +0000] "GET /alerts HTTP/1.1" 500 2650
150.136.200.100 - - [30/12/2019:21:37:24 +0000] "POST /lists HTTP/1.1" 500 2684
84.81.25.45 - - [30/12/2019:21:37:25 +0000] "POST /auth HTTP/1.1" 500 2052
30.222.160.141 - - [30/12/2019:21:37:30 +0000] "PATCH /parsers HTTP/1.1" 500 2017
117.87.158.36 - - [30/12/2019:21:37:34 +0000] "POST /fieldsets HTTP/1.1" 500 4056
212.153.128.212 - - [30/12/2019:21:37:42 +0000] "PUT /fieldsets HTTP/1.1" 500 3259
58.188.83.217 - - [30/12/2019:21:37:46 +0000] "POST /playbooks HTTP/1.1" 500 3947
193.123.131.146 - - [30/12/2019:21:37:47 +0000] "PUT /lists HTTP/1.1" 500 3902
182.7.179.91 - - [30/12/2019:21:37:48 +0000] "GET /users HTTP/1.1" 500 2950
10.25.168.164 - - [30/12/2019:21:38:05 +0000] "PATCH /playbooks HTTP/1.1" 500 3858
215.201.210.173 - - [30/12/2019:21:38:11 +0000] "PATCH /customers HTTP/1.1" 500 3277
179.241.103.167 - - [30/12/2019:21:39:23 +0000] "POST /lists HTTP/1.1" 500 3669
147.188.170.252 - - [30/12/2019:21:39:33 +0000] "POST /fieldsets HTTP/1.1" 500 3313
1.249.123.189 - - [30/12/2019:21:39:36 +0000] "PATCH /alerts HTTP/1.1" 500 4734
124.114.135.105 - - [30/12/2019:21:39:41 +0000] "PATCH /customers HTTP/1.1" 500 3348
35.215.100.202 - - [30/12/2019:21:39:43 +0000] "PUT /alerts HTTP/1.1" 500 4345
86.222.23.128 - - [30/12/2019:21:39:44 +0000] "PATCH /alerts HTTP/1.1" 500 2230
20.161.75.95 - - [30/12/2019:21:39:48 +0000] "DELETE /users HTTP/1.1" 500 2412
196.18.151.117 - - [30/12/2019:21:39:55 +0000] "PUT /events HTTP/1.1" 500 4439
77.101.138.151 - - [30/12/2019:21:39:57 +0000] "PUT /lists HTTP/1.1" 500 2194
208.205.133.127 - - [31/12/2019:21:35:17 +0000] "DELETE /alerts HTTP/1.1" 500 4561
20.145.255.91 - - [31/12/2019:21:35:30 +0000] "GET /parsers HTTP/1.1" 500 3051
91.66.134.13 - - [31/12/2019:21:35:53 +0000] "POST /lists HTTP/1.1" 500 3319
31.88.211.206 - - [31/12/2019:21:35:57 +0000] "DELETE /events HTTP/1.1" 500 2325
171.37.114.114 - - [31/12/2019:21:37:39 +0000] "DELETE /fieldsets HTTP/1.1" 500 3253
223.157.242.167 - - [31/12/2019:21:37:59 +0000] "POST /users HTTP/1.1" 500 2283
98.181.102.34 - - [31/12/2019:21:38:06 +0000] "PATCH /fieldsets HTTP/1.1" 500 4672
254.74.22.79 - - [31/12/2019:21:38:06 +0000] "PUT /lists HTTP/1.1" 500 2259
103.46.238.3 - - [31/12/2019:21:38:09 +0000] "PUT /lists HTTP/1.1" 500 3992
41.62.205.107 - - [31/12/2019:21:39:20 +0000] "PATCH /alerts HTTP/1.1" 500 3631
22.53.105.197 - - [31/12/2019:21:39:31 +0000] "DELETE /collectors HTTP/1.1" 500 4202
178.22.133.42 - - [31/12/2019:21:39:43 +0000] "PUT /alerts HTTP/1.1" 500 4648
102.247.13.50 - - [31/12/2019:21:39:55 +0000] "PATCH /auth HTTP/1.1" 500 3736
171.37.114.114 - - [31/12/2019:21:37:39 +0000] "DELETE /fieldsets HTTP/1.1" 500 3253
223.157.242.167 - - [31/12/2019:21:37:59 +0000] "POST /users HTTP/1.1" 500 2283
98.181.102.34 - - [31/12/2019:21:38:06 +0000] "PATCH /fieldsets HTTP/1.1" 500 4672
254.74.22.79 - - [31/12/2019:21:38:06 +0000] "PUT /lists HTTP/1.1" 500 2259
103.46.238.3 - - [31/12/2019:21:38:09 +0000] "PUT /lists HTTP/1.1" 500 3992
41.62.205.107 - - [31/12/2019:21:39:20 +0000] "PATCH /alerts HTTP/1.1" 500 3631
22.53.105.197 - - [31/12/2019:21:39:31 +0000] "DELETE /collectors HTTP/1.1" 500 4202
178.22.133.42 - - [31/12/2019:21:39:43 +0000] "PUT /alerts HTTP/1.1" 500 4648
102.247.13.50 - - [31/12/2019:21:39:55 +0000] "PATCH /auth HTTP/1.1" 500 3736
```

***

</details>

[Up](#up)

### Task 3

You have a database of taxi rides. According to the plan, 10,550 vehicles were supposed to enter the service line — this figure covers user demand. The team received many complaints — there were not enough available cars. How many taxis actually got on the line? Information about all the cars on the line is in the `cabs' table.
1. Log in to the remote server.
2. Connect to the `chicago_taxi` database.
3. Calculate the total number of cars in the `cabs' table.

In the response, attach:
1. Number of cars
2. The request that you managed to solve the problem.

### Solution

1. Number of cars: 5529.
2. Request:

```bash
SELECT
  COUNT(*) AS cnt
FROM cabs;
```

[Up](#up)

### Task 4

Count the number of cars in each company from the `cabs' table. Sort the values in descending order. The team suggests that some companies have not put enough cars on the line.

Bring out those companies with less than 100 cars. Name the field with the number of cars `cnt`, and the field with the company name `company_name'.

To solve the problem, use the HAVING operator, an analog of WHERE for aggregating functions.

In the response, attach:
1. The list of companies with fewer than 100 cars.
2. The query that you managed to solve the problem.

### Solution

<details>

<summary>1. List of companies with fewer than 100 cars.</summary>

***

```bash
                 company_name                 | cnt 
----------------------------------------------+-----
 Nova Taxi Affiliation Llc                    |  97
 Patriot Taxi Dba Peace Taxi Associat         |  89
 Blue Diamond                                 |  85
 Checker Taxi Affiliation                     |  81
 Chicago Medallion Management                 |  80
 Chicago Independents                         |  69
 24 Seven Taxi                                |  67
 Checker Taxi                                 |  60
 American United                              |  55
 Chicago Medallion Leasing INC                |  53
 Top Cab Affiliation                          |  49
 KOAM Taxi Association                        |  48
 Chicago Taxicab                              |  38
 Norshore Cab                                 |  34
 Gold Coast Taxi                              |  20
 Metro Group                                  |  20
 Service Taxi Association                     |  18
 5 Star Taxi                                  |  14
 American United Taxi Affiliation             |   8
 Metro Jet Taxi A                             |   8
 Setare Inc                                   |   7
 Leonard Cab Co                               |   5
 4615 - 83503 Tyrone Henderson                |   1
 5062 - 34841 Sam Mestas                      |   1
 4623 - 27290 Jay Kim                         |   1
 5997 - 65283 AW Services Inc.                |   1
 2092 - 61288 Sbeih company                   |   1
 1469 - 64126 Omar Jada                       |   1
 2733 - 74600 Benny Jona                      |   1
 2192 - 73487 Zeymane Corp                    |   1
 5006 - 39261 Salifu Bawa                     |   1
 3556 - 36214 RC Andrews Cab                  |   1
 3721 - Santamaria Express, Alvaro Santamaria |   1
 2809 - 95474 C & D Cab Co Inc.               |   1
 2241 - 44667 - Felman Corp, Manuel Alonso    |   1
 3620 - 52292 David K. Cab Corp.              |   1
 2823 - 73307 Lee Express Inc                 |   1
 6057 - 24657 Richard Addo                    |   1
 6742 - 83735 Tasha ride inc                  |   1
 1085 - 72312 N and W Cab Co                  |   1
 3591 - 63480 Chuks Cab                       |   1
 0118 - 42111 Godfrey S.Awir                  |   1
 6574 - Babylon Express Inc.                  |   1
 3094 - 24059 G.L.B. Cab Co                   |   1
 5874 - 73628 Sergey Cab Corp.                |   1
 6743 - 78771 Luhak Corp                      |   1
 5074 - 54002 Ahzmi Inc                       |   1
 3623 - 72222 Arrington Enterprises           |   1
 4053 - 40193 Adwar H. Nikola                 |   1
 Chicago Star Taxicab                         |   1
 3011 - 66308 JBL Cab Inc.                    |   1
```

***

</details>

2. Request:

```bash
SELECT
  company_name,
  COUNT(cab_id) AS cnt
FROM
  cabs
GROUP BY
  company_name
HAVING
  COUNT(cab_id) < 100
ORDER BY
  cnt DESC;
```

[Up](#up)

### Task 5

The taxi app calculates the cost of the trip. If the weather is good, the coefficient value is 1. If it is raining or stormy outside, the coefficient increases to 2. The team has a hypothesis that there is an error in the calculations of the coefficient. To check the coefficient calculation, the team needs a data sample: the developer can compare the coefficient with the data in the logs and fix the bug. Your task is to get a selection.

To do this:
1. Get a description of the weather conditions from the Weather Records table for each hour.
2. We divided all the watches into two groups according to the operator case: "Bad", that is, the "description" field contains the words rain or thunderstorm; "Good" for everyone else.
3. Additional name "weather conditions".

Two fields must be specified in the rating table - date and time (`ts') and 'weather conditions'.

Make a selection for the period from 2017-11-05 00:00 to 2017-11-06 00:00.

In the response, attach:
1. The received table with data for the specified period.
2. The request that succeeded in solving the problem.

### Solution

<details>
<short description>1. A table with data for the specified period.</short description>

***

``bash
ts | weather conditions 
---------------------+--------------------
2017-11-05 00:00:00 | Good
2017-11-05 01:00:00 | Bad
2017-11-05 02:00:00 | Good
 2017-11-05 03:00:00 | OK
 2017-11-05 04:00:00 | Bad
2017-11-05 05:00:00 | Bad
2017-11-05 06:00:00 | Good
 2017-11-05 07:00:00 | OK
 2017-11-05 08:00:00 | OK
 2017-11-05 09:00:00 | OK
 2017-11-05 10:00:00 | OK
 2017-11-05 11:00:00 | OK
 2017-11-05 12:00:00 | OK
 2017-11-05 13:00:00 | OK
 2017-11-05 14:00:00 | Bad
 2017-11-05 15:00:00 | OK
 2017-11-05 16:00:00 | Bad
2017-11-05 17:00:00 | Good
 2017-11-05 18:00:00 | Bad
2017-11-05 19:00:00 | Bad
2017-11-05 20:00:00 | Bad
2017-11-05 21:00:00 | Good
2017-11-05 22:00:00 | Good
 2017-11-05 23:00:00 | OK
 2017-11-06 00:00:00 | OK
```

***

</more details>

2. Request:

`Bash
to choose
  ts,
REGISTER
    IF THE DESCRIPTION IS LIKE "%rain%" OR "%storm%", THEN "Bad"
    OTHERWISE, "Good"
ENDS UP AS weather_conditions
from
weather records
where
the dates ARE BETWEEN "2017-11-05 00:00:00" AND "2017-11-06 00:00:00";
```

[check](#up)

### Task 6

After the software update, taxi companies began to report that the profits they receive do not match the data provided by the application. The development suggests that the problem may be in the data on the number of trips.

To determine if there is a bug, you need to get a sample with the number of trips of each taxi company for November 15 and 16, 2017.

1. Enter the `company name' field. One of the options for the trip is the name "city trip" and enter it.
2. Sort the responses received in the "trips_amount" field in descending order.

Tell me: what to do if there are `taxis` and `rides'. Apply aggregation functions with grouping. Don't forget to write a construction with a condition.

In the response, attach:
1. The received table with data for the specified period.
2. The request that succeeded in solving the problem.

### Solution

<details>
<short description>1. A table with data for the billing period.</short description>

***

`company name bash
| number of trips 
----------------------------------------------+--------------
Cabin Flash | 19558
 Taxi services | 11422
 Medallion Tenant Number | 10367
 Yellow Cab | 9888
 Yellow Taxi service | 9299
 Chicago Carriage Cab Corp | 9181
 City taxi service | 8448
 Sun Taxi | 7701
 Star North Management LLC | 7455
 Blue Ribbon Taxi Association, Inc.            |         5953
 Taxi Association "Choice" | 5015
 Globe Taxi | 4383
 Organization of dispatching taxi service | 3355
 Nova Taxi LLC | 3175
 Organization Patriot Taxi Dba Peace Taxi Associate | 2235
 Checker Taxi Organization | 2216
 Blue Diamond | 2070
 Chicago Management Medallion | 1955
 24 Seventh taxis | 1775
 Chicago Medallion Leasing INC | 1607
 Checker Taxi | 1486
 American United | 1404
 Chicago Independent Companies | 1296
 KOAM Taxi Association | 1259
 Chicago taxi | 1014
 Association of the best taxis | 978
 Gold Coast Taxi | 428
 Association of Service Taxis | 402
5-star taxis | 310
303 Taxis | 250
 Setare Inc | 230
Membership in American United Taxi | 210
 Leonard Cab Co | 147
Jet Taxi Metro Jet Taxi A | 146
 Norshore Cab | 127
6742-83735, Tasha ride, inc | 39
3591 - 63480 Taxi Chux | 37
 Omar Jada, 1469-64126 | 36
6743 - 78771 Luhak Corporation
| 33 0118 - 42111 Godfrey S.Avir
, 33-6574 - Babylon Express, Inc.                  |           31
 Chicago Star Taxi | 29
1085 - 72312 N & W Cab Co. | 29
2809 - 95474 C & D Cab Co., Inc.               | 29
2092 - 61288 Sbeih Company | 27
3011 - 66308 JBL Cab Inc.                    |           25
 3620 - 52292 David K. Cab Corp.              | 21
4615 - 83503 Tyrone Henderson | 21
3623 - 72222 Arrington Enterprises | 20
5074 - 54002 Ahzmi Inc | 16
2823 - 73307 Lee Express Inc | 15
4623-27290, Jay Kim |
15-3721, Santamaria Express, Alvaro Santamaria | 14
5006 - 39261 Salifu Bava | 14
2192 - 73487 Zeiman Corporation / 14
6057 - 24657 Richard Addo | 13
5997 - 65283 AW Services Inc.                |           12
 Metro Group | 11
5062 - 34841 Sam Mestas | 8
4053 - 40193 Advar H. Nicola | 7
2733 - 74600 Benny John | 7
5874 - 73628 Sergey Cab Corp.                | 5
2241 - 44667 - Felman Corporation, Manuel Alonso | 3
3556 - 36214 RC Andrews Cab | 2
```

***

</more details>

2. Request:

`Bash
to choose
  cabs.company_name AS the company name,
  COUNT(trips.trip_id) AS the number
of
taxi rides
DOMESTIC trips ARE JOINED BY trips.cab_id = cabs.cab_id
where
  SELECT(trips.start_ts AS date) BETWEEN "2017-11-15" AND "2017-11-16"
TO GROUP BY
company name
SORT BY
THE number OF trips DEPENDING ON;
```

[check](#up)

## <a name="test-automation" />Basics of Test automation

### Task 1

Automate the test case for Yandex.Routes. Find the necessary selectors on the stand: <https://qa-routes.praktikum-services.ru />

``
[CASE-1] Checking the name of the mode of transport in the result for a taxi

Steps:
1. Enter the "Start time of the trip" — 08:00.
2. In the "From Where" field: Usacheva, 3.
3. In the "Where" field: Komsomolsky Prospekt, 18.
4. Select the "Own" mode.
5. Choose the type of transport: taxi.

OP: The text of the result that appears begins with the word "Taxi".
```

### Solution

Selectors:

| Element | Selector |
|:----------|:----------|
| The "Clock" field | #form-input-hour |
| The "Minutes" field | #form-input-minute |
| The "From" field | #form-input-from |
| The "To" field | #form-input-to |
| Own mode | #form-mode-custom |
| Taxi transport | #from-type-taxi |
| Result string | #result-time-price |

Autotest:

```javascript
const puppeteer = require('puppeteer');

const URL_TEST = 'https://qa-routes.praktikum-services.ru/';

async function testTaxiResult() {
console.log('Browser launch');
    const browser = await puppeteer.launch({headless: false, slowMo: 100});

    console.log('Creating a new tab in the browser');
    const page = await browser.newPage();

    console.log('Click on the link');
    await page.goto(URL_TEST);

    console.log('Step 1: Entering hours and minutes');
    const hoursInput = await page.$('#form-input-hour');
    await hoursInput.type('08');

    const minutesInput = await page.$('#form-input-minute');
    await minutesInput.type('00');

    console.log('Step 2: filling in the From field');
    const fromInput = await page.$('#form-input-from');
    await fromInput.type('Usacheva, 3');

    console.log('Step 3: filling in the Where field');
    const toInput = await page.$('#form-input-to');
    await toInput.type('Komsomolsky Prospekt, 18');

    console.log('Step 4: Select Your Own mode');
    const routeMode = await page.$('#form-mode-custom');
    await routeMode.click();

    console.log('Step 5: choosing the mode of transport');
    const typeOfTransport = await page.$('#from-type-taxi');
    await typeOfTransport.click();

    console.log('Waiting for an element with a result');
    await page.waitForSelector('#result-time-price')

    console.log('Getting a result string');
    const text = await page.$eval('#result-time-price', element => element.textContent);

    console.log('Checking the test case condition');
        if (text.startsWith('Taxi')) {
        console.log('Success. The text contains: ' + text);
    } else {
          console.log(`Error. The text does not start with the word "Taxi")
}

    console.log('Closing the browser');
    await browser.close();
}

testTaxiResult();
```

[Up](#up)

### Task 2

Automate the test case for [ya.ru ](https://ya.ru ) by applying the necessary selectors.

```
[CASE-2] Checking the appearance of search results

Precondition:
Go to the page ya.ru .

Steps:
1. Enter "Test Automation" in the search bar.
2. Click the "Find" button.

OP: The user navigated to the search results page and the search result is not empty.
```

### Solution

Selectors:

| Element | Selector | 
|:----------|:----------|
| Search bar | #text |
| The "Find" button | .button[type=submit]    |
| Search result | .serp-item |

Autotest:

```javascript
const puppeteer = require('puppeteer');

async function testYaRu() {
console.log('Browser launch');
    const browser = await puppeteer.launch();

    console.log('Creating a new tab in the browser');
    const page = await browser.newPage();

    console.log('Going to the page ya.ru ');
    await page.goto('https://ya.ru/');

    console.log('Entering the text "Test automation" in the search bar');
    const searchField = await page.$('#text');
    await searchField.type('Test Automation');

    console.log('Click on the "Find" button');
    const searchButton = await page.$('.button[type=submit]');
    await searchButton.click();
    
    console.log('Waiting for the transition to the search results page');
    await page.waitForNavigation();
    
    console.log('Getting search result elements');
    const result = await page.$('.serp-item');
    
    console.log('Comparison of OP and FR');
    if (result == null) {
        console.log('No search results found');
    } else {
        console.log('The search results are displayed');
    }
    
    console.log('Closing the browser');
    await browser.close();
}

testYaRu();
```

[Up](#up)

