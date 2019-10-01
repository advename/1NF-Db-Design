## Initial data
To make the next steps easier to follow, we minimized our previous the E-R model to this:
![k5Q8vgL.png](https://i.imgur.com/k5Q8vgL.png)

Based on the new (minimized) E-R model and our Spotify Album, we tabulate the data:

| album_name | album_year | artist_name     | artist_genre | song_name                             | song_duration |
|------------|------------|-----------------|--------------|---------------------------------------|---------------|
| Palm Beach | 2019       | Kanye West      | Rap, Hip Hop | Stronger, Heartless                   | 05:12, 3:12   |
| Palm Beach | 2019       | Drake           | Rap, Hip Hop | God's plan, Hold On, We're Going Home | 3:19, 3:48    |
| Palm Beach | 2019       | Micheal Jackson | Pop, Funk    | P.Y.T,You Rock My World               | 3:59, 5:38    |

## 1NF : First normal form

To satisfy the first normal form, a table has to meet following conditions:
1. Each data value (field) should only contain a single value (is atomic)
2. Identify each set of related data with a primary key
3. Each column name should be unique
4. The order in which data is stored, does not matter (aka rows and columns can be messed up)

Our current table satisfies rule 2, 3 & 4 but violates the first condition - we have to separate duplicities into multiple rows.

|  album_name | album_year | artist_name     | artist_genre_1 | artist_genre_2 | song_name_1 | song_duration_1 | song_name_2               | song_duration_2 |
|------------|------------|-----------------|----------------|----------------|-------------|-----------------|---------------------------|-----------------|
|  Palm Beach | 2019       | Kanye West      | Rap            | Hip Hop        | Stronger    | 05:12           | Heartless                 | 03:12           |
|Palm Beach | 2019       | Drake           | Rap            | Hip Hop        | God's plan  | 03:19           | Hold On, We're Going Home | 03:48           |
|  Palm Beach | 2019       | Micheal Jackson | Pop            | Funk           | P.Y.T       | 03:59           | You Rock My World         | 05:38           |

After restructuring the table, every field contains a single value and each row contains a unique set of values and can be determined by using a composite key (primary key) made of *album_name* and *artist_name*. 
> E.g. What if i duplicate the first row and change the *album_name* from Palm Beach to Flower Power? 
> 
> |  album_name | album_year | artist_name     | artist_genre_1 | artist_genre_2 | song_name_1 | song_duration_1 | song_name_2               | song_duration_2 |
|------------|------------|-----------------|----------------|----------------|-------------|-----------------|---------------------------|-----------------|
|  Palm Beach | 2019       | Kanye West      | Rap            | Hip Hop        | Stronger    | 05:12           | Heartless                 | 03:12           |
|Palm Beach | 2019       | Drake           | Rap            | Hip Hop        | God's plan  | 03:19           | Hold On, We're Going Home | 03:48           |
|  Palm Beach | 2019       | Micheal Jackson | Pop            | Funk           | P.Y.T       | 03:59           | You Rock My World         | 05:38           |
|Flower Power | 2019       | Drake           | Rap            | Hip Hop        | God's plan  | 03:19           | Hold On, We're Going Home | 03:48           |
>
> - *album_name* as the unique identifier would not suffice
> - *artist_name* as the unique identifier would not suffice
> - combining *album_name* and *artist_name* suffices as the unique identifier -> composite key
> 
> Hence we can use *album_name* and *artist_name* to fetch any row of data from this table.
> In this table all columns **depends** can be fetched by using *album_name* and *artist_name*. This is also called **dependency**.

The 1NF is satisifed.

#### UNF : Unormalized form
Although now the table formally complies to the 1NF (is atomic), the problem with this solution is obvious - if an album has more than three *artist_genres* or *song_names* it cannot be added to the database without altering its structure. Moreover, the *album_name* contains a repetitive value. 
Essentially, an unnormalized table is a table contains repeating values or columns.

#### Better approach
To solve the problem in a more elegant way, it is necessary to identify entities represented in the table and separate them into their own respective tables. In this case, it would result in **Album**, **Artist**, **Artist Genre** and **Song** tables.

**Album table**

| album_name | album_year |
|------------|------------|
| Palm Beach | 2019       |

**Artist table**

| artist_name     |
|-----------------|
| Kanye West      |
 | Drake           |
 | Micheal Jackson |

**Artist genre table**

| genre |
|-------------|
| Rap         |
 | Hip Hop     |
 | Pop         |
 | Funk        |

**Song table**

 | song_name                 | song_duration |
|---------------------------|---------------|
 | Stronger                  | 05:12         |
  | Heartless                 | 03:12         |
 | God's plan                | 03:19         |
 | Hold On, We're Going Home | 03:48         |
| You Rock My World         | 05:38         |
 | P.Y.T                     | 03:59         |

Simply **separating** the initial **data** into multiple tables would **break the connection between the data**.
That means the relationships between the newly introduced tables need to be determined.

**Album table**

| album_id | album_name | album_year |
|---|------------|------------|
|1| Palm Beach | 2019       |

**Artist table**

| artist_id | artist_name     |album_id|
|----|-----------------|---|
| 7 | Kanye West      | 1 |
| 8 | Drake           | 1 |
| 9  | Micheal Jackson | 1 |

**Artist genre table**

| genre_id | genre |
|----|-------------|
| 55 | Rap         |
| 56  | Hip Hop     |
| 57  | Pop         |
| 58  | Funk        |
 
 **Artist - Artist genre table**
 
| artist_id | genre_id |
|---------|-------------|
|  7   |   55  |
|  7   |   56  |
|  8  |  55   |
|   8  |   56  |
|   9  |   57  |
|  9   |   58  |

**Song table**

 | song_name                 | song_duration | artist_id |
|---------------------------|---------------|-----|
 | Stronger                  | 05:12         | 7 |
  | Heartless                 | 03:12         | 7 |
 | God's plan                | 03:19         | 8 |
 | Hold On, We're Going Home | 03:48         | 8 |
  | P.Y.T                     | 03:59         | 9 |
| You Rock My World         | 05:38  | 9 |

Instead of one table in unnormalized form, there are now 5 tables conforming to the 1NF. 
