# AWS Security Automation Portfolio 🔒☁️

Portfólio de projetos práticos focados em **automação de segurança** e **resposta a incidentes** na AWS, demonstrando competências em DevSecOps, segurança cloud e automação de processos de cibersegurança.

## 🎯 Objetivo

Este repositório documenta minha jornada aprendendo e implementando soluções reais de segurança automatizada na AWS, cobrindo desde controles de IAM até resposta completa a incidentes de segurança com análise forense automatizada.

## 📋 Projetos Implementados

### Fase 1: Fundamentos de Automação de Segurança IAM

#### 🚫 IAM Access Denied Responder
Sistema automatizado que monitora e responde a eventos de acesso negado em tempo real.

**Tecnologias:** CloudTrail, CloudWatch Events, Lambda, SNS  
**Status:** 🚧 Em Desenvolvimento  
**[Ver Projeto →](./fase-1-iam-security-automation/iam-access-denied-responder/)**

**Funcionalidades:**
- Detecção automática de tentativas de acesso não autorizado
- Notificações em tempo real via SNS/Email
- Logging centralizado para análise de comportamento
- Alertas para ações de alto risco

#### 🔐 Force User MFA
Automação que garante que todos os novos usuários IAM tenham MFA configurado obrigatoriamente.

**Tecnologias:** IAM, Lambda, CloudWatch Events, Python  
**Status:** 🚧 Em Desenvolvimento  
**[Ver Projeto →](./fase-1-iam-security-automation/force-user-mfa/)**

**Funcionalidades:**
- Criação automática de dispositivo MFA virtual para novos usuários
- Política de negação de acesso até configuração do MFA
- Instruções automatizadas enviadas aos usuários
- Auditoria de compliance MFA

#### 🛡️ CloudTrail Remediation
Script de remediação automática que protege contra desativação do CloudTrail.

**Tecnologias:** CloudTrail, Lambda, EventBridge, CloudWatch  
**Status:** 🚧 Em Desenvolvimento  
**[Ver Projeto →](./fase-1-iam-security-automation/cloudtrail-remediation/)**

**Funcionalidades:**
- Detecção de desativação do CloudTrail
- Reativação automática com configurações originais
- Notificação de tentativas de desativação
- Placeholders para investigação forense

---

### Fase 2: Resposta Automatizada a Incidentes

#### 🚨 EC2 Auto Clean Room Forensics
Sistema completo de resposta automatizada a incidentes envolvendo instâncias EC2 comprometidas, com isolamento e análise forense.

**Tecnologias:** Step Functions, Lambda, EC2, SNS, VPC, CloudWatch  
**Status:** 📅 Planejado  
**[Ver Projeto →](./fase-2-incident-response/ec2-forensics-automation/)**

**Funcionalidades:**
- Orquestração de resposta com AWS Step Functions
- Isolamento automático de instâncias comprometidas
- Criação de snapshots para preservação de evidências
- Análise forense básica automatizada
- Notificações multi-canal para equipe de segurança
- Tags de segurança para rastreabilidade
- Logs detalhados do processo de resposta

**Workflow de Resposta:**
1. **Detecção** - Alerta recebido via SNS Topic
2. **Notificação** - Equipe de segurança alertada imediatamente
3. **Isolamento** - Instância isolada com Security Group restritivo
4. **Preservação** - Snapshots dos volumes EBS criados
5. **Análise** - Coleta automática de artefatos forenses
6. **Documentação** - Registro completo do incidente

---

## 🏭 Arquitetura Geral

