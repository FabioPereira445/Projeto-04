# Projeto-04
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Pipeline de Dados com Telegram</title>
</head>
<body>

<h1>Pipeline de Dados com Telegram</h1>

<h2>Índice</h2>
<ol>
    <li><a href="#introducao">Introdução</a></li>
    <li><a href="#ingestao">Ingestão</a>
        <ul>
            <li><a href="#aws-s3">AWS S3</a></li>
            <li><a href="#aws-lambda">AWS Lambda</a></li>
            <li><a href="#aws-api-gateway">AWS API Gateway</a></li>
            <li><a href="#telegram">Telegram</a></li>
        </ul>
    </li>
    <li><a href="#etl">ETL (Extract, Transform, Load)</a>
        <ul>
            <li><a href="#aws-s3-2">AWS S3</a></li>
            <li><a href="#aws-lambda-2">AWS Lambda</a></li>
            <li><a href="#aws-eventbridge">AWS EventBridge</a></li>
        </ul>
    </li>
    <li><a href="#apresentacao">Apresentação</a>
        <ul>
            <li><a href="#aws-athena">AWS Athena</a></li>
        </ul>
    </li>
    <li><a href="#conclusao">Conclusão</a></li>
</ol>

<h2 id="introducao">Introdução</h2>
<p>Imagine poder coletar dados em tempo real de conversas no Telegram e transformar essas informações em insights valiosos. O projeto "Pipeline de Dados com Telegram" tem exatamente esse objetivo: construir um pipeline de dados que coleta mensagens do Telegram, as armazena, processa e apresenta de maneira organizada e acessível. Utilizando uma série de serviços da AWS, o projeto demonstra como ingerir, processar e apresentar dados de forma eficiente e escalável.</p>
<img src="images/introducao_4.1.png">
<h2 id="ingestao">1. Ingestão</h2>

<h3 id="aws-s3">1.1. AWS S3</h3>
<p>Nosso primeiro passo foi criar um bucket no AWS S3, batizado de <code>mod-42-ebac-datalake-raw</code>, destinado ao armazenamento de dados crus. Este bucket é onde todas as mensagens coletadas do Telegram são inicialmente armazenadas em seu formato bruto.</p>
<img src="images/1.1. AWS S3.png">
<h3 id="aws-lambda">1.2. AWS Lambda</h3>
<p>Para processar as mensagens do Telegram, desenvolvemos uma função Lambda chamada <code>mod-42-ebac-datalake-raw</code>. Essa função recebe as mensagens via AWS API Gateway e as salva no bucket de dados crus do S3 em formato JSON. As variáveis de ambiente e permissões necessárias foram configuradas para garantir que a função Lambda possa interagir com o S3.</p>
<img src="images/AWS Lambda1.png">
<img src="images/AWS Lambda2.png">
<img src="images/AWS Lambda3.png">
<h3 id="aws-api-gateway">1.3. AWS API Gateway</h3>
<p>Criamos uma API no AWS API Gateway, que serve como o ponto de entrada para as mensagens do Telegram, garantindo que todas as mensagens sejam recebidas e processadas de forma eficiente pela função Lambda.</p>
<img src="images/AWS API Gateway1.png">
<img src="images/AWS API Gateway2.png">
<img src="images/AWS API Gateway3.png">
<h3 id="telegram">1.4. Telegram</h3>
<p>Finalmente, configuramos o webhook do bot do Telegram para redirecionar as mensagens para a URL da API do AWS API Gateway, permitindo que as mensagens enviadas ao bot sejam automaticamente capturadas e processadas pelo pipeline.</p>
<img src="images/telegram1.png">
<img src="images/telegram2.png">
<hr>
<h4> Arquitetura do Pipeline de Dados com Telegram</h4>
<img src="images/Arquitetura do Pipeline.jpg">
<hr>

<h2 id="etl">2. ETL (Extract, Transform, Load)</h2>

<h3 id="aws-s3-2">2.1. AWS S3</h3>
<p>Nesta etapa, criamos um novo bucket no AWS S3 chamado <code>mod-42-ebac-datalake-enriched</code> para armazenar os dados enriquecidos, que já passaram por um processo de transformação e estão prontos para análise.</p>
<img src="images/S3-ENRICHED.png">
<h3 id="aws-lambda-2">2.2. AWS Lambda</h3>
<p>Desenvolvemos uma segunda função Lambda chamada <code>mod-42-ebac-datalake-enriched2</code>. Esta função processa as mensagens JSON do bucket de dados crus, transforma os dados e os armazena no bucket de dados enriquecidos no formato Parquet.</p>
<img src="images/2_2AWS Lambda.png">

<h3 id="aws-eventbridge">2.3. AWS EventBridge</h3>
<p>Criamos uma regra no AWS EventBridge para executar a função Lambda diariamente à meia-noite, no horário de Brasília (GMT-3). Essa automação garante que o pipeline de ETL seja executado regularmente, mantendo os dados sempre atualizados.</p>

<hr>
<h4>Imagem 2: Estrutura do Bucket no S3 para dados enriquecidos</h4>
<img src="images/S3-ENRICHED.png">
<hr>

<h2 id="apresentacao">3. Apresentação</h2>

<h3 id="aws-athena">3.1. AWS Athena</h3>
<p>Para apresentar os dados de forma acessível, utilizamos o AWS Athena. Criamos uma tabela externa no Athena que aponta para os dados armazenados no bucket enriquecido do S3. Isso permite que os usuários façam consultas SQL diretamente nos dados transformados.</p>

<h3>3.2. MSCK REPAIR TABLE</h3>
<p>Executamos o comando <code>MSCK REPAIR TABLE</code> para carregar as partições da tabela no Athena, garantindo que todos os dados estejam disponíveis para consulta.</p>

<hr>
<h4>Imagem 3: Tabela no AWS Athena</h4>
<img src="URL_da_Imagem" alt="Tabela no AWS Athena">
<hr>

<h2 id="conclusao">Conclusão</h2>
<p>O projeto "Pipeline de Dados com Telegram" mostra como é possível coletar, processar e apresentar dados de maneira eficiente utilizando serviços da AWS. Desde a ingestão automática de dados do Telegram até o processamento e armazenamento dos dados enriquecidos, o pipeline fornece uma solução robusta e escalável para a análise de dados em tempo real.</p>

</body>
</html>
