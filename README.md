**Data Audit Challenge: "The Lying Report"**

In this project, I take on the role of a Data analyst who inherits a crucial 2025 Sales Report from a former employee. The report is scheduled to be presented to the board in two days. The problem? The report is full of hidden errors.

My task is to audit the provided Excel file, identify the mistakes, fix them, and provide the management with the ture number

The original report contains multiple hidden errors, formulas, and business logic. Some of these errors completely distort the final business conclusions.

My goals for this project are:
1. Analyze the raw data and find the hidden errors.
2. List each error, explain why it is wrong, and describe how I found it.
3. Rebuild the report to calculate the metrics correctly.
4. Write an executive memo to the boss with the real numbers and updated business insights.


Tools & Skills Demonstrated:
- Microsoft Excel
- Data Auditing 
- Data cleaning 
- Business Logic Verification
- Pivot tables
- Executive Communication


Dataset Overview

<img width="770" height="876" alt="image" src="https://github.com/user-attachments/assets/1f803d01-becf-4eb0-95ae-ceb2c1648c39" />

This sheet contains the final summary prepared by the previous analyst. It includes aggregated metrics like Total Net Revenue, Number of Transactions, and Sales by City.


<img width="1502" height="917" alt="image" src="https://github.com/user-attachments/assets/f1fc3306-2b12-44f3-b253-3ed6b874e53b" />
The core dataset containing 14,998 rows of sales transactions from 2025.

- ID_transakcji - Unique transaction identifier
- Data - Date and time of the transaction
- Sklep - Store location
- Kategoria - Product category
- ID_produktu - Unique product code
- Nazwa produktu - Name of the product
- Ilość - Number of items bought
- Cena_jednostkowa - Price per single item
- Rabat_% - Discount applied to the transaction
- Wartość_netto - Final transaction value after discount
- Status - Order status
- Koszt - Total cost of goods for the transaction


  <img width="1061" height="891" alt="image" src="https://github.com/user-attachments/assets/ccd37afa-188c-472f-a68a-519ca7ec52da" />

The official catalog used to verify pricing and costs.
- ID_produktu - Product code matching the transaction data
- Nazwa_produktu - Name of the product
- Kategoria - Category
- Cena_katalogowa - Official standard price
- Koszt_jednostkowy - Official unit cost




<img width="1025" height="906" alt="image" src="https://github.com/user-attachments/assets/ed393915-5f83-443a-a6dc-36786f886da3" />

An additional catalog, likely for new releases like e-bikes: 


<img width="775" height="920" alt="image" src="https://github.com/user-attachments/assets/fd4a1956-e8c6-49ef-ad6a-bd892126354c" />

A small lookup table containing the 2025 revenue targets for specific cities.


**Discovered Errors & Fixes**
1. I began searching for errors in the data sheet. The formula was missing a fourth parameter, which defaults to TRUE—signifying an "approximate match." Consequently, if the value being searched for is not present in the table, Excel makes a "guess" which can lead to errors.
<img width="1338" height="195" alt="image" src="https://github.com/user-attachments/assets/c5270781-3ef6-46ec-b63b-e2bd60920ed7" />
That is why I changed this value to FALSE:
<img width="1340" height="218" alt="image" src="https://github.com/user-attachments/assets/3e58366e-7780-488e-868b-a9a87158764e" />
2. Because of this, I noticed another error. Several rows in the 'Koszt' column started displaying an #N/A error.
<img width="1370" height="746" alt="image" src="https://github.com/user-attachments/assets/c1c3c22f-95a1-4717-b39d-654f5e78b4f1" />
I realized that this error occurs with new products from the "Produkty_nowe" sheet.
I fixed this error using the "JEŻELI.BŁĄD"
<img width="1485" height="382" alt="image" src="https://github.com/user-attachments/assets/f3434797-2414-4f7c-b8d7-a2100673c9ee" />
3. After fixing the formulas, I checked whether the "ID_transakcji" column contains any duplicates. To fix this, I selected the entire dataset in the 'Dane' sheet and used Excel's "Remove Duplicates" feature, checking strictly by the 'ID_transakcji' column. This removed the redundant rows and brought the metrics back to reality.
<img width="1352" height="712" alt="image" src="https://github.com/user-attachments/assets/ae887872-4395-4052-afc9-65c3f95dc20b" />

