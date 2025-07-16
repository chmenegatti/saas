# MVP - AcervoTech

## Objetivo do MVP

Validar as principais dores do público-alvo e entregar valor imediato, focando nas funcionalidades essenciais para gestão de acervo técnico de engenheiros e arquitetos.

---

## Funcionalidades Essenciais

1. **Cadastro de Usuário e Autenticação**
   - Criação de conta (e-mail, senha, nome, tipo de profissional)
   - Login seguro
   - Recuperação de senha

2. **Upload Seguro de Documentos**
   - Upload de arquivos (PDF, DOC, DWG, JPG)
   - Armazenamento em nuvem (S3/AWS)
   - Organização por categorias/tags manuais

3. **Indexação Básica com OCR**
   - Extração automática de dados principais (número, data, contratante) via AWS Textract
   - Associação dos dados extraídos ao documento

4. **Busca Simples e Filtros**
   - Busca por texto (nome, número, contratante)
   - Filtros por tipo de documento, data, categoria

5. **Dashboard Inicial**
   - Visualização dos últimos documentos adicionados
   - Resumo de quantidade por categoria

6. **Alertas de Vencimento (Manual)**
   - Cadastro de data de validade para documentos
   - Notificação por e-mail próxima ao vencimento

7. **Download e Compartilhamento**
   - Download de documentos
   - Geração de link seguro para compartilhamento (com prazo de validade)

---

## Arquitetura Técnica

- **Frontend:** React (web responsivo)
- **Backend:** Node.js (TypeScript) ou Python (FastAPI)
- **Banco de Dados:** PostgreSQL
- **Armazenamento:** AWS S3
- **OCR:** AWS Textract
- **E-mail:** AWS SES
- **Autenticação:** JWT

---

## Fluxo do Usuário

1. Usuário acessa a plataforma e cria sua conta
2. Realiza login
3. Faz upload de documentos, que são indexados via OCR
4. Organiza documentos por tags/categorias
5. Busca e filtra documentos conforme necessidade
6. Recebe alertas de vencimento por e-mail
7. Compartilha documentos via link seguro

---

## Critérios de Sucesso

- Usuários conseguem cadastrar e acessar documentos em menos de 5 minutos
- Busca retorna resultados relevantes em menos de 2 segundos
- 80% dos documentos indexados corretamente via OCR
- Compartilhamento seguro funcionando
- Feedback positivo dos primeiros usuários (NPS > 8)

---

## Roadmap de Entregas do MVP

- Semana 1-2: Prototipação de telas e validação com usuários reais
- Semana 3-6: Desenvolvimento backend, frontend e integração OCR
- Semana 7: Testes internos e ajustes
- Semana 8: Lançamento beta restrito
- Semana 9+: Coleta de feedback e priorização de melhorias

---

## Limitações do MVP

- Sem integração automática com CREA/CAU
- Relatórios customizados e automação avançada ficam para versões futuras
- Permissões avançadas e gestão de equipes restritas ao plano inicial

---

## Próximos Passos Pós-MVP

- Implementar relatórios automáticos
- Expandir integrações regulatórias
- Adicionar gestão de equipes e permissões avançadas
- Evoluir dashboard e analytics

---

## Observações

## Próximos Passos Pós-MVP (Roadmap de Evolução)

1. **Relatórios Automáticos e Customizados**
   - Geração de dossiês e quadros customizados para licitações, exportação em PDF/Excel, personalização conforme edital.
2. **Integrações Regulatórias**
   - APIs para importação automática de ARTs/RRTs do CREA/CAU, atualização automática do acervo.
3. **Gestão de Equipes e Permissões Avançadas**
   - Controle granular de acesso, múltiplos usuários, permissões por projeto/documento, histórico de ações.
4. **Dashboard Avançado e Analytics**
   - Indicadores de performance, benchmarking com o setor, relatórios de uso, exportação de dados.
5. **Inteligência de Licitações**
   - Monitoramento automático de portais de licitação, cruzamento dos requisitos do edital com o acervo, sugestão de documentos e alertas de oportunidades.
6. **Mobile App Completo**
   - Digitalização via câmera, upload instantâneo, notificações inteligentes, assistente virtual.
7. **Automação de Compliance**
   - Checklist automático de conformidade, alertas de pendências, sugestões de regularização.
8. **Marketplace de Serviços**
   - Conexão com despachantes, consultores, advogados, ofertas de serviços complementares.
9. **Assinatura Digital e Blockchain**
   - Assinatura digital integrada, registro em blockchain para autenticidade e integridade dos documentos.
10. **Gamificação e Certificação**
    - Badges, certificações digitais, ranking opcional de profissionais/empresas mais preparados.
11. **API Pública e Ecossistema**
    - API aberta para integração com ERPs, CRMs, softwares de orçamento, plugins de terceiros.
12. **Suporte Proativo e Consultivo**
    - Suporte consultivo para análise de editais, compliance, central de conhecimento, templates e dicas.

## Observações 2

O MVP deve ser enxuto, funcional e focado em resolver a dor principal do público-alvo, servindo como base para validação e evolução do produto. O roadmap acima detalha todas as etapas para que o AcervoTech evolua até uma versão completa, robusta e estável, capaz de atender desde autônomos até grandes empresas, com diferenciais competitivos e alto valor agregado.
