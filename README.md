# Tradução automática

### 1. Tente valores diferentes do argumento num_examples na função load_data_nmt. Como isso afeta os tamanhos do vocabulário do idioma de origem e do idioma de destino?
O valor do argumento num_examples na função load_data_nmt foi testado com quatro valores diferentes, sendo eles:

* 900:
  Output: X: [[ 77 153   2   5   6   6   6   6]
 [112  57   2   5   6   6   6   6]]
valid lengths for X: [4 4]
Y: [[ 64   2   4   5   5   5   5   5]
 [282   8 283   2   4   5   5   5]]
valid lengths for Y: [3 5]

* 400:
Output: X: [[20  0  4  5  5  5  5  5]
 [59 26  2  4  5  5  5  5]]
valid lengths for X: [3 4]
Y: [[105   0   4   5   5   5   5   5]
 [ 63  78 111   6   2   4   5   5]]
valid lengths for Y: [3 6]

* 600:
Output: X: [[ 59 138   2   4   5   5   5   5]
 [ 83  96   2   4   5   5   5   5]]
valid lengths for X: [4 4]
Y: [[ 14  58   0   4   5   5   5   5]
 [100 171  74   2   4   5   5   5]]
valid lengths for Y: [4 5]

* 200:
Output: X: [[ 9  5  1  3  4  4  4  4]
 [ 5 62  0  3  4  4  4  4]]
valid lengths for X: [4 4]
Y: [[55 29  1  3  4  4  4  4]
 [ 5  0  3  4  4  4  4  4]]
valid lengths for Y: [4 3]

Pelo fato do tamanho do vocabulário em cada idioma depender diretamente da quantidade de dados processados,  ao ajustar num_examples para um valor menor, o número de frases lidas será menor, o que significa que o conjunto de palavras ou tokens únicos encontrados nos dados também será reduzido. Já se aumentar o valor de num_examples, mais frases serão incluídas, aumentando a diversidade de palavras/tokens e, assim, ampliando o tamanho do vocabulário tanto no idioma de origem quanto no idioma de destino. Ou seja:
Valores menores ->  vocabulário menor
Valores maiores -> vocabulário maior

Isso porque o vocabulário, tanto no idioma de origem quanto no idioma de destino, é diretamente proporcional ao número de tokens únicos(palavras/subpalavras) no conjunto de dados. Reduzindo ou aumentando, menos ou mais sentenças são processadas e [o tamanho do vocabulário se torne um hiperparâmetro que afeta a generalização do modelo.](https://machinetranslate.org/vocabulary)

Neste trabalho [fonte](https://www.rws.com/language-weaver/blog/issue-121-finding-the-optimal-vocabulary-size-for-neural-machine-translation/), na seção de resultados e análise, vocabulários pequenos parecem ser subótimos e espera-se que vocabulários maiores alcancem os melhores resultados de avaliação.


### 2. O texto em alguns idiomas, como chinês e japonês, não tem indicadores de limite de palavras (por exemplo, espaço). A tokenização em nível de palavra ainda é uma boa ideia para esses casos? Por que ou por que não? 


No caso de idiomas como chinês e japonês, onde não há separadores de palavras explícitos(como espaços em inglês ou português), a tokenização em nível de palavra não é uma boa ideia porque tanto o chinês quanto o japonês são idiomas que têm uma estrutura mais complicada referente à separação de palavras e por ter partículas que compõem o texto e dão sentido à sentenças. Essas características específicas causam confusão, visto que os caracteres podem representar palavras individuais ou partes de palavras. Fazer uma tokenização em nível de palavra nesses idiomas demandaria uma segmentação prévia, sendo uma tarefa difícil e vulnerável a erros. Há algumas alternativas para tentar resolver esse problema que são:

Tokenização em nível subword: acontece de maneira semelhante com os métodos BPE (Byte Pair Encoding). Essa abordagem trata os caracteres diretamente como tokens e também divide-os em subpartes comuns. A ferramenta SentencePiece, por exemplo, também pode ser uma alterativa para encontrar padrões comuns em subpalavras, melhorando a tokenização.

Tokenização em nível de caractere: Cada caractere é tratado como um token individual. No chinês, por exemplo, muitos caracteres são equivalentes a uma palavra completa.

Ou seja, a tokenização em nível de palavra não é ideal para idiomas como chinês e japonês A abordagem recomendada é a tokenização em nível de caractere ou subword, que lida melhor com idiomas asiáticos que possuem estrutura completamente diferente de idiomas derivados do latim como inglês e português. [Pode-se argumentar que os limites das palavras são mal definidos em todas as línguas, não apenas no chinês. Hapselmath (2011) definiu 10 critérios linguísticos para determinar se algo é uma palavra (vs. um afixo ou expressão), mas é difícil chegar a algo consistente. A maioria dos sistemas de escrita coloca espaços entre as palavras, então não há confusão. Além do chinês, apenas um punhado de outras línguas (japonês, vietnamita, tailandês, khmer, laosiano e birmanês) têm esse problema.](https://luckytoilet.wordpress.com/2020/09/07/the-biggest-headache-with-chinese-nlp-indeterminate-word-segmentation/)


Fontes:
**1**: 
* https://www.rws.com/language-weaver/blog/issue-121-finding-the-optimal-vocabulary-size-for-neural-machine-translation/
* A seção 4 desse artigo evidência porque o tamnaho de alguns vocabulários são melhores que outros:
https://aclanthology.org/2020.findings-emnlp.352/
* https://machinetranslate.org/vocabulary

**2**:
* https://medium.com/the-artificial-impostor/nlp-four-ways-to-tokenize-chinese-documents-f349eb6ba3c3
* https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00560/116047/Sub-Character-Tokenization-for-Chinese-Pretrained
* https://luckytoilet.wordpress.com/2020/09/07/the-biggest-headache-with-chinese-nlp-indeterminate-word-segmentation/



