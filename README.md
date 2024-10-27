# CONCATENATEX-Function
Do you want to learn how to put a filter range from slaicer on a visualization ? SEE THIS 🚀
----------------------
Czy chciałbyś wiedzieć, które pozycje ze slicera mają wpływ na twoją wizualizacje bez konieczności przeglądania wszystkich jego pozycji? 
To może być uciążliwe oraz czasochłonne zwłaszcza jeśli jesteś zmuszony użyć slicera typu "drop-down".

Z pomocą przychodzi Funkcja " CONCATENATEX " ✨ Pozwoli nam na wyświetlanie wszystkich zaznaczonych pozycji ze slaicera na naszej wizualizacji !!!

Before :  
 
![image](https://github.com/user-attachments/assets/6e5d0978-1269-4500-aaeb-0f1503a31d30)

After :

![image](https://github.com/user-attachments/assets/ac8d1037-09e1-421f-b764-e78499d63618)

-----------------------------

W pierwszej kolejności dla naszej przykładowej tabeli musimy stworzyć miarę, która będzie wyświetlać kolory ze slaicera.

![image](https://github.com/user-attachments/assets/adda96ee-3003-4380-87cc-9a0ee9b66c94)

Explanation of the Measure's Function:|

Selected Colors =

This is the name of the measure (Selected Colors) that we define in Power BI. The measure will return text describing the selected colors in the ColorName column.

"Showing " &

"Showing " is a text string that will appear at the beginning of the result. The & symbol in DAX is a concatenation (joining) operator, used to combine text. In this case, "Showing " is combined with the result of the CONCATENATEX function, which is explained below.

CONCATENATEX

The CONCATENATEX function is used to combine values from a column or table, placing a specified separator between them. The function’s syntax is:

CONCATENATEX (table, expression, separator, order by column, direction)

VALUES ( 'Product'[ColorName] )

VALUES returns unique values from the 'Product'[ColorName] column within the current filter context. In this case, VALUES ('Product'[ColorName]) creates a table with the unique colors of the products within the current visualization or filter. This provides a list of colors that have been selected or are currently visible in the visualization (e.g., in a table).

'Product'[ColorName] (second argument in CONCATENATEX)

This argument specifies the value to be used in creating the text string. Here, it is the value from the 'Product'[ColorName] column.

", " (third argument in CONCATENATEX)

", " is the separator that will be placed between values from ColorName while combining them. This way, the colors will be separated by a comma and a space, e.g., "Red, Blue, Green".

'Product'[ColorName], ASC (fourth and fifth arguments in CONCATENATEX)

These arguments specify that the colors should be sorted by the values in the 'Product'[ColorName] column in ascending (alphabetical) order (ASC). This ensures that the colors appear in alphabetical order.

& " colors."

& again works as the concatenation operator, joining the result of CONCATENATEX with another text string. " colors." is the text that will be added at the end of the result. The final output will look like: "Showing Red, Blue, Green colors."

Miarę możemy umieścić w dowolnym miejscu, np. w tytule wykresu tworząc dynamiczny tytuł czy w polu tekstowym umieszczonym w dowolnym miejscu na stronie raportu. 
W naszym przykładzie użyliśmy dynamicznego tytułu.

Oto jak go zaimplementować na naszej wizualizacji. 

![image](https://github.com/user-attachments/assets/4e5165c0-7796-47ed-8f7c-22094fe3ab32)

![image](https://github.com/user-attachments/assets/42d1b789-e297-4d1d-87b8-9b7ebdb02cbd)

Jedyną wadą naszej miary może być fakt, że jeśli nie będzie żadnego wybranego koloru, wygeneruje ona długą listę która będzie zabierać dużo miejsca na naszej wizualizacji. Co więcej, w przypadku wyboru więcej niż 5/6 kolorów lista będzie tak czy inaczej zbyt długa, aby zadowolić użytkownika.

Example :

![image](https://github.com/user-attachments/assets/3303e1db-230c-411f-ab0a-d4df112006ae)

Na to również posiadam rozwiązanie !!! 🚀
musimy nieco skomplikować nasz kod aby uwzględnić omawiane wyżej możliwości. 

Result :

![image](https://github.com/user-attachments/assets/d556f19c-80a9-4447-afc4-6e1cc77d493f)

![image](https://github.com/user-attachments/assets/a1365bd6-78d1-4c24-8130-2f3012f8bdda)

![image](https://github.com/user-attachments/assets/62406abc-8d2f-497f-87d1-f002190f5eed)


New code : 


![image](https://github.com/user-attachments/assets/afd538a2-c07b-4b8d-a52c-42f84c763c3b)


Explanation of the Measure's Function:
Selected Colors v2 =

This is the name of the measure. This measure will return text describing the selected colors based on the number of selected colors and other conditions.

Variables

VAR Colors = VALUES ( 'Product'[ColorName] )

Creates a variable Colors that stores the unique values from the 'Product'[ColorName] column within the current filter context.
VALUES returns unique values from the column, meaning that Colors contains a list of colors that are currently selected or shown in the visualization.



VAR NumberOFcolors = COUNTROWS ( Colors )

Creates a variable NumberOFcolors, which stores the number of selected colors.
COUNTROWS (Colors) counts the rows in Colors, meaning it counts the unique selected colors.

VAR NumberOFALLcolors = COUNTROWS ( ALL ( 'Product'[ColorName] ) )

Creates a variable NumberOFALLcolors, which stores the total number of available colors in the 'Product' table, ignoring the current filter context.
ALL('Product'[ColorName]) removes all filters on the ColorName column, so COUNTROWS counts all possible colors, even those that are not currently selected.


VAR ALLcolorsSelected = ( NumberOFcolors = NumberOFALLcolors )

Creates a boolean variable ALLcolorsSelected, which is TRUE if the number of selected colors (NumberOFcolors) is equal to the total number of available colors (NumberOFALLcolors).
This variable checks if all colors are selected. If they are, ALLcolorsSelected will be TRUE; otherwise, it will be FALSE.

VAR SelectedColors =
    CONCATENATEX (
        Colors,
        'Product'[ColorName],
        ", ",
        'Product'[ColorName],
        ASC
    )

VAR SelectedColors = CONCATENATEX (...)
Creates a variable SelectedColors that stores a sorted list of the selected colors as a single text string, with the colors separated by commas.
CONCATENATEX combines all values in Colors, using ", " as the separator.
The colors are sorted alphabetically (ASC) based on the column 'Product'[ColorName].

Result

VAR Result =
    IF (
        ALLcolorsSelected,
        "Showing all colors.",
        IF (
             NumberOFcolors > 5,
             "More than 5 colors selected, see slicer page for details.",
             "Showing " & SelectedColors & " colors."
        )
    )

VAR Result = IF (...)
Creates a variable Result that stores the final output text, based on the number of selected colors and the total number of colors. The IF function works as follows:
IF (ALLcolorsSelected, "Showing all colors.", ...)
Checks if ALLcolorsSelected is TRUE (meaning all available colors are selected).
If ALLcolorsSelected is TRUE, Result will be "Showing all colors.", and the check ends.
IF (NumberOFcolors > 5, "More than 5 colors selected, see slicer page for details.", ...)
If ALLcolorsSelected is FALSE, it goes to the next condition to check if the number of selected colors is greater than 5.
If more than 5 colors are selected, Result will be "More than 5 colors selected, see slicer page for details.", which suggests the user should check the slicer page for full details.
"Showing " & SelectedColors & " colors."
If ALLcolorsSelected is FALSE and the number of selected colors is 5 or less, the measure returns "Showing " & SelectedColors & " colors.".
It combines the text "Showing ", the list of selected colors (SelectedColors), and the text " colors.", resulting in something like "Showing Red, Blue, Green colors.".

Returning the Result

RETURN
    Result

The RETURN function outputs the value of Result as the final result of the measure.
Depending on the conditions, the result can be one of the following:
"Showing all colors." – if all colors are selected,
"More than 5 colors selected, see slicer page for details." – if more than 5 colors are selected,
"Showing [list of colors] colors." – if 5 or fewer colors are selected, displaying them in alphabetical order.


As you can see, we are gradually raising our bar on data visualization ✨🚀 
I hope you enjoyed it: )

That's all in this article !

I hope you will use this technique and show it off at work : )

Greetings, Mateusz Rajca