It turned out there were 178 duplicates
From a business perspective, this is a serious error. Duplicates cause numerous transactions to artificially inflate both revenue and expenses, which may lead management to make poor decisions.


<img width="460" height="245" alt="image" src="https://github.com/user-attachments/assets/ae1015f9-7fb0-4a2b-8312-9b88396b89e1" />

4. Next, I checked the "report" sheet. In this case, the revenue for 2025 is calculated using a SUM function, which is incorrect because this method includes cancelled transactions.

<img width="543" height="370" alt="image" src="https://github.com/user-attachments/assets/7f337c31-217c-4833-af0e-004c1de6ad8b" />

To fix this, I used to SUMA.JEŻELI function and set the criterion to "Zrealizowane", so it only counts completed transactions.

<img width="886" height="367" alt="image" src="https://github.com/user-attachments/assets/06fa98f7-8f5b-4237-9d59-a7a5e321bb84" />

5. The number of transactions is also calculated incorrectly. It includes unexecuted transactions.

<img width="702" height="376" alt="image" src="https://github.com/user-attachments/assets/9f20f099-bb6d-4655-b94c-9d1da302f760" />

To fix this, I used the 'LICZ.JEŻELI' function to count only the rows where the order status is  marked as "Zrealizowana" This prevents the number of transactions from being overstated due to unrealized transactions.

<img width="745" height="385" alt="image" src="https://github.com/user-attachments/assets/158a92a9-9890-48ad-8ab0-9e98c46720cf" />

6. Averaging Averages
I noticed that the "Average transaction value" was calculated based on the averages for each store. what causes a false result. This is a classic statistical fault known as "averaging averages." It completely ignores the volume (weight) of transactions in those sub-categories. A store with 100 sales was given the same mathematical weight as a store with 5,000 sales, leading to a highly distorted overall average.

<img width="730" height="701" alt="image" src="https://github.com/user-attachments/assets/9fac9580-a1de-4d5a-a581-8683b130a5eb" />

I discarded that formula and simply divided the net revenue by the number of transactions.

<img width="703" height="385" alt="image" src="https://github.com/user-attachments/assets/3a5b6de4-e652-427a-85b6-8fc7f1f87f94" />

7. While verifying the "cel roczny sieci", I noticed the value in cell B8 was hardcoded as '13 500 000' instead of dynamically pulling from the official 'cele' sheet.
   
<img width="742" height="392" alt="image" src="https://github.com/user-attachments/assets/78374c22-2ab2-43b7-8c35-43e8eb5d0d86" />
 
 I deleted the hardcoded number and replaced it with a dynamic 'SUMA' formula
 
 <img width="732" height="350" alt="image" src="https://github.com/user-attachments/assets/d410a9f3-12f6-47cd-b93b-18561b00ce98" />

 8. Including Cancelled Orders in "Koszt sprzedanych towarów".  It was calculated using a simple 'SUMA' function, which wrongly included the costs of cancelled orders.

<img width="747" height="416" alt="image" src="https://github.com/user-attachments/assets/bf7df377-9adb-430e-b34b-33b4ed6957fb" />

This is an error in financial reporting, as it artificially inflates costs. I replaced the 'SUMA' function with a 'SUMA.JEŻELI' formula to ensure we only sum the costs for transactions explicitly marked as "Zrealizowana"

<img width="905" height="387" alt="image" src="https://github.com/user-attachments/assets/44f7e3ab-10ce-43aa-8e70-85e6b6ef3d62" />

9. Confusing Margin with Markup. When reviewing the "Marża" indicator, I noticed the formula used was (B5-B10)/B10. So, in this case, we subtract the cost from the revenue and divide by the cost. This means that this is the formula for the markup.

<img width="725" height="371" alt="image" src="https://github.com/user-attachments/assets/ffda59f1-9570-46cb-99e3-b4e30e5fa647" />


I corrected the formula to '=(B5-B10)/B5', which correctly divides the profit by the total revenue and we have calculated the correct margin.

