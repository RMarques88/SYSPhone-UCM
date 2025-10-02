# SYSPHONE - Sistema de Análise de Ligações

Sistema completo para **visualização, análise e relatórios de dados de ligações telefônicas**. Integra-se com **UCM Grandstream** para coleta automática de dados em tempo real via API, oferecendo interface web para análise detalhada, geração de relatórios PDF e auditoria completa das atividades. O sistema está em conformidade com a **Lei Geral de Proteção de Dados (LGPD)**.

## Arquitetura do Sistema

### Fluxo de Dados
1. **UCM Grandstream** → Envia dados via Real Time Output (RTO) para a API
2. **API PHP** → Recebe e processa os dados no banco MySQL
3. **Interface Web** → Permite consulta, filtros avançados e análises
4. **Relatórios** → Geração de PDFs com dados filtrados
5. **Logs de Auditoria** → Registro detalhado de todas as atividades

### Estrutura de Arquivos
```
├── index.php                 # API para recebimento de dados da UCM
├── config.php                # Configurações do banco de dados
├── db.php                    # Classe de conexão com MySQL
├── log.php                   # Sistema de logs de API
├── view/                     # Interface web do sistema
│   ├── public/              # Páginas públicas
│   │   ├── login.php        # Autenticação de usuários
│   │   ├── index.php        # Dashboard principal
│   │   ├── ramais.php       # Gestão de ramais
│   │   ├── reports.php      # Relatórios básicos
│   │   ├── advanced_reports.php # Relatórios avançados (Admin)
│   │   ├── logs.php         # Logs do sistema (Admin)
│   │   ├── users.php        # Gestão de usuários (Admin)
│   │   └── lgpd.php         # Termos LGPD
│   ├── data/                # Endpoints de dados
│   │   ├── dados.php        # API principal de consultas
│   │   ├── get_reports_data.php    # Dados para relatórios básicos
│   │   └── get_advanced_reports.php # Dados para relatórios avançados
│   └── includes/            # Funções e configurações
│       ├── config.php       # Configurações da interface
│       ├── functions.php    # Funções auxiliares
│       ├── log.php          # Sistema de logs da interface
│       ├── pdf.php          # Configurações TCPDF
│       ├── export-pdf.php   # Exportação PDF básica
│       └── export-pdf-advanced.php # Exportação PDF avançada
└── db/                      # Scripts de banco de dados
    └── sysphonedb.sql      # Estrutura inicial do banco
```

## Funcionalidades Principais

### 🔌 API de Integração
- **Recebimento automático** de dados da UCM Grandstream via RTO
- **Processamento em tempo real** das ligações
- **Validação e normalização** dos dados recebidos
- **Logs detalhados** de todas as operações da API

### 📊 Interface de Visualização
- **Dashboard interativo** com métricas principais
- **Filtros avançados** por data, ramal, número, duração
- **Visualização em tempo real** das ligações ativas
- **Interface responsiva** com Bulma CSS

### 📈 Sistema de Relatórios

#### Relatórios Básicos (Todos os usuários)
- **Ligações por período** com filtros de data
- **Análise por ramal** específico
- **Ligações efetuadas vs recebidas**
- **Estatísticas de atendimento**

#### Relatórios Avançados (Apenas Administradores)
- **Análise detalhada por ramal** com métricas completas
- **Performance de ramais** com taxas de atendimento
- **Análise por horário** para identificar picos
- **Análise de ligações perdidas** com agrupamento opcional
- **Tabela paginada** de todas as ligações do ramal (20 por página)

### 📋 Filtros Inteligentes
O sistema aplica filtros automáticos para garantir dados relevantes:
- **Exclusão de filas**: Números 6500-6600 (filas de atendimento)
- **Exclusão de serviços**: Faixa 7000-7100 (números de serviço)
- **Exclusão de ramais fixos externos**: 9333-9339, 08000009333, 33519333
- **Apenas ramais válidos**: Filtros garantem exibição apenas de ramais internos

### 📄 Exportação PDF
- **Relatórios básicos em PDF** com filtros aplicados
- **Relatórios avançados em PDF** com análises detalhadas
- **Logs de exportação** para auditoria
- **Layout profissional** com dados organizados

### 🔐 Sistema de Segurança
- **Autenticação por token** com tempo de expiração
- **Níveis de permissão**: Usuário comum (perm=0) e Administrador (perm=1)
- **Acesso restrito**: Relatórios avançados apenas para administradores
- **Sessões seguras** com validação contínua

### 📝 Auditoria e Logs
- **Logs de API**: Todas as operações de recebimento de dados
- **Logs de Sistema**: Acessos, consultas e ações dos usuários
- **Logs de Segurança**: Tentativas de login e validações
- **Logs de Ramais**: Operações específicas de ramais
- **Logs detalhados**: Incluem filtros utilizados em cada consulta

