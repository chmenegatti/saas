# Definições de Arquitetura - MVP AcervoTech

---

## Visão Geral

A arquitetura do MVP do AcervoTech foi desenhada para garantir escalabilidade, segurança, alta disponibilidade e facilidade de evolução. O objetivo é suportar o crescimento do produto, desde os primeiros usuários até grandes empresas, mantendo custos controlados e flexibilidade para incorporar novas funcionalidades.

---

## Componentes Principais

### 1. Frontend

- **Framework:** React (TypeScript)
- **Web responsivo** para desktop e mobile
- **Hospedagem:** AWS S3 + CloudFront (static hosting)
- **Autenticação:** JWT, integração com backend
- **Comunicação:** REST API (futuro: GraphQL)

### 2. Backend

- **Linguagem:** Node.js (TypeScript) ou Python (FastAPI)
- **Hospedagem:** AWS ECS (Docker containers) ou EC2 (auto scaling)
- **API:** RESTful, versionada
- **Autenticação:** JWT
- **Serviços:**
  - Upload/Download de documentos
  - Indexação via OCR (AWS Textract)
  - Notificações por e-mail (AWS SES)
  - Compartilhamento seguro (links temporários)
  - Alertas de vencimento

### 3. Banco de Dados

- **Tipo:** PostgreSQL (RDS/AWS)
- **Estrutura:**
  - Usuários
  - Documentos
  - Categorias/Tags
  - Logs de acesso/ações
  - Alertas
- **Escalabilidade:** Read replicas, backups automáticos

### 4. Armazenamento de Arquivos

- **Serviço:** AWS S3
- **Segurança:**
  - Objetos privados
  - Links temporários para download/compartilhamento
  - Versionamento de arquivos

### 5. OCR e Indexação

- **Serviço:** AWS Textract
- **Fluxo:**
  - Documento enviado pelo usuário
  - Backend aciona Textract
  - Dados extraídos são associados ao documento

### 6. Notificações e Alertas

- **Serviço:** AWS SES (e-mail)
- **Fluxo:**
  - Backend agenda/verifica vencimentos
  - Dispara e-mails automáticos

### 7. Segurança

- **Autenticação:** JWT
- **Criptografia:** HTTPS (TLS), dados sensíveis criptografados
- **Controle de acesso:** Permissões básicas por usuário
- **Auditoria:** Logs de ações e acessos

### 8. DevOps e Infraestrutura

- **Infraestrutura como código:** Terraform ou AWS CloudFormation
- **CI/CD:** GitHub Actions ou AWS CodePipeline
- **Monitoramento:** AWS CloudWatch, Sentry (erros frontend/backend)
- **Backup:** Automático (RDS, S3)

---

## Escalabilidade e Evolução

- **Containers:** Uso de Docker para facilitar deploy e escalar serviços
- **Auto Scaling:** Backend e banco de dados preparados para escalar horizontalmente
- **Separação de serviços:** Possibilidade de evoluir para arquitetura de microserviços conforme crescimento
- **Cache:** Redis/Memcached para acelerar buscas e sessões (futuro)
- **API Gateway:** AWS API Gateway para centralizar e proteger APIs (futuro)

---

## Diagrama Resumido

```bash
Usuário → Frontend (React/S3/CloudFront) → Backend (Node.js/Python/ECS) → PostgreSQL (RDS)
                                                        ↓
                                                    AWS S3 (Arquivos)
                                                        ↓
                                                    AWS Textract (OCR)
                                                        ↓
                                                    AWS SES (E-mail)
```

---

## 1. Escolha de Frameworks e Tecnologias

- **Frontend:** React + TypeScript, Styled Components, React Query, Axios
- **Backend:** Node.js + TypeScript (Express.js) ou Python (FastAPI)
- **ORM:** Prisma (Node.js) ou SQLAlchemy (Python)
- **Banco de Dados:** PostgreSQL (AWS RDS)
- **Armazenamento:** AWS S3
- **OCR:** AWS Textract
- **E-mail:** AWS SES
- **Autenticação:** JWT
- **Infraestrutura:** Docker, AWS ECS, Terraform
- **Monitoramento:** Sentry, AWS CloudWatch

## 2. Estrutura de Banco de Dados (PostgreSQL)

### Principais Tabelas

- **users**
  - id (uuid)
  - name
  - email
  - password_hash
  - type (engenheiro, arquiteto, empresa)
  - created_at