<img width="700" height="360" alt="image" src="https://github.com/user-attachments/assets/570d851f-24c0-4357-8421-2f1e4538b3e1" />


10. Excluding Boundary Dates. While verifying the "Przychód Q4" metric, I discovered a critical double fault in the "SUMA.WARUNKÓW) formula. Flawed Logical Operators: By using strictly greater than (`>`) and less than (`<`), it completely excludes all transactions made on exactly October 1st and December 31st. Also It sums all transactions within the date range, bringing back the issue of including "Anulowana" orders into the revenue stream.
<img width="1317" height="392" alt="image" src="https://github.com/user-attachments/assets/942ff1c5-38b7-4bd9-9b2c-ee9266e23901" />

I rewrote the formula to include a status condition "Zrealizowana". I also fixed the date boundaries by using '>=DATA(2025,10,1)' and <DATA(2026,1,1) The second ensures that any transactions with late-day timestamps on December 31st  (e.g., `23:50:00`) are perfectly captured.

<img width="1616" height="380" alt="image" src="https://github.com/user-attachments/assets/979df8dd-2157-493d-bb06-04992171823f" />

11. While verifying the "Sprzedaż wg sklepów". I caught a critical error in the 'SUMA.WARUNKÓW' formula. The previous analyst correctly locked the criteria range '$C$2:$C$14999' using absolute references, but left the sum range 'J2:J14999' relative. As a result, when the formula was dragged down to calculate subsequent stores, the sum range shifted downwards (e.g., to `J3`, `J4`), silently dropping valid transactions from the top of the dataset. Furthermore, the formula once again failed to filter out "Anulowana" (Cancelled) orders.
    
<img width="1130" height="638" alt="image" src="https://github.com/user-attachments/assets/ac068ce2-5ee9-4d8b-b69e-7d0e3e22015a" />

I fully locked the sum range with '$' signs '$J$2:$J$14821' to freeze it in place. I also added the necessary condition to only sum transactions marked as "Zrealizowana", and applied the corrected formula to all stores.

<img width="1262" height="706" alt="image" src="https://github.com/user-attachments/assets/a04c1a84-9d2c-4826-b29f-76cbe9850e48" />


12. While checking the total revenue against the store breakdown, I found they didn't match. Looking closely at the data, there were two entries for Krakow—one spelled with the Polish accent (Kraków) and one without (Krakow). Because the formula was only looking for "Kraków," all the sales listed under "Krakow" were left out of the total.

<img width="327" height="677" alt="image" src="https://github.com/user-attachments/assets/5fe83631-8c61-40a7-a696-b672280cbfaa" />

I used Excel's Find & Replace tool to standardize all entries in the Store column to the correct spelling "Kraków".

<img width="1106" height="678" alt="image" src="https://github.com/user-attachments/assets/d47db45e-96fc-4bab-9b38-01dba62e4a1c" />

13. After fixing the Kraków entries, the totals still didn’t match. Total net revenue (B5) was still 244,849.65 PLN higher than the sum of the store table (B29). That meant another group of rows was being counted in the total but not under any store. This time the filter showed nothing unusual. Every store appeared only once. To find the missing rows, I added a helper column in the Dane sheet that checked if each transaction’s store name was on the store list in the report: =LICZ.JEŻELI(Raport!$A$23:$A$28;C2)=0

<img width="1880" height="812" alt="image" src="https://github.com/user-attachments/assets/f510a4b9-af17-495c-a626-de0c2b2afa46" />

I used a filter and selected "TRUE"; it turned out that the error was the word "Gdańsk" written with a space at the end.

<img width="1877" height="775" alt="image" src="https://github.com/user-attachments/assets/e750bb3a-559f-4a9c-8263-dc0ffe69d21b" />

I used Find & Replace to change “Gdańsk “ to “Gdańsk”, and after that B5 and B29 matched.


14. Next, I checked the "liczba transakcji" once again, there was no condition that would allow for selecting only completed transactions.

<img width="722" height="696" alt="image" src="https://github.com/user-attachments/assets/96615c2d-0d33-4463-99e0-120d52cac40a" />

