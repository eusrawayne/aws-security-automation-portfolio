# Guia de Setup Inicial

## 📝 Visão Geral

Este guia irá orientá-lo na configuração do ambiente AWS necessário para implementar os projetos deste portfólio.

## ⚙️ Pré-requisitos

### Conta AWS
1. Crie uma conta AWS (Free Tier disponível)
2. Acesse: https://aws.amazon.com/free/
3. Configure MFA para conta root (obrigatório para segurança)

### Ferramentas Locais

#### AWS CLI
```bash
# Linux/Mac
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Windows
# Baixe o instalador: https://awscli.amazonaws.com/AWSCLIV2.msi

# Verificar instalação
aws --version
```

#### Python 3.9+
```bash
# Verificar versão
python3 --version

# Instalar pip
python3 -m pip install --upgrade pip

# Instalar boto3
pip install boto3
```

## 🔑 Configuração de Credenciais AWS

### 1. Criar Usuário IAM

1. Acesse AWS Console → IAM
2. Crie um novo usuário: `security-automation-user`
3. Tipo de acesso: **Acesso programático**
4. Anexe políticas:
   - `AdministratorAccess` (apenas para ambiente de testes)
   - Para produção, use políticas mais restritivas

### 2. Configurar AWS CLI

```bash
aws configure
# AWS Access Key ID: [Sua Access Key]
# AWS Secret Access Key: [Sua Secret Key]
# Default region name: us-east-1
# Default output format: json
```

### 3. Testar Configuração

```bash
# Verificar identidade
aws sts get-caller-identity

# Listar buckets S3 (deve retornar lista vazia ou seus buckets)
aws s3 ls
```

## 🏭 Configuração Inicial da AWS

### 1. Habilitar CloudTrail

```bash
# Criar bucket para logs do CloudTrail
aws s3 mb s3://seu-nome-cloudtrail-logs-$(aws sts get-caller-identity --query Account --output text)

# Criar CloudTrail
aws cloudtrail create-trail \
    --name security-automation-trail \
    --s3-bucket-name seu-nome-cloudtrail-logs-$(aws sts get-caller-identity --query Account --output text)

# Iniciar logging
aws cloudtrail start-logging --name security-automation-trail
```

### 2. Configurar SNS para Notificações

```bash
# Criar tópico SNS
aws sns create-topic --name security-alerts

# Subscrever seu email
aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:SEU_ACCOUNT_ID:security-alerts \
    --protocol email \
    --notification-endpoint seu-email@exemplo.com

# Confirme a inscrição no email recebido
```

### 3. Criar Bucket S3 para Artefatos

```bash
# Criar bucket para armazenar código Lambda e logs
aws s3 mb s3://seu-nome-security-automation-artifacts-$(aws sts get-caller-identity --query Account --output text)

# Habilitar versionamento
aws s3api put-bucket-versioning \
    --bucket seu-nome-security-automation-artifacts-$(aws sts get-caller-identity --query Account --output text) \
    --versioning-configuration Status=Enabled
```

## 🛠️ Ferramentas Opcionais

### SAM CLI (para desenvolvimento Lambda local)
```bash
pip install aws-sam-cli
sam --version
```

### Terraform (para IaC alternativo)
```bash
# Mac
brew install terraform

# Linux
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
unzip terraform_1.6.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

## ✅ Checklist de Setup

- [ ] Conta AWS criada e MFA configurado
- [ ] AWS CLI instalado e configurado
- [ ] Python 3.9+ instalado
- [ ] Boto3 instalado
- [ ] CloudTrail habilitado
- [ ] SNS tópico criado e email inscrito
- [ ] Bucket S3 para artefatos criado
- [ ] Credenciais AWS testadas com sucesso

## 📚 Próximos Passos

Após concluir este setup:
1. Escolha um projeto da Fase 1 para começar
2. Leia o README específico do projeto
3. Siga o guia de deployment do projeto

## ⚠️ Segurança

**IMPORTANTE:**
- Nunca commite credenciais AWS no Git
- Use `.gitignore` para arquivos sensíveis
- Sempre use MFA na conta root
- Revise permissões IAM regularmente
- Monitore custos no AWS Billing Dashboard

## 🔗 Recursos Úteis

- [AWS Free Tier](https://aws.amazon.com/free/)
- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
- [Boto3 Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [AWS Security Best Practices](https://docs.aws.amazon.com/security/)