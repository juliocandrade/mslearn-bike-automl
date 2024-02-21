# Trabalhando com Machine Learning na Prática do Azure ML

Para a realização do trabalho, foram seguidos os passos descritos no site **[Microsoft Learn](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html)**

O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Nesse passo a passo, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.

# Uso

Criar um novo trabalho automatizado, treinar, criar modelo, implantar, testar o modelo e o serviço implantado.

# Processo

<details>
<summary>Crie um espaço de trabalho do Azure Machine Learning</summary>
           
1. Entre no portal do **[Azure](https://azure.microsoft.com)** com suas credenciais da Microsoft;
2. Selecione **+ Criar um recurso**, pesquise Machine Learning e crie um novo recurso do **Azure Machine Learning** com as seguintes configurações:
    - **Assinatura**: sua assinatura do Azure;
    - **Grupo de recursos**: Crie ou selecione um grupo de recursos;
    - **Nome**: Insira um nome exclusivo para seu espaço de trabalho;
    - **Região**: Selecione a região geográfica mais próxima;
    - **Conta de armazenamento**: observe a nova conta de armazenamento padrão que será criada para seu espaço de trabalho;
    - **Cofre de chaves**: Observe o novo cofre de chaves padrão que será criado para seu espaço de trabalho;
    - **Insights de aplicativos**: observe o novo recurso padrão de insights de aplicativos que será criado para seu espaço de 
    trabalho;
    - **Registro de contêiner**: Nenhum ( será criado um automaticamente na primeira vez que você implantar um modelo em um contêiner);
3. Selecione **Revisar + criar** e selecione **Criar**. Aguarde a criação do seu espaço de trabalho (pode demorar alguns minutos) e, em 
    seguida, vá para o recurso implantado;
4. Selecione **Launch Studio** (ou abra uma nova guia do navegador e navegue até **[Azure Machine Learning Studio](https://ml.azure.com)** usando sua conta da Microsoft). Feche todas as mensagens exibidas;
5. No estúdio Azure Machine Learning, você deverá ver seu espaço de trabalho recém-criado. Caso contrário, selecione **Todos os espaços de trabalho** no menu à esquerda e selecione o espaço de trabalho que você acabou de criar.


</details>
<details>
<summary>Use aprendizado de máquina automatizado para treinar um modelo</summary>

1. No Azure Machine Learning Studio, veja a página Automated ML **(em Authoring)**.

2. Crie um novo trabalho de ML automatizado com as seguintes configurações, usando **Next** conforme necessário para avançar pela interface do usuário:

     ### Configurações básicas:

   - **Nome do trabalho**: mslearn-bike-automl;
   - **Novo nome do experimento**: mslearn-bike-rental;
   - **Descrição**: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas;
   - **Marcadores**: nenhum;

        
        #### Tipo de tarefa e dados:

        - **Selecione o tipo de tarefa**: Regressão;
        - **Selecionar conjunto de dados**: crie um novo conjunto de dados com as seguintes configurações:
      
 
        #### Tipo de dados:
     
        - **Nome**: bike-rentals;
        - **Descrição**: dados históricos de aluguel de bicicletas;
        - **Tipo**: Tabular;

        #### Fonte de dados:
     
        - Selecione **"Dos arquivos da web"**;

        #### URL da Web:

        - **URL da Web**: https://aka.ms/bike-rentals;
        - **Ignorar validação de dados**: não selecionar;
          
        #### Configurações:

        - **Formato de arquivo**: Delimitado;
        - **Delimitador**: Vírgula;
        - **Codificação**: UTF-8;
        - **Cabeçalhos de coluna**: apenas o primeiro arquivo possui cabeçalhos;
        - **Pular linhas**: Nenhum;
        - **O conjunto de dados contém dados multilinhas**: não selecione;

        #### Esquema:

        - Incluir todas as colunas exceto **Caminho**;
        - Revise os tipos detectados automaticamente;

       #### Criar:

        - Selecione **Criar**. Após a criação do conjunto de dados, selecione o conjunto de dados de **bike-rentals** para continuar a enviar o trabalho de ML automatizado;

       ##### Configurações de tarefas:
  
       - **Tipo de tarefa**: Regressão;
       - **Conjunto de dados**: bike-rentals;
       - **Coluna de destino**: Rentals (integer);

       ### Configurações adicionais:

       - **Métrica primária**: raiz do erro quadrático médio normalizado;
       - **Explique o melhor modelo**: Não selecionado;
       - **Usar todos os modelos suportados**: Desmarcado. Você restringirá o trabalho para tentar apenas alguns algoritmos 
       específicos;
       - **Modelos permitidos**: Selecione apenas **RandomForest** e **LightGBM** — normalmente você gostaria de tentar o máximo possível, mas 
       cada modelo adicionado aumenta o tempo necessário para executar o trabalho;

       #### Limites: Expanda a seção
    
       - **Máximo de testes**: 3;
       - **Máximo de testes simultâneos**: 3;
       - **Máximo de nós**: 3;
       - **Limite de pontuação da métrica**: 0,085 ( para que, se um modelo atingir uma pontuação da métrica de erro quadrático médio normalizado de 0,085 ou menos, o trabalho termina);
       - **Tempo limite**: 15;
       - **Tempo limite de iteração**: 15;
       - **Habilitar rescisão antecipada**: selecionado;

        #### Validação e teste:

       - Tipo de validação: divisão de validação de treinamento;
       - Porcentagem de dados de validação: 10;
       - Conjunto de dados de teste: Nenhum;

       #### Calcular:

      - **Selecione o tipo de computação**: sem servidor;
      - **Tipo de máquina virtual**: CPU;
      - **Camada de máquina virtual**: Dedicada;
      - **Tamanho da máquina virtual**: Standard_DS3_V2*;
      - **Número de instâncias**: 1;
      
3. Envie o trabalho para treinamento. Ele iniciará automaticamente.
4. Espere o trabalho terminar. Pode demorar um pouco.

</details>
<details>
<summary>Avalie o melhor modelo</summary>
  
Quando o trabalho automatizado de aprendizado de máquina for concluído, você poderá revisar o melhor modelo treinado.

1. Na guia **Visão geral** do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo.
   ![imagem de status de modelo](https://github.com/juliocandrade/mslearn-bike-automl/assets/66694754/666b0438-a38f-41ea-95c3-06764487c378)
>[!WARNING]
> Você poderá ver uma mensagem com o status “Aviso: pontuação de saída especificada pelo usuário alcançada…”. Esta é uma mensagem esperada. Continue para a próxima etapa.
2. Selecione o texto em **Nome do algoritmo** do melhor modelo para visualizar seus detalhes.
3. Selecione a guia **Métricas** e selecione os gráficos **residuais** e **predito_true** se eles ainda não estiverem selecionados.
   Revise os gráficos que mostram o desempenho do modelo. O gráfico de **resíduos** mostra os *resíduos* (as diferenças entre os valores previstos e reais) como um histograma. O gráfico predito_true compara os valores previstos com os valores verdadeiros.
</details>
<details>
<summary>Implantar e testar o modelo</summary>

1. Na guia **Modelo** do melhor modelo treinado pelo seu trabalho automatizado de machine learning, selecione **Implantar** e use a opção de **serviço Web** para implantar o modelo com as seguintes configurações:
      - **Nome**: prever-aluguéis;
      - **Descrição**: Prever aluguel de bicicletas;
      - **Tipo de computação**: Instância de Contêiner do Azure;
      - **Habilitar autenticação**: selecionado;
2. Aguarde o início da implantação – isso pode levar alguns segundos. O **status de implantação** do endpoint de **previsão de aluguel** será indicado na parte principal da página como Running.
3. Aguarde até que o **status da implantação** mude para Succeeded. Isso pode levar de 5 a 10 minutos.
</details>
<details>
<summary>Testar o serviço implantado</summary>

Agora você pode testar seu serviço implantado.

1. No estúdio Azure Machine Learning, no menu esquerdo, selecione **Endpoints** e abra o ponto final em tempo real de previsão de aluguel.
2. Na página do endpoint em tempo real de **previsão de aluguel**, visualize a guia **Teste**.
3. No painel **Dados de entrada para testar o endpoint**, substitua o modelo **JSON** pelos seguintes dados de entrada:

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

4. Clique no botão **Testar**.
5. Revise os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada - semelhante a este:

                                           {
                                             "Results": [
                                               444.27799000000000
                                             ]
                                           }

   O painel de teste pegou os dados de entrada e usou o modelo treinado para retornar o número previsto de aluguéis.

Vamos revisar o que você fez. Você usou um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número de alugueis de bicicletas esperados num determinado dia, com base em características sazonais e meteorológicas.
</details>
<details>
<summary>Excluir</summary>

O serviço web que você criou está hospedado em uma *instância de contêiner do Azure*. Se não pretender experimentá-lo ainda mais, deverá excluir para evitar cobrança desnecessária de recursos do Azure.

1. No [Studio Azure Machine Learning](https://ml.azure.com/?azure-portal=true), na guia **Endpoints**, selecione o ponto de **extremidade de previsão de aluguel**. Em seguida, selecione **Excluir** e confirme que deseja excluir o endpoint.
2. Excluir sua computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, será cobrada uma pequena quantia pelo armazenamento de dados, desde que o espaço de trabalho do Azure Machine Learning exista na sua assinatura. Se tiver terminado de explorar o Azure Machine Learning, poderá eliminar o espaço de trabalho Azure Machine Learning e os recursos associados.

Para excluir seu espaço de trabalho:

1. No [portal Azure](https://portal.azure.com/?azure-portal=true), na página **Grupos de recursos**, abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.
1. Clique em **Excluir grupo de recursos**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione **Excluir**.
</details>
