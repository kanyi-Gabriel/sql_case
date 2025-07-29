# 🎧 SQL Practice – (Chinook Database)

## 📌 Overview
This project is part of my **SQL learning journey** using the **Chinook SQLite database**.  
On this day, I moved beyond basic joins and aggregations to explore **advanced SQL techniques** such as:

- ✅ **Common Table Expressions (CTEs)**
- ✅ **Window Functions** (ranking, running totals)
- ✅ **CASE Statements** for conditional logic


---

## 🗂️ Database Used
- **Chinook SQLite** (download from [Chinook Database GitHub](https://github.com/lerocha/chinook-database))

---

## 🧠 TODAYS SQL Challenges
The queries written on Day 3 answer the following questions:

1. 🏆 **Top-selling artist in each country** (using CTE + Window Functions)  
2. 📈 **Running total of sales per year** (using Window Functions)  
3. 🎵 **Classify tracks as 'Short', 'Medium', or 'Long'** (using CASE Statements)  
4. 🌍 **Most popular genre for each country** (using CTE + Ranking)

---

## 💻 Sample Query (CTE + Window Function)
```sql
with ArtistSale as (
    SELECT 
        a.name as Name,
        round(sum(item.UnitPrice * item.Quantity), 3) as Total_sales,
        c.Country
    FROM 
        artists as a
    JOIN
        albums as al
    ON 
        a.ArtistId = al.ArtistId
    JOIN
        tracks as t
    ON
        al.AlbumId = t.AlbumId   
    JOIN
        invoice_items as item
    ON
        t.TrackId = item.TrackId  
    JOIN
        invoices as i
    ON 
        item.InvoiceId = i.InvoiceId
    JOIN
        customers as c
    ON
        i.CustomerId = c.CustomerId
    GROUP BY 
            a.name, 
            c.Country
)
SELECT 
    Name,
    country,
    Total_sales,
    Ranking
FROM
    (
        SELECT Name,
        country,
        Total_sales,
        DENSE_RANK() OVER(PARTITION BY Country ORDER BY Total_sales DESC) as Ranking
        FROM ArtistSale
        
    ) ranked

WHERE Ranking = 1
ORDER BY Total_sales DESC
;
