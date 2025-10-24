
**Implementação de um Fluxo de Automação Serverless com AWS e LocalStack**

Este projeto apresenta a implementação de uma tarefa automatizada utilizando os serviços da AWS: **Amazon S3, AWS Lambda e Amazon DynamoDB**. Para o desenvolvimento e testes, a sugestão é utilizar o **LocalStack**, uma ferramenta open source que simula toda a infraestrutura da AWS localmente, garantindo um ambiente de desenvolvimento prático e totalmente sem custos.

O objetivo é demonstrar a construção de uma **Arquitetura Orientada a Eventos**, onde a simples ação de subir um arquivo dispara um processo de automação completo, eliminando a necessidade de servidores dedicados ou agendamentos complexos.

**Prática:** 

O desenvolvimento desta solução foi integralmente realizado, e a sua funcionalidade é comprovada pelas ***capturas de tela e imagens*** anexadas. Essas evidências visuais acompanham as etapas, desde a criação dos recursos via AWS CLI até a verificação do processamento nos logs do LocalStack.

**A Arquitetura em Ação: Fluxo Orientado a Eventos** 

A ***estrutura fundamental*** deste projeto é o conceito **“Serverless”** e o ***desacoplamento*** dos serviços, garantindo que cada componente funcione de forma independente, mas coordenada:

1. **O Gatilho (Amazon S3)**:
 O processo se inicia com o ***Amazon S3***, utilizado como o nosso storage de entrada. A cada novo arquivo enviado, o S3 emite uma notificação para o próximo componente.

2. **O Processamento (AWS Lambda)**: 
Esta notificação é o **gatilho** que invoca a nossa função AWS Lambda. O ***Lambda***, sendo ***serverless***, executa o código Python sob demanda, sem que precisemos gerenciar qualquer servidor.

3. **O Registro (Amazon DynamoDB)**: 
Após o Lambda processar o arquivo, ele registra o resultado (o log de processamento) no ***Amazon DynamoDB***. Este banco de dados NoSQL de alta performance garante que o registro da automação seja rápido e extremamente escalável.

***O resultado é um fluxo eficiente:***

**Upload** →  **Processamento de Código** →  **Registro de Log**.



**Destaque Técnico: Desenvolvimento Prático com LocalStack** 

A escolha do **LocalStack** como ambiente de desenvolvimento é um diferencial que eleva a qualidade do projeto, focando em práticas ágeis e econômicas:

**Ambiente Espelhado**: O LocalStack espelha as APIs da AWS, permitindo que a criação de buckets S3, tabelas DynamoDB e funções Lambda seja feita exatamente com os mesmos comandos do ***AWS CLI*** que seriam usados em produção. (Veja a imagem: Criação do Bucket S3/ s3_bucket_01.png)

**Zero Custo e Testes Ilimitados**: Elimina custos de consumo de nuvem e permite a execução de testes complexos em um ***ambiente de sandbox*** completamente isolado, validando a funcionalidade antes do deploy final.

***A Lógica de Negócio (Função Lambda)***

O **módulo central da automação** reside no código Python da função Lambda. Sua responsabilidade é clara e objetiva, funcionando como a **Unidade de Inteligência do Fluxo**:

**Recepção e Identificação do Evento**: A função recebe o pacote de dados do S3 e extrai as coordenadas cruciais do arquivo (Nome do Bucket e Chave do Objeto).

**Processamento do Arquivo**: Utilizando o **SDK Boto3**, o código acessa o S3 para ler o conteúdo do arquivo. Ele executa a lógica de negócio definida (ex. validação do formato, contagem de linhas e extração de metadados importantes).

**Persistência do Log no DynamoDB**: Por fim, o Lambda insere um novo item na tabela DynamoDB. O registro contém informações essenciais como o nome do arquivo, o status final (Sucesso ou Falha) e as métricas extraídas (como número de linhas ou tamanho). 

Com essa arquitetura, demonstramos a capacidade de construir sistemas **escaláveis, auto-acionados e auditáveis**, que são a base da moderna engenharia de software na nuvem.



