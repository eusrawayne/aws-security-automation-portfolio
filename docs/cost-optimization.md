# Guia de Otimização de Custos AWS

## 💰 Visão Geral

Este guia ajuda a manter os custos dos projetos dentro do Free Tier da AWS e fornece dicas para otimizar gastos.

## 🆓 AWS Free Tier - Limites Relevantes

### Compute
- **Lambda**: 1 milhão de requisições gratuitas/mês
- **Lambda**: 400.000 GB-segundos de tempo de computação/mês

### Storage
- **S3**: 5 GB de armazenamento padrão
- **S3**: 20.000 requisições GET
- **S3**: 2.000 requisições PUT

### Monitoring
- **CloudWatch Logs**: 5 GB de ingestão
- **CloudWatch**: 10 métricas customizadas
- **CloudWatch Alarms**: 10 alarmes

### Messaging
- **SNS**: 1.000 publicações gratuitas/mês
- **SNS**: 100.000 notificações HTTP/HTTPS
- **SNS**: 1.000 notificações por email

### Security & Management
- **CloudTrail**: 1 trail gratuito
- **Step Functions**: 4.000 transições de estado/mês

## 🚨 Custos Fora do Free Tier

### Serviços Pagos (Mesmo no Free Tier)

#### EventBridge
- **Custo**: $1.00 por milhão de eventos
- **Estimativa para este projeto**: ~$0.10/mês

#### NAT Gateway (se usado)
- **Custo**: ~$0.045/hora + transferência de dados
- **Estimativa**: ~$32/mês
- **⚠️ Evite usar se possível!**

#### EBS Snapshots
- **Custo**: $0.05 por GB-mês
- **Estimativa**: $0.50-$2/mês (10-40 GB)

## 🛡️ Estratégias de Otimização

### 1. Lambda

**Otimizações:**
```python
# Reutilize conexões fora do handler
import boto3
client = boto3.client('ec2')  # Global, reutilizado

def lambda_handler(event, context):
    # Use o cliente global
    response = client.describe_instances()
```

**Configuração de Memória:**
- Use 128-256 MB para funções simples
- Monitore uso real e ajuste
- Mais memória = mais CPU, pode ser mais rápido e barato

### 2. CloudWatch Logs

**Retenção de Logs:**
```bash
# Configurar retenção de 7 dias (ao invés de "nunca expirar")
aws logs put-retention-policy \
    --log-group-name /aws/lambda/sua-funcao \
    --retention-in-days 7
```

**Filtragem de Logs:**
```python
# Evite logs excessivos em produção
import logging
logger = logging.getLogger()
logger.setLevel(logging.WARNING)  # Só WARNING e ERROR
```

### 3. S3

**Lifecycle Policies:**
```json
{
  "Rules": [{
    "Id": "DeleteOldSnapshots",
    "Status": "Enabled",
    "Prefix": "forensics/",
    "Expiration": {
      "Days": 30
    }
  }]
}
```

**Comandos:**
```bash
aws s3api put-bucket-lifecycle-configuration \
    --bucket seu-bucket \
    --lifecycle-configuration file://lifecycle.json
```

### 4. EBS Snapshots

**Limpeza Automatizada:**
```python
# Lambda para deletar snapshots antigos
import boto3
from datetime import datetime, timedelta

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    # Deletar snapshots com mais de 30 dias
    snapshots = ec2.describe_snapshots(OwnerIds=['self'])
    
    for snapshot in snapshots['Snapshots']:
        start_time = snapshot['StartTime'].replace(tzinfo=None)
        if datetime.now() - start_time > timedelta(days=30):
            ec2.delete_snapshot(SnapshotId=snapshot['SnapshotId'])
```

### 5. Step Functions

**Otimizações:**
- Use Step Functions Express para workflows de curta duração
- Consolide estados quando possível
- Evite polling frequente

## 📊 Monitoramento de Custos

### 1. Configurar Budget Alert

```bash
# Criar alerta de orçamento de $5
aws budgets create-budget \
    --account-id $(aws sts get-caller-identity --query Account --output text) \
    --budget file://budget.json
```

**budget.json:**
```json
{
  "BudgetName": "Security-Automation-Budget",
  "BudgetLimit": {
    "Amount": "5",
    "Unit": "USD"
  },
  "TimeUnit": "MONTHLY",
  "BudgetType": "COST"
}
```

### 2. Cost Explorer

1. Acesse: AWS Console → Cost Explorer
2. Ative (pode levar 24h)
3. Crie relatórios customizados
4. Agrupe por serviço

### 3. Billing Alarms

```bash
# Criar alarme no CloudWatch
aws cloudwatch put-metric-alarm \
    --alarm-name billing-alarm \
    --alarm-description "Alert when charges exceed $5" \
    --metric-name EstimatedCharges \
    --namespace AWS/Billing \
    --statistic Maximum \
    --period 21600 \
    --threshold 5 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 1
```

## 📅 Checklist de Revisão Mensal

- [ ] Revisar Cost Explorer
- [ ] Deletar snapshots antigos não utilizados
- [ ] Verificar logs do CloudWatch (retenção)
- [ ] Limpar objetos S3 desnecessários
- [ ] Revisar funções Lambda não utilizadas
- [ ] Verificar instâncias EC2 ativas
- [ ] Desativar recursos de testes

## ⚠️ Recursos para Deletar Após Testes

```bash
# Deletar Stack CloudFormation
aws cloudformation delete-stack --stack-name nome-do-stack

# Deletar funções Lambda
aws lambda delete-function --function-name nome-funcao

# Deletar snapshots
aws ec2 delete-snapshot --snapshot-id snap-xxxxx

# Esvaziar e deletar bucket S3
aws s3 rm s3://seu-bucket --recursive
aws s3 rb s3://seu-bucket
```

## 📊 Custos Estimados por Projeto

### Fase 1 - Projetos IAM
- **IAM Access Denied Responder**: < $0.50/mês
- **Force User MFA**: < $0.20/mês
- **CloudTrail Remediation**: < $0.30/mês

**Total Fase 1**: ~$1.00/mês

### Fase 2 - Incident Response
- **EC2 Forensics**: $1-3/mês (depende de snapshots)

**Total Geral**: $2-4/mês

## 💡 Dicas Finais

1. **Sempre delete recursos de teste** quando não estiver usando
2. **Configure alertas de billing** antes de começar
3. **Use tags** em todos os recursos (Project:SecurityAutomation)
4. **Revise custos semanalmente** durante desenvolvimento
5. **Prefira recursos serverless** (Lambda vs EC2)

## 🔗 Recursos

- [AWS Free Tier](https://aws.amazon.com/free/)
- [AWS Pricing Calculator](https://calculator.aws/)
- [AWS Cost Optimization](https://aws.amazon.com/pricing/cost-optimization/)