I added a second condition to the formula so it only counts transactions with the status “Zrealizowana”

<img width="1142" height="695" alt="image" src="https://github.com/user-attachments/assets/3e74e70b-f306-4efb-82ee-898dc6debb71" />

15. When reviewing the "Udział w przychodzie" column, I noticed the formula '=JEŻELI.BŁĄD(B23/B29;"-")'. The total share summed up to an impossible 242.4%, and several stores displayed a missing value dash ("-").

<img width="1140" height="262" alt="image" src="https://github.com/user-attachments/assets/ff06335b-ade4-4e3b-b5b2-0650974328c1" />

The previous analyst failed to use absolute references for the denominator (Total Revenue in B29). When dragged down, the relative reference shifted into empty cells, causing errors.  I anchored the denominator with absolute references and applied it down the column. This exposed the true percentage shares for all missing stores and brought the grand total to 100%.

16. In the "Sprzedaż i marża wg kategorii" table, for both the Revenue and Cost columns, I added a condition to include only completed "Zrealizowana" orders. I did this in the exact same way as in the "Sprzedaż wg sklepów" table, using the 'SUMA.WARUNKÓW' function.
    
<img width="1265" height="471" alt="image" src="https://github.com/user-attachments/assets/aaf85e8d-86c7-4bbd-b11a-c39adca390fc" />

17. found the exact same margin error here as in the main table. The formula was calculating markup instead of margin. I fixed it to divide profit by revenue instead of cost (=(B33-C33)/B33).
<img width="715" height="582" alt="image" src="https://github.com/user-attachments/assets/37bff5d5-980b-4fd7-84f1-9b46d01a6081" />
<img width="720" height="578" alt="image" src="https://github.com/user-attachments/assets/454cd7a1-1f7a-45f0-bebf-fdc8febf3eb7" />

18. The "Sprzedaż wg miesiący" table contained hardcoded values pasted from an old pivot table. Because the data was static, it didn't update after cleaning the raw data (like removing duplicates) and showed an inflated total of over 14.2M.

<img width="697" height="322" alt="image" src="https://github.com/user-attachments/assets/74f8b6a8-54d6-4f20-96eb-328d2997151b" />

I fixed this by creating a helper month column in the raw dataset (=MIESIĄC()) and replacing the static numbers with a dynamic SUMA.WARUNKÓW formula, filtering by month and the "Zrealizowana" order status.
<img width="1220" height="747" alt="image" src="https://github.com/user-attachments/assets/7d836bce-be55-4594-b2b3-0c84af5c4ddd" />

19. The original "Conclusions" section was driven by flawed data and broken formulas, leading to dangerous business recommendations.

<img width="1115" height="160" alt="image" src="https://github.com/user-attachments/assets/c2d5938c-663f-4dcd-8351-435c42c4af95" />
<img width="1657" height="285" alt="image" src="https://github.com/user-attachments/assets/51555b25-3edc-4894-a3ab-6211f7aefe7d" />

**Project Summary:**
The old report showed completely wrong results due to dirty data, broken formulas, and hardcoded numbers. Because of this, management was drawing false conclusions and planning harmful business decisions.

What I Fixed:
- I removed duplicates and fix typos (e.g., changing "Krakow" to "Kraków").
- I replaced the wrong markup formula (profit/cost) with the correct margin calculation (profit/revenue).
- I added locked ranges ($) and filtered out cancelled orders in all 'SUMA.WARUNKÓW' functions.
- I removed hardcoded numbers and linked all tables to dynamic formulas that pull data directly from the source.

Business Impact:
- The real e-bike margin is 31.9%, not a crazy 600%. This saved the company from buying unnecessary stock.
- I proved that the Gdańsk store (which was going to be penalized for the "worst result") is actually the best location and the only one that hit its target (100.8%).
- Management now sees the actual target completion at 92.4% (instead of a fake 112%). The file is now a fully dynamic, automated tool.

The corrected, fully working file is available in this repository: [Raport_ktory_klamie corrected.xlsx](<Raport_ktory_klamie corrected.xlsx>). The original version is also included for comparison: [Raport_ktory_klamie.xlsx](Raport_ktory_klamie.xlsx).
