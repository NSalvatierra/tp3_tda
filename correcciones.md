# ✔️ Checklist de correcciones

## Código

- [X] **Línea 11-14**
  clusters_nuevo = []
  for cluster in clusters:
      set_cluster = set(cluster)
      clusters_nuevo.append(set_cluster)
  ✏️ Ninguna operación posterior se beneficia del uso de `set`. Además, si hay vértices repetidos en un clúster, la solución ya es inválida.

- [X] **Línea de distancia:**
  distancia = bfs_distancia_uv(grafo, u, v)
  ✏️ Mejorar complejidad precalculando las distancias desde `u` una sola vez con `bfs(grafo, u)`.

- [X] **Análisis de complejidad:**
  ✏️ El BFS se llama múltiples veces, por lo que la complejidad no es solo `O(k + V + E)`. Analizar con más precisión cuántas veces se invoca BFS.

## Reducción a K-Coloring

- [R] **Descripción de la reducción**
  ✏️ La reducción planteada está al revés. Se debe mostrar cómo resolver K-Coloring usando una solución a K-Clustering, no al revés.

- [R] **Definición de C**
  ✏️ Falta definir el valor de `C` (el diámetro máximo permitido). No se puede demostrar propiedad alguna sin este dato clave.

- [R] **Demostración ida y vuelta**
  ✏️ No se puede hacer demostración sin que quede claro qué garantiza que dos nodos del mismo color tengan distancia menor o igual a `C`. Esto no es cierto en general.

- [R] **Transformación incorrecta**
  ✏️ Reformular la construcción del grafo transformado. No se garantiza que la distancia entre nodos del mismo color sea ≤ C, y por tanto, la reducción no es válida.

## Implementación backtracking

- [X] **mejor_solucion = {...}**
  ✏️ Bien definida, pero...

- [X] **Funciones anidadas**
  ✏️ Evitar funciones anidadas como `esta_lleno_los_clusters_anteriores`.

- [X] **backtracking_recursivo(...) no retorna**
  ✏️ La función tiene un propósito (encontrar mejor solución), pero no retorna ese resultado. Se espera que lo haga.

- [X] **calcular_nuevo_diametro(...)**
  ✏️ Los `break` hacen que se devuelva un valor desactualizado. Se recomienda:

      nuevo_diametro = max(diametro_actual, max(distancias[vertice][v] for v in cluster))

  Esto aparece en el informe pero **no está en el código**.

## Programación Lineal

- [X] **Restricción de unicidad**
  model += pulp.LpSum(x[v, i] for i in range(k)) == 1
  ✏️ Expresar como fórmula matemática en el informe, no como código Pulp.

- [X] **Restricción de D[i]**
  model += D[i] >= dist * (x[u, i] + x[v, i] - 1)
  ✏️ Explicar su comportamiento para distintos valores de `x[u,i] + x[v,i]`. También justificar por qué el modelo no hace `D[i]` arbitrariamente grande.

- [X] **Restricción de D_max**
  model += D_max >= D[i]
  ✏️ También justificar por qué `D_max` no se va a valores altos arbitrariamente.

## Comparación con Louvain

- [ ] **Descripción del algoritmo Louvain**
  ✏️ Agregar explicación de cómo funciona Louvain antes de mostrar resultados.

- [ ] **Tabla de comparación**
  ✏️ Si Louvain encuentra menos clústers que el óptimo, eso es inconsistente. Un resultado de Louvain con 2 clústers vs óptimo de 3 no puede ser válido.

- [ ] **Cuadro 3 (datasets grandes)**
  ✏️ Falta incluir la solución óptima como referencia en los casos grandes. Como ustedes generan los grafos, pueden conocer el óptimo por construcción.

- [ ] **Criterio de evaluación**
  ✏️ No se puede afirmar que Louvain no aproxima bien sólo porque da menos clústers. Puede estar resolviendo un problema diferente (menos restricciones).

## Resultados Backtracking

- [ ] **Validación de correctitud**
  ✏️ Generar datasets simples donde se pueda validar manualmente la solución.

- [X] **Medición de tiempos**
  ✏️ Mostrar gráficos por separado para PL y Backtracking. PL tiene tiempos mucho más altos y hace ilegible la curva de backtracking.
