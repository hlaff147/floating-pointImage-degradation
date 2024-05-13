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