- **documents**
  - id (uuid)
  - user_id (uuid)
  - file_url (S3)
  - name
  - number
  - contractor
  - type (PDF, DOC, DWG, JPG)
  - category
  - tags (array)
  - valid_until (date)
  - extracted_data (json)
  - created_at

- **alerts**
  - id (uuid)
  - document_id (uuid)
  - user_id (uuid)
  - alert_type (vencimento, compartilhamento)
  - sent_at

- **share_links**
  - id (uuid)
  - document_id (uuid)
  - url
  - expires_at

- **logs**
  - id (uuid)
  - user_id (uuid)
  - action
  - document_id (uuid)
  - timestamp

## 3. Integração com Serviços AWS

- **S3:** Upload/download de arquivos, links temporários, versionamento, controle de acesso por usuário.
- **Textract:** Extração de dados de documentos via API, integração assíncrona (callback ou polling), armazenamento dos dados extraídos no banco.
- **SES:** Envio de e-mails automáticos (alertas de vencimento, notificações de compartilhamento, recuperação de senha).
- **RDS:** Banco de dados gerenciado PostgreSQL, backups automáticos, alta disponibilidade.
- **ECS:** Deploy de containers do backend, escalabilidade horizontal.
- **CloudFront:** CDN para frontend, distribuição global, SSL.

### Fluxo de Integração

1. Upload de documento → S3
2. Backend aciona Textract → recebe dados extraídos
3. Backend armazena dados no banco e agenda alertas
4. Backend utiliza SES para notificações
5. Frontend acessa arquivos via links temporários S3

---

## 4. Endpoints Principais (REST API)

### Autenticação

- `POST /auth/register` — Cadastro de usuário
- `POST /auth/login` — Login
- `POST /auth/forgot-password` — Recuperação de senha

### Documentos

- `POST /documents` — Upload de documento
- `GET /documents` — Listar documentos do usuário
- `GET /documents/:id` — Detalhes de documento
- `PUT /documents/:id` — Atualizar metadados
- `DELETE /documents/:id` — Remover documento

### Busca e Filtros

- `GET /search?query=...&filters=...` — Busca avançada

### Compartilhamento

- `POST /documents/:id/share` — Gerar link seguro
- `GET /share/:token` — Download via link compartilhado

### Alertas

- `GET /alerts` — Listar alertas do usuário

### Dashboard

- `GET /dashboard` — Resumo do acervo, últimos documentos, estatísticas

---

## 5. Modelos de Dados (Exemplo TypeScript)

```typescript
// User
interface User {
  id: string;
  name: string;
  email: string;
  passwordHash: string;
  type: 'engenheiro' | 'arquiteto' | 'empresa';
  createdAt: Date;
}

// Document
interface Document {
  id: string;
  userId: string;
  fileUrl: string;
  name: string;
  number: string;
  contractor: string;
  type: string;
  category: string;
  tags: string[];
  validUntil?: Date;
  extractedData?: Record<string, any>;
  createdAt: Date;
}

// Alert
interface Alert {
  id: string;
  documentId: string;
  userId: string;
  alertType: string;
  sentAt: Date;
}
```

## 6. Fluxos Principais

### Upload e Indexação

1. Usuário faz upload via frontend
2. Backend salva arquivo no S3
3. Backend aciona Textract para OCR
4. Dados extraídos são salvos em `extracted_data` do documento
5. Documento fica disponível para busca e visualização

### Compartilhamento Seguro

1. Usuário solicita compartilhamento
2. Backend gera link temporário (token, expiração)
3. Link enviado por e-mail ou copiado
4. Destinatário acessa documento via link seguro

### Alertas de Vencimento

1. Usuário cadastra data de validade
2. Backend agenda/verifica vencimentos
3. Backend dispara e-mail via SES próximo ao vencimento

### Busca e Dashboard

1. Usuário acessa dashboard
2. Backend retorna resumo do acervo, últimos documentos, estatísticas
3. Busca por texto/tags retorna documentos filtrados

---

## Observações

- Arquitetura pensada para MVP, mas pronta para escalar e incorporar novas funcionalidades (marketplace, mobile, integrações, etc).
- Todos os componentes podem ser substituídos ou expandidos conforme demanda e crescimento do produto.
- Foco em segurança, disponibilidade e facilidade de manutenção.

---

Essas definições servem como base para implementação e evolução do AcervoTech.