```
┌─────────────────────────────────────────────────────────────┐
│                    AWS Security Automation                   │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐      ┌──────────────┐      ┌───────────┐ │
│  │  CloudTrail  │─────▶│  EventBridge │─────▶│  Lambda   │ │
│  │   Logging    │      │    Rules     │      │ Functions │ │
│  └──────────────┘      └──────────────┘      └─────┬─────┘ │
│                                                      │        │
│  ┌──────────────┐      ┌──────────────┐      ┌─────┴─────┐ │
│  │     IAM      │◀────▶│     SNS      │◀────▶│    Step   │ │
│  │   Policies   │      │   Topics     │      │ Functions │ │
│  └──────────────┘      └──────────────┘      └───────────┘ │
│                                                               │
│  ┌──────────────┐      ┌──────────────┐      ┌───────────┐ │
│  │     EC2      │─────▶│  CloudWatch  │─────▶│    S3     │ │
│  │  Instances   │      │     Logs     │      │  Buckets  │ │
│  └──────────────┘      └──────────────┘      └───────────┘ │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## 🛠️ Tecnologias e Serviços AWS

| Categoria | Serviços |
|-----------|----------|
| **Compute** | Lambda, EC2 |
| **Security** | IAM, CloudTrail, GuardDuty |
| **Orchestration** | Step Functions, EventBridge |
| **Monitoring** | CloudWatch (Logs, Events, Alarms) |
| **Notification** | SNS, SES |
| **Storage** | S3, EBS Snapshots |
| **Networking** | VPC, Security Groups |
| **IaC** | CloudFormation |

## 📚 Habilidades Demonstradas

- ✅ Automação de resposta a incidentes de segurança
- ✅ Desenvolvimento de funções Lambda em Python
- ✅ Orquestração de workflows com AWS Step Functions
- ✅ Implementação de políticas IAM baseadas em least privilege
- ✅ Configuração de monitoramento e alertas proativos
- ✅ Análise forense digital em ambientes cloud
- ✅ Infrastructure as Code com CloudFormation
- ✅ Integração de múltiplos serviços AWS
- ✅ Documentação técnica e diagramas de arquitetura
- ✅ Boas práticas de segurança em desenvolvimento

## 🚀 Como Usar Este Repositório

### Pré-requisitos
```bash
- Conta AWS (Free Tier compatível)
- AWS CLI configurado
- Python 3.9+
- Permissões IAM necessárias (documentadas em cada projeto)
```

### Clone o Repositório
```bash
git clone https://github.com/eusrawayne/aws-security-automation-portfolio.git
cd aws-security-automation-portfolio
```

### Deploy de Projetos
Cada projeto contém seu próprio README com instruções específicas de deployment. Navegue até o diretório do projeto desejado:

```bash
cd fase-1-iam-security-automation/iam-access-denied-responder/
# Siga as instruções no README.md do projeto
```

## 💰 Considerações de Custo

Todos os projetos foram desenvolvidos considerando otimização de custos:

- **Lambda**: Free tier cobre até 1 milhão de requisições/mês
- **CloudWatch**: Logs e métricas básicas incluídas no free tier
- **SNS**: 1.000 notificações gratuitas/mês
- **CloudTrail**: Um trail gratuito por conta
- **Step Functions**: 4.000 transições de estado gratuitas/mês

**Custo estimado mensal:** < $5 USD para ambiente de testes

## 📖 Documentação Adicional

- [Guia de Setup Inicial](./docs/setup-guide.md)
- [Guia de Testes](./docs/testing-guide.md)
- [Otimização de Custos](./docs/cost-optimization.md)
- [Troubleshooting Comum](./docs/troubleshooting.md)
- [Incident Response Playbook](./fase-2-incident-response/incident-playbook.md)

## 🎓 Aprendizados e Próximos Passos

### Principais Aprendizados
- Importância da automação em ambientes cloud de produção
- Como desenhar sistemas de segurança resilientes e escaláveis
- Integração efetiva entre diferentes serviços AWS
- Boas práticas de resposta a incidentes

### Roadmap Futuro
- [ ] Integração com AWS Security Hub
- [ ] Dashboard centralizado de segurança
- [ ] Automação com Terraform
- [ ] Testes automatizados (pytest)
- [ ] Integração com Slack/Microsoft Teams
- [ ] Machine Learning para detecção de anomalias
- [ ] Expansão para ambientes multi-conta com AWS Organizations

## 🔗 Conecte-se Comigo

- **LinkedIn:** [Seu Nome](seu-linkedin-url)
- **GitHub:** [@eusrawayne](https://github.com/eusrawayne)
- **Portfolio:** [Em Construção]

## 📝 Referências

Este projeto é baseado e inspirado em:
- [AWS Security Automation](https://github.com/awslabs/aws-security-automation) - AWS Labs
- [AWS Security Best Practices](https://docs.aws.amazon.com/security/)
- [AWS Well-Architected Framework - Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## 📝 Licença

Este projeto é baseado no [AWS Security Automation](https://github.com/awslabs/aws-security-automation) da AWS Labs, licenciado sob Apache License 2.0.

Modificações e documentação © 2026 - Disponível sob Apache License 2.0

---

⭐ **Se este repositório for útil, considere dar uma estrela!**

*Última atualização: Janeiro 2026*