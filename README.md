## **Treino & Dieta App**

# **Treino & Dieta App**
Este projeto é um **Single Page Application (SPA)** desenvolvido com **Angular 2 no front-end** e **Python (FastAPI) no back-end**, totalmente hospedado na AWS. O sistema permite registrar e monitorar **treinos e dieta** com segurança, integrando **AWS Lambda, DynamoDB, AWS Cognito e AWS KMS**.

## **1. Tecnologias Utilizadas**
### **Frontend (Angular 2)**
- **Framework:** Angular 2 (SPA)
- **Linguagem:** TypeScript
- **Gerenciador de Pacotes:** npm
- **Estilização:** TailwindCSS / Bootstrap

### **Backend (Python - FastAPI)**
- **Framework:** FastAPI
- **Banco de Dados:** AWS DynamoDB
- **Autenticação:** AWS Cognito (OAuth2/JWT)
- **Segurança:** AWS KMS (armazenamento de credenciais)

### **Infraestrutura AWS**
- **API Serverless:** AWS Lambda (Python)
- **Banco de Dados:** DynamoDB
- **Armazenamento de Arquivos:** Amazon S3
- **Autenticação:** AWS Cognito
- **Infra como Código:** AWS CDK (Python)
- **Monitoramento:** AWS CloudWatch + X-Ray
- **Segurança:** AWS WAF + AWS IAM + AWS KMS

### **CI/CD e Qualidade**
- **Controle de Código:** GitHub
- **CI/CD:** GitHub Actions + AWS CodePipeline
- **Testes:** Jest (Front-end), PyTest (Back-end)
- **Análise de Código:** SonarQube

---

## **2. Como Configurar o Projeto**

### **2.1. Pré-requisitos**
Antes de iniciar, certifique-se de ter instalado:
- **Node.js** `>= 16`
- **Angular CLI** `>= 12`
- **Python** `>= 3.8`
- **AWS CLI** configurado com suas credenciais
- **AWS CDK** `npm install -g aws-cdk`
- **Terraform (opcional)** `brew install terraform`
- **Docker** (para testes locais do Lambda)

---

## **3. Configuração do Frontend**
### **3.1. Clonando o projeto**
```sh
git clone https://github.com/seu-usuario/treino-dieta-app.git
cd treino-dieta-app
```

### **3.2. Instalando dependências**
```sh
cd frontend
npm install
```

### **3.3. Executando o projeto**
```sh
ng serve --open
```
O frontend estará disponível em `http://localhost:4200/`.

### **3.4. Configurar ambiente**
Crie o arquivo **`frontend/src/environments/environment.ts`**:
```ts
export const environment = {
  production: false,
  apiBaseUrl: 'https://api.treino-dieta.com',
  cognitoClientId: 'SEU_COGNITO_CLIENT_ID',
  cognitoUserPoolId: 'SEU_COGNITO_POOL_ID'
};
```

---

## **4. Configuração do Backend**
### **4.1. Criando ambiente virtual e instalando dependências**
```sh
cd backend
python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

### **4.2. Estrutura do backend (FastAPI)**
```
backend/
│── app/
│   ├── main.py  # Entry point
│   ├── routes/
│   │   ├── treino.py
│   │   ├── dieta.py
│   ├── models/
│   ├── services/
│   ├── dynamodb.py
│   ├── cognito.py
│── tests/
│   ├── test_treino.py
│   ├── test_dieta.py
```

---

## **5. Deploy na AWS com CDK**
### **5.1. Instalando dependências**
```sh
cd cdk
pip install -r requirements.txt
```

### **5.2. Criando a infraestrutura**
```sh
cdk bootstrap
cdk synth
cdk deploy
```

### **5.3. Recursos Criados**
- AWS Lambda (`TreinoDietaLambda`)
- DynamoDB (`TreinoDietaDB`)
- API Gateway (`TreinoDietaAPI`)
- Cognito (`TreinoDietaAuth`)

---

## **6. CI/CD com GitHub Actions**
Crie `.github/workflows/ci-cd.yml`:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
      - develop

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout código
        uses: actions/checkout@v2

      - name: Instalar dependências do front-end
        run: |
          cd frontend
          npm install
          npm run test

      - name: Instalar dependências do backend
        run: |
          cd backend
          python -m venv venv
          source venv/bin/activate
          pip install -r requirements.txt
          pytest tests/

  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy AWS CDK
        run: |
          cd cdk
          pip install -r requirements.txt
          cdk deploy --require-approval never
```

---

## **7. Monitoramento e Segurança**
### **7.1. Configurando AWS CloudWatch**
```sh
aws logs create-log-group --log-group-name /aws/lambda/TreinoDietaLambda
```

### **7.2. Proteção contra ataques**
- **AWS WAF** configurado para API Gateway
- **AWS KMS** para armazenar tokens e credenciais
- **AWS IAM** com permissões mínimas necessárias

---

## **8. Próximos Passos**
✅ Melhorar UI do front-end  
✅ Criar gráficos de evolução  
✅ Implementar notificações AWS SNS  

---

Isso cobre **infraestrutura completa**, **segurança**, **deploy automatizado** e **monitoramento**! 🚀

