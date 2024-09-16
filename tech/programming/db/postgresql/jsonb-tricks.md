# JSONB tips & trick

### JOIN with JSONB arrays.

example:

- **Table Orders**.

| id  | contents |
| :--- | :--- |
| 0191e324-c30a-704a-a7d6-bcd04c2d0032 | \[{"total": 15, "menu\_id": "0191dcea-c4e1-7c4b-9cac-0c3c0ee97baa", "carbohydrate": "red-rice"}\] |
| 0191e324-8f6f-7ba8-8cc5-ee19a9e3f5fc | \[{"total": 10, "menu\_id": "0191dcea-c4e1-7c4b-9cac-0c3c0ee97baa", "carbohydrate": "red-rice"}, {"total": 50, "menu\_id": "0191dcea-c4e1-7c4b-9cac-0c3c0ee97baa", "carbohydrate": "potato"}\] |

- **Table Menus**

| id | customer\_id | name | description |
| :--- | :--- | :--- | :--- |
| 0191dcea-c4e1-7c4b-9cac-0c3c0ee97baa | 0191d943-90a9-7986-821c-ebc85a593561 | Woku Ayam | Ayam makanan enak ayayayaya |
| 0191f129-00e7-7e82-a2a3-e11bf69f62a5 | 0191d943-90a9-7986-821c-ebc85a593561 | Ayam Bakar Spesial | Menu andalan kita, kamu harus coba |

Query JOIN `menu` dengan `order.contents` yang isinya array.
```SQL
SELECT
    menus.name,
    menus.description,
    orders->>'total' AS total
FROM orders
JOIN LATERAL jsonb_array_elements(orders.contents) AS order_meta ON true
JOIN menus ON menus.id = (order_meta->>'menu_id')::uuid AND menus.deleted_at IS NULL
WHERE orders.id = '...'
```
