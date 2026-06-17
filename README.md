# Fine-Tuning de Análise de Sentimentos em Português com Qwen 3.5 e Unsloth Studio

Este repositório contém os dados e o guia para o ajuste fino (fine-tuning) do modelo Qwen 3.5 0.8B para classificação de sentimentos em português (positivo, negativo e neutro) utilizando o Unsloth Studio.

O modelo ajustado resultante deste treinamento está publicado no Hugging Face:
**[Caueh/Qwen3.5-0.8B-Sentiment-SFT-ptbr](https://huggingface.co/Caueh/Qwen3.5-0.8B-Sentiment-SFT-ptbr)**


## Estrutura do Repositório

* Unsloth_Studio_Colab.ipynb: Notebook com o pipeline completo de preparação de dados, inicialização do Unsloth Studio e testes/avaliação.
* imgs: Capturas de tela utilizadas no tutorial do notebook.

## Bases de Dados Utilizadas

Para o fine-tuning do modelo, foram utilizadas três fontes de dados diferentes para criar os conjuntos de treino, teste local e testes com dataset que possui outro padrão do que o modelo foi treinado. Abaixo estão as fontes e os links de onde cada dataset foi retirado:

1. **Análise de Sentimentos PT-BR (Kaggle)**
   - **Link:** [gazprom/anlise-de-sentimentos-pt-br](https://www.kaggle.com/datasets/gazprom/anlise-de-sentimentos-pt-br)
   - **Descrição:** Dataset generalista em português adaptado de bases públicas.

2. **Dataset Análise de Sentimentos Gemini (Kaggle)**
   - **Link:** [rogesgrandi/dataset-para-anlise-de-sentimentos-gemini](https://www.kaggle.com/datasets/rogesgrandi/dataset-para-anlise-de-sentimentos-gemini)
   - **Descrição:** Dataset sintético gerado via API do Gemini contendo sentenças curtas e sua classificação.

3. **TweetSentBR Few-Shot (Hugging Face)**
   - **Link:** [eduagarcia/tweetsentbr_fewshot](https://huggingface.co/datasets/eduagarcia/tweetsentbr_fewshot)
   - **Descrição:** Dataset com dados reais do Twitter (X) focado em português. Ele não é usado no treino, mas sim importado diretamente na etapa de avaliação para testar a capacidade de generalização do modelo em um domínio diferente.

## Curadoria e Balanceamento dos Dados

1. **Treino Balanceado (1.998 amostras)**: 999 amostras de cada base (gemini.csv e sentiment_analysis_pt_br.csv), divididas igualmente em 333 por classe (positivo, negativo, neutro) e formatadas no padrão ChatML.
2. **Teste Local Balanceado (999 amostras)**: Montado a partir do excedente das bases, com 333 amostras por classe.
3. **Teste de Domain Shift (2.010 amostras)**: Base externa eduagarcia/tweetsentbr_fewshot do Hugging Face.

## Executado no Google Colab

Todo o fluxo foi executado no notebook Unsloth_Studio_Colab.ipynb:

1. **Configuração**: Executado as célula de clonagem e instalação do Unsloth e suas dependências.
2. **Preparação dos Dados**: A Etapa 1 baixa os datasets do Kaggle automaticamente e gera as divisões de treino e teste.
3. **Treinamento no Unsloth Studio**:
   - Iniciado o servidor do Studio no notebook para gerar a URL de acesso.
   - Configurado o treinamento com o modelo Qwen3.5 0.8B, método QLoRA 4-bit, 1 época, tamanho de contexto 512 e batch size 4.
4. **Avaliação**: O benchmark e a geração dos gráficos de desempenho rodam logo após a etapa de treinamento, importando o modelo do Hugging Face e medindo a acurácia.


## Resultados

Os resultados do fine-tuning podem ser observados nas imagens abaixo:

![Resultados no teste de Domain Sift](https://raw.githubusercontent.com/ClassNeuralNetwork/sentiment-analysis-qwen-unsloth/main/imgs/benchmark_domain_shift_results.png)
![Resultados no teste local](https://raw.githubusercontent.com/ClassNeuralNetwork/sentiment-analysis-qwen-unsloth/main/imgs/benchmark_local_results.png)
