HW 2 - Data Cleaning, CS 625, Fall 2022
================
Brock Briggs
Sep 19, 2022

## Part 1. Data Cleaning

Preparation: \* Copied Petnames.tsv dataset and pasted into openrefine.
Source: <https://github.com/jgolbeck/petnames> \* Parsed cells for text
to numbers which identified most of the ‘age’ column as numbers \*
Initially 1783 records

Steps Taken:

1.  Consolidate the ‘kinds of pets’ to as few as possible

-   Select arrow on column 2 (What kind off pet is this(Dog,…)), create
    text facet, shows 83 choices
-   Selected ‘Cluster’ and merged 15 different clusters of different pet
    types to overcome spelling issues. Resulting values: Dog, Cat,
    Hamster, Guinea pig, Rabbit, Bird, Bunny, Horse, Hermit Crab, Other,
    Chinchilla, Goldfish, Tortoise, Lizard, Fish. Occured under key
    collision method and fingerprinting keying function.
-   Under ‘nearest neighbor’ method levenshtein, consolidated two rows
    of beta fish into one
-   Using key collision and Beider-Morse keying function, merged’Cat’
    and ‘Cats’
-   Experimented with changing radius size and block charts under
    nearest neighbor method ppm. Merged two values of guinea pig, two of
    gold fish, and two of chinchilla.
-   Decided for consolidation purposes to house all types of animals
    under one blanket category. This eliminated beta fish, gold fish,
    other fish to just fish. Same for snakes and geckos.
-   Selected records with one or two rows of animals I didn’t recognize
    and used the ‘Pet’s Breed’ column to identify what animal was being
    spoken a bout. For example pitbull was changed to dog. Torti was
    changed to cat. Often the kind of pet was mispelled or name was
    given in the field incorrectly but by using other information filled
    out can be corrected.
-   Used custom text facet to search for “other” kinds of pets. Used
    expression ’value.replace(“Other:”,““)’ under transform values to
    get rid of the”Other: ” in front of Prairie Dog and Bees.
-   Used value.replace(“Bunny”,“Rabbit”) to merge bunny category with
    rabbit.
-   Trimmed leading and trailing whitespace function under common
    transforms. Also used Titlecase.
-   Saw one record with multiple values in the kind. Used split multi
    valued cells function and then merged with cluster and edit (dog,
    dog, dog, cat).
-   Scrolled through list of 36 and pulled 10 rows of odd named ‘kinds’
    where breed didn’t indicate what kind of animal it was (‘server’,
    ‘virus’, ‘mona’, ‘robot’, etc). Did custom text transform to bring
    these all under ‘Other’.
-   Final Pet kind count of 27.

2.  Pet’s Full Names

-   Merged 57 clusters under key collision fingerprint and 3 under
    ngram-fingerprint
-   Noted that less name clustering can be done because there’s such a
    variety in spelling names. For example Button and Buttons could
    easily be two distinct names and without knowing more information,
    can’t merge.
-   1461 distinct full names in column with 8 blanks. If I were making
    this dataset complete and had leeway on editing, I would fill the
    full name with the most common name by count for that particular
    animal. This would be done by doing text facet on pet kind,
    selecting the kind of animal it fell into and then doing the same
    for the pet’s full name column to see what is the most common name
    for that category.

3.  Pet’s everyday names

-   Merged 64 clusters under key collision fingerprint and 3 under
    ngram-fingerprint
-   Noted that less name clustering can be done because there’s such a
    variety in spelling names. For example Button and Buttons could
    easily be two distinct names and without knowing more information,
    can’t merge.

4.  Pet’s Age

-   Merged 15 clusters with key collision and fingerprint. Without any
    way to verify ages, took input as truth so any year with a “?”
    implied that it was correct year. Moved new cell values to suggested
    whole number.
-   Used custom text transform GREL expression ‘value.replace(” years”,
    ““)’ to remove years from all values. Want to get every field to a
    numeric value. This actually changed everything in the column to a
    text value rather than numeric. Will come back and fix this.
-   Searched for all values with text filter for everything that
    included deceased and merged into just deceased.
-   Identified multiple categories of numbers, whole and then values in
    months and weeks as well. Need to convert those to one consistent
    value. Given most values are already in years, going to convert
    weeks and months to years. Did text search for ‘week’, whittled
    weeks out using replace function, changed to number with toNumber,
    and divided by 52 weeks to get part of year number. Expression:
    ’toNumber(value.replace(“weeks”,““) / 52)’. Changed one value that
    contained a long sentence manually. Performed similar process for
    months except using 12 as the number divided by.
    ’toNumber(value.replace(”months”,““)) / 12’. Performed the same
    process for”mo” and “mos” signifying months.
-   Edited 4 rows with different verbiage of passing away clustering
    doesn’t pick up on.

## Part 2. Analyze Cleaned Data

1.  How many types (kinds) of pets are there?

2.  How many dogs?

3.  How many breeds of dogs?

4.  What’s the most popular dog breed?

5.  What’s the age range of the dogs?

6.  What’s the age range of the guinea pigs?

7.  What is the oldest pet?

8.  Which are more popular, betta fish or goldfish? How many of each?

9.  What’s the most popular everyday name for a cat?

10. What’s the most popular full name for a dog?
