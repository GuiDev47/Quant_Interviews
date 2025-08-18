# 📘 Révision SQL – Support Complet

## 1. Introduction
SQL (**Structured Query Language**) est utilisé pour interroger et manipuler des bases relationnelles.  
Une base est composée de **tables** (relations), elles-mêmes constituées de **colonnes** et de **lignes** (tuples).

---

## 2. Commandes de Base (CRUD)
- **C**reate : `INSERT`
- **R**ead : `SELECT`
- **U**pdate : `UPDATE`
- **D**elete : `DELETE`

### Exemple
```sql
-- Sélection simple
SELECT first_name, last_name
FROM employees
WHERE department = 'Quant';

-- Insertion
INSERT INTO employees (id, first_name, last_name, department)
VALUES (101, 'Guillaume', 'Bodard', 'Quant');

-- Mise à jour
UPDATE employees
SET department = 'Quant Dev'
WHERE id = 101;

-- Suppression
DELETE FROM employees
WHERE id = 101;
```

---

## 3. Filtres et Opérateurs
- `WHERE`, `=`, `<`, `>`, `<>`
- `BETWEEN`, `IN`, `LIKE`, `IS NULL`

```sql
SELECT *
FROM trades
WHERE price BETWEEN 100 AND 200
  AND trader_id IN (12, 13, 14)
  AND symbol LIKE 'EUR%';
```

---

## 4. Agrégations
- `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`
- `GROUP BY` + `HAVING`

```sql
-- Volume moyen par trader
SELECT trader_id, AVG(volume) AS avg_volume
FROM trades
GROUP BY trader_id
HAVING AVG(volume) > 1000;
```

---

## 5. Jointures
- **INNER JOIN** : intersection
- **LEFT JOIN** : tout à gauche + correspondances
- **RIGHT JOIN** : tout à droite
- **FULL OUTER JOIN** : tout
- **SELF JOIN** : jointure sur soi-même

```sql
-- Lister les trades avec nom du trader
SELECT t.trade_id, t.symbol, tr.name
FROM trades t
INNER JOIN traders tr
    ON t.trader_id = tr.id;
```

---

## 6. Sous-requêtes
```sql
-- Traders ayant un volume supérieur à la moyenne
SELECT trader_id, SUM(volume) as total_volume
FROM trades
GROUP BY trader_id
HAVING SUM(volume) > (
    SELECT AVG(total_volume)
    FROM (
        SELECT SUM(volume) as total_volume
        FROM trades
        GROUP BY trader_id
    ) sub
);
```

---

## 7. Fenêtres (Window Functions) ⭐
- `ROW_NUMBER()`
- `RANK()`, `DENSE_RANK()`
- `LAG()`, `LEAD()`
- `SUM() OVER(...)`, `AVG() OVER(...)`

```sql
-- Classement des traders par volume
SELECT trader_id,
       SUM(volume) AS total_volume,
       RANK() OVER(ORDER BY SUM(volume) DESC) as rank_volume
FROM trades
GROUP BY trader_id;

-- Différence de prix avec la ligne précédente
SELECT symbol, trade_time, price,
       price - LAG(price) OVER(PARTITION BY symbol ORDER BY trade_time) AS price_diff
FROM trades;
```

---

## 8. Index, Performance et Bonnes Pratiques
- Indexer les colonnes souvent utilisées dans `JOIN` ou `WHERE`.
- Utiliser `EXPLAIN` pour analyser les plans d’exécution.
- Attention aux `SELECT *` (inutile et coûteux).

---

## 9. Exercices Classiques d’Entretien
1. **Deuxième plus haut salaire**  
```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

2. **Trades avec un volume au-dessus de la médiane**  
(Utilise `PERCENTILE_CONT` si dispo)  
```sql
SELECT *
FROM trades
WHERE volume > (
  SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY volume)
  FROM trades
);
```

3. **Rolling sum sur 3 jours par symbole**  
```sql
SELECT symbol, trade_date, volume,
       SUM(volume) OVER(
            PARTITION BY symbol
            ORDER BY trade_date
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) AS rolling_volume
FROM trades;
```

---

## 10. Checklist pour les révisions rapides
✅ SELECT / WHERE / GROUP BY / HAVING  
✅ JOINS (INNER, LEFT, SELF)  
✅ Sous-requêtes corrélées  
✅ Fenêtres (`ROW_NUMBER`, `LAG`, `RANK`)  
✅ Optimisation via INDEX et EXPLAIN  
✅ Cas pratiques (2ème max, médiane, rolling sums)  

---
