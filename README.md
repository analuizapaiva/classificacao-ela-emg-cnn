# Classificação de Esclerose Lateral Amiotrófica por Sinais EMG

## Sobre o projeto

Este projeto investiga a aplicação de técnicas de aprendizado profundo para a classificação binária de sinais eletromiográficos (EMG), buscando diferenciar registros de indivíduos com Esclerose Lateral Amiotrófica (ELA) e controles saudáveis.

O trabalho utiliza redes neurais convolucionais para identificar padrões presentes na representação espectral dos sinais, além de investigar a contribuição de descritores estatísticos e fisiológicos.

## Objetivos

- Desenvolver uma CNN 2D para classificação de espectrogramas EMG.
- Extrair características dos sinais: RMS, ZCR, WL, SSC e MF.
- Investigar a combinação de características espectrais e descritores EMG.
- Comparar os modelos utilizando métricas de classificação.
- Avaliar limitações relacionadas à quantidade de pacientes e à generalização.

## Dataset

Foi utilizado o conjunto de dados EMGLAB N2001, composto por registros eletromiográficos de indivíduos com ELA e controles saudáveis.

Na seleção binária utilizada neste projeto, foram considerados 16 pacientes, distribuídos entre as classes ELA e controle.

Os registros foram segmentados em janelas de aproximadamente um segundo.

A divisão foi realizada por paciente, evitando que sinais de um mesmo indivíduo fossem compartilhados entre treinamento, validação e teste.

| Conjunto | Pacientes | Janelas |
|---|---:|---:|
| Treinamento | 9 | 2.247 |
| Validação | 3 | 874 |
| Teste reservado | 4 | 901 |

**Fonte dos dados:** EMGLAB N2001

## Pré-processamento

As principais etapas foram:

1. Leitura dos registros EMG.
2. Verificação de integridade e remoção de janelas inválidas.
3. Segmentação dos sinais em janelas de um segundo.
4. Redução da frequência de amostragem com filtragem antialiasing.
5. Normalização utilizando estatísticas do treinamento.
6. Transformação dos sinais em espectrogramas por STFT.
7. Extração e padronização dos cinco descritores EMG.

## Arquiteturas desenvolvidas

### CNN espectral independente

Modelo principal composto por camadas convolucionais 2D, operações de pooling, agregação global e camadas densas.

A entrada é o espectrograma de uma janela EMG e a saída é uma pontuação de classificação entre 0 e 1.

### CNN espectral com fusão de características

Modelo comparativo com dois ramos:

- CNN 2D para processamento dos espectrogramas.
- MLP para processamento de RMS, ZCR, WL, SSC e MF.

As representações aprendidas são concatenadas antes da classificação final.

## Treinamento

Os modelos foram desenvolvidos em Python com TensorFlow/Keras.

Foram utilizados:

- Função de perda Binary Cross-Entropy.
- Otimizador Adam.
- Pesos de classe para lidar com o desbalanceamento.
- EarlyStopping para interromper o treinamento.
- ReduceLROnPlateau para ajustar a taxa de aprendizado.
- Limiar de classificação de 0,5.

## Resultados

Os resultados abaixo correspondem ao conjunto de validação.

| Métrica | CNN independente | CNN com características |
|---|---:|---:|
| Acurácia | 91,65% | 82,15% |
| Acurácia balanceada | 92,73% | 87,55% |
| Precisão | 76,60% | 58,01% |
| Sensibilidade | 94,86% | 98,13% |
| F1-score | 84,76% | 72,92% |
| AUROC | 98,01% | 97,18% |
| AUPRC | 94,21% | 93,23% |

A CNN espectral independente apresentou o melhor equilíbrio entre precisão e sensibilidade.

A arquitetura com fusão obteve maior sensibilidade, mas apresentou mais falsos positivos, reduzindo sua precisão e seu F1-score.

## Como executar

1. Abra o arquivo `projetofinal_ELA.ipynb` no Google Colab.
2. Instale as dependências indicadas no notebook.
3. Obtenha o dataset EMGLAB N2001 e prepare um arquivo ZIP com os registros.
4. Execute as células em sequência.
5. Quando solicitado, envie o arquivo ZIP.
6. Aguarde o pré-processamento, o treinamento e a avaliação dos modelos.

## Limitações

O experimento utiliza um número reduzido de pacientes independentes, e as janelas extraídas de um mesmo paciente apresentam correlação.

A comparação final entre os dois modelos foi realizada no conjunto de validação. O conjunto de teste havia sido consultado em experimentos exploratórios anteriores, não constituindo uma avaliação final independente dessas arquiteturas.

Os resultados não devem ser interpretados como validação clínica ou evidência de desempenho diagnóstico em novos pacientes.

## Conclusão

Os resultados indicam que a representação espectral dos sinais EMG pode fornecer informações relevantes para a diferenciação entre registros de ELA e controles saudáveis.

A incorporação dos cinco descritores não melhorou o desempenho global no experimento de fusão realizado.

Trabalhos futuros devem considerar conjuntos de dados maiores, validação externa e novas estratégias de integração de características.

## Tecnologias

Python, NumPy, Pandas, SciPy, Scikit-learn, TensorFlow/Keras, Matplotlib, WFDB e Google Colab.

## Contexto acadêmico

Projeto desenvolvido no contexto de estudos de aprendizado de máquina aplicado à saúde, com finalidade acadêmica e experimental.
