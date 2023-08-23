# Ferramenta de Reconstrução de Arquivo D64 `combine_d64`

Esta ferramenta ajuda a reconstruir uma imagem correta do Commodore 1541 `.D64` a partir de múltiplas cópias comparando diferentes cópias bloco a bloco.

Existem dois casos de uso:

## Preservando Software Comercial

Se você deseja preservar os discos 1541 comerciais originais, verifique se a imagem que está criando está correta. Se todos tiverem uma única cópia do disco original, mesmo sem nenhum erro de leitura, alguns blocos ainda podem ter sido alterados pelo usuário.

Se você tiver várias cópias do disco original e duas (ou melhor: três) delas forem idênticas, pode ter certeza de que possui um despejo correto.

Mas se todas as suas cópias tiverem blocos alterados ou quebrados, você ainda pode ter sorte, desde que não sejam os mesmos nos discos. É aqui que esta ferramenta ajuda.

## Resgatando discos quebrados

Se você tiver um disco (dados pessoais) com muitos erros de leitura, o primeiro passo (depois de limpar bem o cabeçote de leitura com álcool!) seria maximizar o número de tentativas ao lê-lo. Você também pode lê-lo em diferentes unidades e ver qual delas oferece o melhor resultado.

Mas, muitas vezes, diferentes tentativas de leitura fornecem uma distribuição diferente de blocos quebrados. Esta ferramenta ajuda a combinar os dados corretos em várias imagens com erros.

## Uso

python combine_d64.py <arquivo1.d64> <arquivo2.d64> [...]

Dependendo dos arquivos de entrada, você obterá um destes resultados:

### 1. Imagens idênticas

As seguintes imagens são idênticas:
05b.d64 14b.d64

Duas ou mais imagens são idênticas e todas as outras imagens são diferentes. Você pode assumir que tem duas leituras corretas e pode usar qualquer um dos arquivos corretos como resultado.

### 2. Conjuntos de imagens idênticas

Os seguintes conjuntos de imagens são idênticos:
0: 42b.d64 45b.d64
1: 44b.d64 46b.d64

Duas ou mais imagens são idênticas, mas também existem outros conjuntos idênticos. Um dos conjuntos _pode_ estar correto. Isso geralmente acontece no caso de "Preservação de software comercial" se diferentes usuários fizerem as mesmas alterações em um disco. Você precisará observar as diferenças entre os conjuntos e decidir qual deles (se houver) é o estado original.

### 3. Imagem de Blocos Duplicados

Cada bloco das imagens a seguir também está contido em pelo menos um outro arquivo:
48a.d64

Existe uma imagem cujo bloco existe como duplicado em outra imagem. Esta é uma boa indicação de que esta imagem é uma leitura correta e você pode tomá-la como resultado.

### 4. Imagem Combinada

O resultado foi combinado a partir das seguintes imagens:
41ad64 (667 blocos)
42ad64 (16 blocos)

De cada bloco, existem pelo menos duas cópias idênticas nos discos. A ferramenta cria "result.d64" com os dados combinados e imprime quais imagens foram utilizadas para quantos blocos.

### 5. Duplicatas insuficientes

Para os seguintes blocos, não existem duplicatas:
400 (Trk 20 seg 5)

Se não houver pelo menos duas cópias idênticas de cada bloco, a ferramenta imprime os números de bloco (e trilha/setor) dos blocos que precisam de mais cópias.

### Avisos

Para os casos 3 e 4, a ferramenta também imprime uma lista de blocos para os quais existiam apenas 2 cópias idênticas. Você pode querer ter cuidado com esses bloqueios e verificar novamente se eles não são causados por alterações idênticas por usuários diferentes (caso "Preservar software comercial").

Os seguintes blocos tiveram apenas um número limitado de cópias:
2 cópias do bloco 125 (Trk 6 Sec 20) em 48a.d64 51a.d64
2 cópias do bloco 357 (Trk 18 Sec 0) em 48a.d64 50a.d64
2 cópias do bloco 358 (Trk 18 Sec 1) em 48a.d64 50a.d64

## Limitações

Observe também que os discos comerciais podem estar usando truques de proteção contra cópia que não podem ser representados em imagens `.D64`. Você precisará de imagens `.G64` para isso. Você pode usar esta ferramenta nas versões `.D64`, para que ela possa dizer qual arquivo `.G64` correspondente é provavelmente o correto. Se a ferramenta criar um "result.d64" combinado, crie manualmente um arquivo `.G64` correto a partir dele.

Houve apenas testes limitados, especialmente em imagens com erros. Use por sua conta e risco e verifique os resultados manualmente, especialmente no caso 4!# Reconstrucao64
