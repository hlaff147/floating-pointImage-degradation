# Magnificação
```
Dada uma imagem representada por uma matriz 𝑀 com dimensões 𝑚 × 𝑛 × 𝑐, onde 𝑚 é o número de linhas, 𝑛 é o número de colunas e 𝑐 é o número de canais de cor (geralmente 1 para imagens em escala de cinza ou 3 para imagens coloridas).

Para ampliar a imagem por um fator 𝑘, criamos uma nova matriz 𝑀' com dimensões 𝑘𝑚 × 𝑘𝑛 × 𝑐. A cada pixel (𝑖, 𝑗) da imagem original, copiamos seu valor 𝑘 × 𝑘 vezes para a região ampliada (𝑖𝑘, 𝑗𝑘) a (𝑖+1)𝑘−1, (𝑗+1)𝑘−1).
```
# Contração
```Dada uma imagem representada por uma matriz M com dimensões m x n x c, onde m é o número de linhas, n é o número de colunas e c é o número de canais de cor (geralmente 1 para imagens em escala de cinza ou 3 para imagens coloridas).

Para contrair a imagem por um fator k, criamos uma nova matriz M' com dimensões (m/k) x (n/k) x c. Em seguida, para cada bloco k x k na imagem original, selecionamos o primeiro pixel do bloco e o atribuímos ao pixel correspondente na imagem contraída.
```
# Deformação
```
A função `deform_image` aplica uma deformação horizontal à imagem, deslocando cada linha horizontalmente por uma quantidade determinada `dx`.

Dada uma imagem representada por uma matriz M com dimensões m x n, onde m é o número de linhas e n é o número de colunas, a deformação é aplicada deslocando cada linha da imagem horizontalmente por dx pixels. Isso resulta em uma imagem deformada M' com as mesmas dimensões que a imagem original.

Para cada pixel (i, j) na imagem original, copiamos seu valor para a posição deslocada (i+dx, j) na imagem deformada, desde que a linha resultante esteja dentro dos limites da imagem.
```

```
Na função `warpPerspective`, que é responsável por aplicar uma transformação de perspectiva a uma imagem usando uma matriz de transformação. 
```
### Objetivo da Função
A função `warpPerspective` transforma a imagem de entrada conforme a matriz de transformação `M`, ajustando-a para o tamanho especificado por `dsize`.

### Parâmetros da Função
- `image`: A imagem de entrada representada como um array numpy.
- `M`: A matriz de transformação (3x3).
- `dsize`: Uma tupla que especifica as dimensões da imagem de saída `(altura, largura)`.

### Etapas Detalhadas da Função

#### 1. Inicializar a Imagem de Saída
```python
h, w = dsize
if len(image.shape) == 2:  # Imagem em escala de cinza
    output = np.zeros((h, w), dtype=image.dtype)
```
Aqui, inicializamos a imagem de saída `output` com zeros e com o mesmo tipo de dados da imagem de entrada. Isso é feito apenas se a imagem de entrada for em escala de cinza (2 dimensões).

#### 2. Calcular a Matriz Inversa
```python
M_inv = np.linalg.inv(M)
```
Calculamos a inversa da matriz de transformação `M`. A inversa é necessária para mapear os pontos da imagem de destino de volta para a imagem de origem.

#### 3. Percorrer Cada Pixel da Imagem de Saída
```python
for y in range(h):
    for x in range(w):
```
Aqui, percorremos cada pixel `(x, y)` da imagem de saída.

#### 4. Calcular as Coordenadas de Origem
```python
dest_coords = np.array([x, y, 1])
src_coords = M_inv @ dest_coords
src_x, src_y = src_coords[0] / src_coords[2], src_coords[1] / src_coords[2]
```
Para cada pixel da imagem de destino, representado por `dest_coords`, calculamos suas coordenadas correspondentes na imagem de origem usando a matriz inversa `M_inv`. As coordenadas são calculadas em coordenadas homogêneas, e depois convertidas de volta para coordenadas cartesianas (`src_x, src_y`).

#### 5. Verificar Se as Coordenadas Mapeadas Estão Dentro dos Limites da Imagem de Origem
```python
if 0 <= src_x < image.shape[1] and 0 <= src_y < image.shape[0]:
```
Verificamos se as coordenadas calculadas `src_x` e `src_y` estão dentro dos limites da imagem de origem.

#### 6. Interpolação Bilinear
Se as coordenadas estão dentro dos limites, realizamos a interpolação bilinear para calcular o valor do pixel na imagem de saída.

```python
x0, y0 = int(src_x), int(src_y)
x1, y1 = min(x0 + 1, image.shape[1] - 1), min(y0 + 1, image.shape[0] - 1)
dx, dy = src_x - x0, src_y - y0

if len(image.shape) == 2:  # Imagem em escala de cinza
    output[y, x] = (
        image[y0, x0] * (1 - dx) * (1 - dy) +
        image[y0, x1] * dx * (1 - dy) +
        image[y1, x0] * (1 - dx) * dy +
        image[y1, x1] * dx * dy
    )
```
##### Explicação da Interpolação Bilinear
- `x0, y0`: Ponto superior esquerdo.
- `x1, y1`: Ponto inferior direito (próximos pixels).
- `dx, dy`: Diferenças fracionárias entre `src_x` e `src_y` e os pontos `x0` e `y0`.

O valor do pixel de saída é calculado como uma combinação ponderada dos valores dos quatro pixels vizinhos na imagem de origem, baseando-se nas distâncias fracionárias `dx` e `dy`.

