**Tradução livre do artigo publicado em** https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html

# Explore o Aprendizado de Máquina Automatizado no Azure Machine Learning

Neste exercício, você usará o recurso de aprendizado de máquina automatizado no Azure Machine Learning para treinar e avaliar um modelo. Em seguida, você implantará e testará o modelo treinado.

## Crie um espaço de trabalho do Azure Machine Learning

Para utilizar o Azure Machine Learning, é necessário provisionar um espaço de trabalho na sua subscrição do Azure. Depois, você poderá utilizar o **Azure Machine Learning Studio** para trabalhar com os recursos do seu workspace.

Entre no portal do Azure em https://portal.azure.com  com suas credenciais da Microsoft.

Selecione **+ Criar recurso**, pesquise **Machine Learning** e crie um novo recurso do **Azure Machine Learning** com as configurações abaixo:
* **Assinatura:** Sua assinatura do Azure.
* **Grupo de recursos:** Crie ou selecione um grupo de recursos.

**Detalhes do Grupo de trabalho**

* **Nome:** Insira um nome exclusivo para seu espaço de trabalho. Nome utilizado nos testes: **mslearn-bike-resource**
* **Região:** Selecione a região geográfica mais próxima, (nos testes, foi utilizada a região **East US 2**, por questões de custo).
* **Conta de armazenamento:** identificação da conta de armazenamento padrão que será criada para seu espaço de trabalho. Para os testes, foi utilizado o padrão gerado.
* **Cofre de chaves:** identificação do cofre de chaves padrão que será criado para seu espaço de trabalho. Para os testes, foi utilizado o padrão gerado.
* **Application insights:** identificação do recurso padrão de insights de aplicativo que será criado para seu espaço de trabalho. Para os testes, foi utilizado o padrão gerado.
* **Registro de container:** Nenhum ( um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner ).
Selecione **Revisar + criar** e após a revisão, selecione **Criar**. Aguarde a criação do seu espaço de trabalho (pode demorar alguns minutos) e, em seguida, vá para o recurso implantado.

