# 👩🏻‍⚕️ Analyzing International Students' Mental Health

## Project Description
In this project, we analyze a dataset on the mental health of international students to identify which factors may have the greatest impact on their mental well-being.

## Our Data
The dataset used in this project is provided by DataCamp and can be accessed through their [project page](https://app.datacamp.com/learn/projects/analyzing_students_mental_health).

Studying abroad can impact mental health. A 2018 survey at a Japanese university found international students face more mental health issues than the general population.
Social connectedness and acculturative stress are key factors in depression.

| Field Name    | Description                                      |
| ------------- | ------------------------------------------------ |
| `inter_dom`     | Types of students (international or domestic)   |
| `japanese_cate` | Japanese language proficiency                    |
| `english_cate`  | English language proficiency                     |
| `academic`      | Current academic level (undergraduate or graduate) |
| `age`           | Current age of student                           |
| `stay`          | Current length of stay in years                  |
| `todep`         | Total score of depression (PHQ-9 test)           |
| `tosc`          | Total score of social connectedness (SCS test)   |
| `toas`          | Total score of acculturative stress (ASISS test) |

## Objective
We’ll explore the data to answer key questions about international students' mental health, including:
- Whether they experience more depression than domestic students
- How social connectedness and acculturative stress impact their mental well-being
- If factors like length of stay, language proficiency and age play a role
- How depression levels differ between undergrad and grad students

## SQL Queries & Analysis

### 1. Are international students more depressed than domestic students?

```sql
SELECT inter_dom, 
       AVG(todep) AS avg_depression,
       COUNT(*) AS num_students

FROM public.students

GROUP BY inter_dom
```

Result:
| Type of Student | Average Depression Score |
| ------------- | ------------- |
| International Students | 8.61 |
| Domestic Students | 8.04 |

International students show a slightly higher average depression score. This could be due to challenges like culture shock, homesickness and language barriers.

---

### 2. Does social connectedness affect depression levels?

```sql
SELECT CORR(tosc, todep) AS correlation_tosc_todep

FROM public.students
```

Result:
Correlation between social connectedness and depression: -0.55

A negative correlation means that as social connectedness goes up, depression tends to go down. Students who feel more connected socially have lower depression levels.

---

### 3. Does acculturative stress predict depression?

```sql
SELECT CORR(toas, todep) AS correlation_toas_todep

FROM public.students
```

Result:
Correlation between acculturative stress and depression: 0.39

There’s a moderate positive correlation between acculturative stress and depression. The more stress students feel from adapting to a new culture, the higher their depression levels.

---

### 4. Does language proficiency impact mental health?

```sql
SELECT CORR(japanese, todep) AS correlation_japanese_todep

FROM public.students
WHERE inter_dom = 'Inter'
```
Correlation between Japanese language proficiency and depression: 0.017

```sql
SELECT CORR(english_cate::int, todep) AS correlation_english_todep

FROM public.students
WHERE inter_dom = 'Inter'
```
Correlation between English language proficiency and depression: 0.013

Result:
Both correlations are close to zero, showing almost no link between language proficiency and depression. While knowing the language is important, it doesn't seem to have a strong impact on mental health.

---

### 5. Does length of stay affect depression for international students?

```sql
SELECT 
  stay_cate,
  AVG(todep) AS avg_depression,
  COUNT(*) AS num_students

FROM public.students
WHERE inter_dom = 'Inter'

GROUP BY stay_cate
ORDER BY stay_cate
```

Result:
| Category | Average Depression Score |
| ------------- | ------------- |
| Long Stay | 7.59 |
| Medium Stay | 8.72 |
| Short Stay | 7.48 |

Length of stay doesn’t show a clear strong impact on depression. Medium-stay students report the highest depression scores, but other factors like acculturative stress might matter more.

---

### 6. Undergraduate vs Graduate Depression Levels

```sql
SELECT academic, 
       AVG(todep) AS avg_depression,
       COUNT(*) AS num_students

FROM public.students

GROUP BY academic
```

Result:
| Academic Level | Average Depression Score |
| ------------- | ------------- |
| Undergraduate | 8.43 |
| Graduate | 5.28 |

Graduate students report lower depression levels than undergrads. This might be because they’re older (something we will analyze above), more experienced, and have better coping strategies. More support for undergrad students could help.

---

### 7. Is age related to depression?

```sql
SELECT CORR(age, todep) AS correlation_age_todep

FROM public.students
```

Result:
Correlation between age and depression: -0.11

Age has a very weak negative correlation with depression. So, it seems like age isn’t a big factor in depression levels here. Other things, such as social support and cultural stress, likely play a bigger role.

---

## Final Conclusions
- Social connectedness is a key factor in fighting depression. Helping students build strong social networks could significantly improve their mental health.
- Acculturative stress is a big contributor to depression. Programs that help students adjust to a new culture could make a real difference.
- Length of stay and language proficiency don’t have a major impact on depression, so it’s better to focus on the other factors.
- Graduate students tend to have lower depression levels, so more targeted support for undergrads could help reduce depression.

These insights can guide future research and help create programs that better support international students' mental health, especially by focusing on social connections and reducing acculturative stress.