### ⚖️ Conformidade LGPD
- **Consentimento explícito** para tratamento de dados
- **Política de privacidade** detalhada
- **Controle de acesso** por usuário
- **Logs de auditoria** para compliance

## Tecnologias Utilizadas

- **Backend**: PHP 7.4+
- **Frontend**: JavaScript ES6+, jQuery 3.6, Bulma CSS
- **Banco de Dados**: MySQL 8.0+
- **PDF**: TCPDF Library
- **API**: RESTful com JSON
- **Segurança**: Sessions + Token Based Authentication

## Endpoints da API

### API Principal (UCM Integration)
- `POST /index.php` - Recebimento de dados da UCM Grandstream

### Endpoints de Dados
- `GET /view/data/dados.php` - Consulta geral de ligações
- `GET /view/data/get_reports_data.php` - Dados para relatórios básicos
- `GET /view/data/get_advanced_reports.php` - Dados para relatórios avançados

### Endpoints de Exportação
- `POST /view/includes/export-pdf.php` - Exportação PDF básica
- `POST /view/includes/export-pdf-advanced.php` - Exportação PDF avançada

## Parâmetros de Filtros

### Relatórios Básicos
```javascript
{
    "startDate": "YYYY-MM-DD",    // Data inicial
    "endDate": "YYYY-MM-DD",      // Data final
    "extension": "xxxx",          // Ramal específico (opcional)
    "reportType": "basic"         // Tipo do relatório
}
```

### Relatórios Avançados
```javascript
{
    "startDate": "YYYY-MM-DD",    // Data inicial
    "endDate": "YYYY-MM-DD",      // Data final
    "extension": "xxxx",          // Ramal específico (opcional)
    "hourStart": "00-23",         // Hora inicial
    "hourEnd": "00-23",           // Hora final
    "reportType": "extension|hourly_analysis|performance|missed_analysis",
    "groupMissedCalls": "0|1",    // Agrupar ligações perdidas
    "page": 1,                    // Página (para paginação)
    "limit": 20                   // Limite por página
}
```

## Configuração e Instalação

### 1. Banco de Dados
```sql
-- Importar a estrutura inicial
mysql -u usuario -p sysphone < db/sysphonedb.sql
```

### 2. Configuração PHP
```php
// config.php
$host = 'localhost';
$dbname = 'sysphone';
$username = 'seu_usuario';
$password = 'sua_senha';
```

### 3. Configuração UCM
Configurar Real Time Output (RTO) na UCM para enviar dados para:
- URL: `http://seu_servidor/index.php`
- Método: POST
- Formato: JSON

### 4. Permissões
- Usuário comum (perm=0): Acesso aos relatórios básicos
- Administrador (perm=1): Acesso completo ao sistema

## Exemplos de Uso

### Consulta de Ligações por Ramal
```javascript
// Filtrar ligações do ramal 1001 nos últimos 7 dias
const filters = {
    startDate: '2025-06-16',
    endDate: '2025-06-23',
    extension: '1001',
    reportType: 'extension'
};
```

### Análise de Performance
```javascript
// Relatório de performance de todos os ramais
const filters = {
    startDate: '2025-06-01',
    endDate: '2025-06-23',
    reportType: 'performance'
};
```

### Ligações Perdidas Agrupadas
```javascript
// Agrupar ligações perdidas por número e período de 15 minutos
const filters = {
    startDate: '2025-06-23',
    endDate: '2025-06-23',
    reportType: 'missed_analysis',
    groupMissedCalls: '1'
};
```

## Logs e Auditoria

### Tipos de Log
1. **Logs de API** (`application.log`): Recebimento de dados da UCM
2. **Logs de Sistema** (`logs.php`): Acessos e operações gerais
3. **Logs de Segurança** (`log_secure.php`): Autenticação e validações
4. **Logs de Ramais** (`log_ramais.php`): Operações específicas de ramais

### Formato dos Logs
```
[2025-06-23 10:30:15] [usuario123] Relatório avançado de ramal. Filtros: [Ramal: 1001, Período: 2025-06-16 a 2025-06-23, Hora: 08h-18h, Agrupar perdidas: 0] [advanced_reports.php]
```

## Requisitos do Sistema

- **PHP**: 7.4 ou superior
- **MySQL**: 8.0 ou superior
- **Extensões PHP**: PDO, JSON, TCPDF
- **Servidor Web**: Apache ou Nginx
- **Navegador**: Compatível com ES6+

## Suporte e Manutenção

- **Logs detalhados** para troubleshooting
- **Validação de sintaxe** em todos os arquivos PHP
- **Tratamento de erros** com mensagens específicas
- **Backup automático** recomendado para dados críticos

---

**Desenvolvido por**: Raul M. M. Nascimento  
**Licença**: MIT  
**Versão**: 1.0.0