Selecione **Iniciar o estúdio** (ou abra uma nova guia do navegador e navegue até https://ml.azure.com e entre no **Azure Machine Learning Studio** usando sua conta da Microsoft). Feche todas as mensagens exibidas.

No **Azure Machine Learning Studio**, você deverá ver seu espaço de trabalho recém-criado. Caso contrário, selecione **Todos espaços de trabalho** no menu à esquerda e selecione o espaço de trabalho que você acabou de criar.

## Use aprendizado de máquina automatizado para treinar um modelo

O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.

1. No **Azure Machine Learning Studio**, procure o item de menu **ML Automatizado** e clique sobre o mesmo.

2. Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Próximo conforme necessário para avançar pela interface do usuário:

**Configurações básicas:**
* **Nome do trabalho**: mslearn-bike-automl
* **Novo nome do experimento**: mslearn-bike-rental
* **Descrição**: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
* **Marcas**: nenhum

**Tipo de tarefa e dados:**
* **Selecionar tipo de tarefa**: Regressão
* **Selecionar os dados**: crie um novo conjunto de dados com as seguintes configurações:

**Criar ativo de dados**

**Tipo de dados**:
* **Nome**: aluguel de bicicletas
* **Descrição**: dados históricos de aluguel de bicicletas
* **Tipo**: Tabular

**Fonte de dados**:
* Selecione **De arquivos da Web**

**URL da Web**:
* **URL da Web**: https://aka.ms/bike-rentals
* **Ignorar validação de dados**: não selecionar

**Configurações**:
* **Formato do arquivo**: Delimitado
* **Delimitador**: Vírgula
* **Codificação**: UTF-8
* **Cabeçalhos de coluna**: Somente o primeiro arquivo tem cabeçalho
* **Ignorar linhas**: Nenhum
* **Conjunto de dados com dados de várias linhas**: não selecione

**Esquema**:
* Incluir todas as colunas exceto **Path**
* Revise os tipos detectados automaticamente

Pressione o botão **Criar**. Após a criação do conjunto de dados, selecione o conjunto de dados **bike-rentals** para continuar a enviar o trabalho de ML automatizado.

**Configurações de tarefas**:

* **Tipo de tarefa**: Regressão
* **Dados**: bike-rentals
* **Coluna de destino**: Rentals (integer)

**Exibir definições de configurações adicionais**:
* **Métrica primária**: NormalizedRootMeanSquareError
* **Explicar o melhor modelo**: Não selecionado
* **Utilizar todos os modelos suportados**: Desmarcado. Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
* **Modelos permitidos**: Selecione apenas **RandomForest** e **LightGBM** — normalmente você gostaria de tentar o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.

**Limites**:
* **Máximo de avaliações**: 3
* **Máximo de avaliações simultâneas**: 3
* **Máximo de nós**: 3
* **Limite de pontuação métrica**: 0,085 ( para que, se um modelo atingir uma pontuação da métrica de erro quadrático médio normalizado de 0,085 ou menos, o trabalho termina. )
* **Tempo limite do experimento (minutos)**: 15
* **Tempo limite de iteração (minutos)**: 15
* **Habilitar encerramento antecipado**: selecionado

**Validar e testar**:
* **Tipo de validação:** divisão de validação de treinamento
* **Validação de percentual de dados:** 10. O ML Automatizado recomenda entre 10 e 30 por cento dos dados sejam mantidos para validação.
* **Dados de teste:** divisão de validação de treinamento
* **Teste percentual de dados:** 10. O ML Automatizado recomenda entre 10 e 30 por cento dos dados sejam mantidos para teste.

**Computação**:
* **Selecione o tipo de computação:** sem servidor
* **Tipo de máquina virtual:** CPU
* **Camada de máquina virtual:** Dedicada
* **Tamanho da máquina virtual:** Standard_DS3_V2*
* **Número de instâncias:** 1 

*Se a sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

3. Envie o trabalho de treinamento. Ele inicia automaticamente.

4. Espere o trabalho terminar. Pode demorar um pouco – agora pode ser um bom momento para uma pausa para o café!

## Avalie o melhor modelo
Quando o trabalho automatizado de aprendizado de máquina for concluído, você poderá revisar o melhor modelo treinado.

1. Na guia Visão geral do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo.

2. Selecione o texto em **Nome do algoritmo** do melhor modelo para visualizar seus detalhes.

3. Selecione a guia **Métricas** e selecione os gráficos **residuais** e **predito_true** se eles ainda não estiverem selecionados.

4. Revise os gráficos que mostram o desempenho do modelo. O **gráfico de resíduos** mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O **gráfico predito_true** compara os valores previstos com os valores verdadeiros.

## Implantar e testar o modelo
Na guia **Modelo** do melhor modelo treinado pelo seu trabalho automatizado de machine learning, selecione **Implantar** e use a opção de **serviço Web** para implantar o modelo com as seguintes configurações:
* **Nome:** predict-rentals
* **Descrição:** Prever aluguel de bicicletas
* **Tipo de computação:** Instância de Contêiner do Azure
* **Habilitar autenticação:** selecionado
Aguarde o início da implantação – isso pode levar alguns segundos. O **status de implantação** do endpoint de **predict-rentals** será indicado na parte principal da página como **Em Execução**.
Aguarde até que o status da implantação mude para **Finalizado**. Isso pode levar de 5 a 10 minutos. *Nos testes feitos, demorou 1 hora.

## Testar o serviço implantado
Agora você pode testar seu serviço implantado.

No **Azure Machine Learning Studio**, no menu esquerdo, selecione **Pontos de extremidade**; em Pontos de extremidade em tempo real, selecione o endpoint **predict-rentals**.

Na página do endpoint **predict-rentals**, navegue até a guia Testar.

No painel Dados de entrada para testar o endpoint , substitua o modelo JSON pelos seguintes dados de entrada:

```
 {
   "Inputs": { 
     "data": [
       {
         "day": 1,
         "mnth": 1,   
         "year": 2022,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }
 ```

Clique no botão **Testar.**

Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhantes a este:

```
 {
   "Results": [
     444.27799000000000
   ]
 }
```

O painel de teste pegou os dados de entrada e usou o modelo treinado para retornar o número previsto de aluguéis.

Vamos revisar o que você fez. Você usou um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de alugueres de bicicletas esperados num determinado dia, com base em características sazonais e meteorológicas.

## Limpar
O serviço web que você criou está hospedado em uma instância de contêiner do Azure . Se não pretender experimentá-lo ainda mais, deverá eliminar o ponto final para evitar acumular utilização desnecessária do Azure.

1. No **Azure Machine Learning Studio**, na guia **pontos de extremidade**, selecione o ponto de extremidade **predict-rentals**. Em seguida, selecione **Excluir** e confirme que deseja excluir o endpoint.

Excluir sua computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, será cobrada uma pequena quantia pelo armazenamento de dados, desde que o espaço de trabalho do Azure Machine Learning exista na sua assinatura. Se tiver terminado de explorar o Azure Machine Learning, poderá eliminar o espaço de trabalho Azure Machine Learning e os recursos associados.

### Para excluir seu espaço de trabalho:

1. No portal Azure, na página **Grupos de recursos**, abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.
2. Clique em **Excluir grupo de recursos**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**.
