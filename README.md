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

Selected Colors = 
 "Showing " &
 CONCATENATEX ( 
    VALUES ( 'Product'[ColorName] ) ,
    'Product'[ColorName],
     ", ",
    'Product'[ColorName],
    ASC
 ) & " colors."

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

