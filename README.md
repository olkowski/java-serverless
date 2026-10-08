## Criando uma Role para AWS Lambda

Para que uma função Lambda consiga executar e acessar outros serviços da AWS, é necessário criar uma **IAM Role** com permissões adequadas.

### Passo a passo

1. **Acesse o IAM Console**
   - Vá até [IAM Console](https://console.aws.amazon.com/iam/).

2. **Crie uma nova Role**
   - Clique em **Roles → Create role**.
   - Em **Trusted entity type**, selecione **AWS service**.
   - Em **Use case**, escolha **Lambda**.
   - Clique em **Next**.

3. **Anexe permissões**
   - Para logs no CloudWatch: adicione a política `AWSLambdaBasicExecutionRole`.
   - Para acesso a outros serviços (exemplo: S3, DynamoDB), adicione as políticas correspondentes (`AmazonS3FullAccess`, `AmazonDynamoDBFullAccess`, etc.).

4. **Nomeie a Role**
   - Dê um nome claro, como `LambdaExecutionRole`.
   - Clique em **Create role**.

5. **Associe a Role à Lambda**
   - Vá até o **Lambda Console**.
   - Abra sua função.
   - Em **Configuration → Permissions**, edite a **Execution role** e selecione a Role criada.

## Configuração da Role da Lambda

Após criar a Role no IAM e obter a sua **ARN**, exporte-a como variável de ambiente para que possa ser utilizada no projeto:

```bash
export LAMBDA_ROLE_ARN="arn:aws:iam::123456789012:role/LambdaExecutionRole"
```

No PowerShell, use:

```powershell
$env:LAMBDA_ROLE_ARN="arn:aws:iam::123456789012:role/LambdaExecutionRole"
```

## Script de gerenciamento da Lambda

O projeto possui o script `manage.sh` para build, deploy, invocacao e remocao da funcao Lambda.

### Pre-requisitos

- AWS CLI configurado (`aws configure`)
- Credenciais com permissao para Lambda e IAM PassRole
- Java 21 e Maven Wrapper do projeto

### Variaveis de ambiente

- `LAMBDA_ROLE_ARN` (obrigatoria para deploy)
- `LAMBDA_FUNCTION_NAME` (opcional, padrao: `lambda-fiap-hello`)
- `AWS_REGION` (opcional, padrao: `us-east-1`)
- `LAMBDA_TIMEOUT` (opcional, padrao: `15`)
- `LAMBDA_MEMORY_SIZE` (opcional, padrao: `512`)

### Comandos

```bash
./manage.sh build
./manage.sh deploy
./manage.sh invoke
./manage.sh delete
./manage.sh help
```

### Fluxo recomendado

1. Exporte a role (`LAMBDA_ROLE_ARN`).
2. Execute `./manage.sh deploy`.
3. Teste com `./manage.sh invoke`.
4. Verifique a resposta em `response.json`.

