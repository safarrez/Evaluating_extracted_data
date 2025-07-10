Some useful SQL queries and analysis ideas to gain insights:

---

### 1. Basic Counts - Done
- **Total number of studies:**
  ```sql
  SELECT COUNT(*) FROM Ground_truth;
  ```
- **Number of extracted records:**
  ```sql
  SELECT COUNT(*) FROM Extracted;
  ```

### 2. Distribution of Categories - Done
- **How many studies per Allocation type?** 
```sql
-- Count in ground truth
SELECT Allocation, COUNT(*) FROM Ground_truth GROUP BY Allocation;

-- Count of each Allocation type in Extracted
SELECT Allocation, COUNT(*) as count FROM Extracted GROUP BY Allocation;
  ```
- **How many studies per Experimenter type?** 
```sql
SELECT Experimenter, COUNT(*) FROM Ground_truth GROUP BY Experimenter;

SELECT Experimenter, COUNT(*) as count FROM Extracted GROUP BY Experimenter;
  ```

---
### 3. Studies with Disagreement
Find studies where the 'Allocation' or 'Experimenter' values disagree between ground truth and extracted:
````sql
SELECT gt.Study, gt.Allocation as gt_alloc, ext.Allocation as ext_alloc
FROM Ground_truth gt
JOIN Extracted ext ON gt.Study = ext.Study
WHERE gt.Allocation != ext.Allocation;

SELECT gt.Study, gt.Experimenter as gt_exp, ext.Experimenter as ext_exp
FROM Ground_truth gt
JOIN Extracted ext ON gt.Study = ext.Study
WHERE gt.Experimenter != ext.Experimenter;
````

---
### 4. Agreement Between Extracted and Ground Truth - useless
- How many studies have matching Allocation?
  ```sql
  SELECT COUNT(*) 
  FROM Ground_truth gt
  JOIN Extracted ext ON gt.Study = ext.Study
  WHERE gt.Allocation = ext.Allocation;
  ```
- How many studies have matching Experimenter?
  ```sql
  SELECT COUNT(*) 
  FROM Ground_truth gt
  JOIN Extracted ext ON gt.Study = ext.Study
  WHERE gt.Experimenter = ext.Experimenter;
  ```

---

### 5. Error Analysis
- Which studies have mismatches in Allocation? (isn't this just |gt join ext| - |accuracy| why bother counting both)
  ```sql
  SELECT gt.Study, gt.Allocation AS gt_alloc, ext.Allocation AS ext_alloc
  FROM Ground_truth gt
  JOIN Extracted ext ON gt.Study = ext.Study
  WHERE gt.Allocation != ext.Allocation;
  ```
- **Which Experimenter types are most often misclassified?**
  ```sql
  SELECT gt.Experimenter AS true_label, ext.Experimenter AS predicted_label, COUNT(*)
  FROM Ground_truth gt
  JOIN Extracted ext ON gt.Study = ext.Study
  WHERE gt.Experimenter != ext.Experimenter
  GROUP BY gt.Experimenter, ext.Experimenter;
  ```

---

### 6. Coverage
- Are there studies in ground truth not present in extracted (obviously yes because we only use 20% of the data for testing and 80% for training)
  ```sql
  --- i don't like the not in, i'll try doing an anti join
  SELECT Study FROM Ground_truth 
  WHERE Study NOT IN (SELECT Study FROM Extracted);
  ```
- **Are there studies in extracted not present in ground truth?**
  ```sql
  SELECT Study FROM Extracted
  WHERE Study NOT IN (SELECT Study FROM Ground_truth);
  ```

---

### 7. Advanced: Confusion Matrix for Experimenter
- How often is each Experimenter type predicted as each other type? (i doubt this one works as expected tbh)
  ```sql
  SELECT gt.Experimenter AS true_label, ext.Experimenter AS predicted_label, COUNT(*)
  FROM Ground_truth gt
  JOIN Extracted ext ON gt.Study = ext.Study
  GROUP BY gt.Experimenter, ext.Experimenter;
  ```

---

