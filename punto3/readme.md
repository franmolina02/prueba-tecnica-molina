## Explicación

1. Se detecta el commit con tipo de cambio mayor. Se utilizan conventional commits. En base a esto y la versión actual se genera el bump de version correspondiente.

2. Se buildea la imagen y actualiza el chart de Helm de forma paralela utilizando el nuevo tag de version.

3. Se upgradea la release de helm a la ultima version del chart.