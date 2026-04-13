# LogbookApp: SaaS de Aviação Offline-First (Vitrine)

*Leia em outros idiomas: [English](README.md), [Português](README.pt-BR.md).*

> **Nota sobre Software Proprietário:** Este repositório serve como uma vitrine técnica e visão geral da arquitetura. O código-fonte completo faz parte de um projeto comercial proprietário. Para avaliações técnicas, demonstrações ao vivo ou propostas de parceria, por favor, entre em contato diretamente.

## Visão Geral
O LogbookApp é um aplicativo móvel multiplataforma de alta performance, projetado especificamente para a indústria da aviação. Ele resolve o problema crítico do registro manual de voos, automatizando cálculos de tempo de voo e fornecendo um ambiente robusto e *offline-first* para os pilotos gerenciarem seus dados aeronáuticos.

Atualmente, o motor offline principal (MVP) está 100% concluído e funcional, enquanto a camada de sincronização SaaS em nuvem está em fase final de desenvolvimento.

## Principais Recursos Técnicos e Arquiteturais

### 1. Arquitetura Offline-First e Gerenciamento de Dados
A aviação exige softwares que funcionem perfeitamente em modo avião ou em hangares remotos. O LogbookApp implementa uma camada de dados local robusta:
* **Acesso Multicamadas Seguro:** Protegido por um login em nuvem e um fluxo dedicado de PIN numérico offline para resguardar dados sensíveis no dispositivo.
* **SQLite Local de Alta Engenharia:** Estrutura de banco de dados altamente otimizada para gerenciar conjuntos de mais de **4.000 registros oficiais de aeroportos da ANAC**.
* **Zero Bloqueio de UI:** Operações de busca e CRUD assíncronas implementadas usando o padrão MVVM, garantindo uma interface fluida independente da carga no banco de dados.

### 2. Engenharia Geoespacial e Relatórios Automatizados
* **Planejamento de Voo Inteligente:** O fluxo automatizado de "Novo Voo" utiliza dados de performance da aeronave para fornecer cálculos em tempo real de distância e tempo estimado de voo entre códigos ICAO.
* **Divisão Automática Diurno/Noturno:** O algoritmo utiliza a hora local e coordenadas geográficas para calcular automaticamente a divisão entre horas de voo diurnas e noturnas.
* **Implementação da Fórmula de Haversine:** Recurso central de engenharia utilizado para calcular distâncias esféricas exatas entre aeroportos nativamente no dispositivo.
* **Integração METAR/TAF:** O aplicativo consome e exibe relatórios meteorológicos de aeródromos de origem e destino na tela quando há conexão.

### 3. Gestos Nativos e UX
* **Implementação MVVM:** Segue estritamente o padrão Model-View-ViewModel para um código limpo e testável.
* **Interação de Alta Fidelidade:** Implementa gestos nativos de deslizar baseados em física (swipe-to-delete/edit) na lista principal, fornecendo feedback imediato e fluido ao usuário.
* **Captura de Dados:** Possui recursos ricos de entrada de dados, incluindo fotografia de recibos no próprio dispositivo e captura de assinatura nativa.
* **Exportação CSV/PDF:** Inclui manipulação nativa de fluxos de dados (streams) para exportar relatórios auditáveis em formato CSV ou PDF diretamente do dispositivo.

### 4. Segurança Corporativa e Auditoria Não-Destrutiva (Conformidade ANAC)
A segurança e a integridade dos dados são tratadas como pilares fundamentais da arquitetura. O conceito central é a **manipulação não-destrutiva de dados**.
* **A Trilha de Auditoria "Caixa Preta":** Mesmo quando a interface apresenta confirmações de exclusão ou edição, o banco de dados não remove os registros fisicamente. O sistema utiliza **soft-deletes** e mantém um "Log de Auditoria" dedicado (a 'Caixa Preta'). Todos os dados de voo originais permanecem preservados no dispositivo (e futuramente na nuvem) para conformidade com auditorias da ANAC, independentemente das modificações do piloto.
* **Justificativa de Edição Obrigatória:** Um fluxo de 'Edição' não pode ser finalizado até que o piloto insira um motivo justificativo, garantindo uma trilha clara e auditável de alterações para cada registro assinado e modificado.
* **Sanitização de Entradas:** Validação pesada e sanitização de dados são aplicadas em todos os formulários para prevenir ataques comuns de injeção no banco de dados local.

## Stack Tecnológica
* **Framework:** .NET MAUI / C# / XAML
* **Arquitetura:** MVVM (Model-View-ViewModel)
* **Banco de Dados Local:** SQLite
* **Geoespacial e ETL:** Algoritmos Haversine, pipeline Apache Hop para importação de dados da ANAC
* **Ciência de Dados:** Python para análise exploratória de dados
* **Plataformas Alvo:** Android, iOS (Multiplataforma)

## Demonstração de Fluxo e Arquitetura

*(Este primeiro GIF demonstra a jornada completa do usuário e a arquitetura de auditoria. Começa com o login seguro e validação de PIN, passa pela configuração inicial do piloto e aeronave, e mostra a inteligência da criação de um novo voo — apresentando preenchimento automático, busca de METAR em tempo real, cálculos automáticos via Haversine e captura de assinatura. Conclui destacando os gestos de deslizar de alta fidelidade e o recurso de auditoria "Caixa Preta" não-destrutivo, onde edições exigem uma justificativa obrigatória e exclusões são arquivadas de forma segura em segundo plano.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)

*(Este segundo GIF foca inteiramente no Dashboard interativo e no motor de relatórios. Ele destaca a agregação em tempo real de métricas de voo e estatísticas financeiras. Também demonstra como os pilotos podem selecionar intervalos de datas específicos para gerar e exportar instantaneamente relatórios em PDF diretamente do armazenamento local do dispositivo.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)

## Status Atual de Desenvolvimento
- [x] **Fase 1: MVP Offline Core** - Banco de dados, CRUD, UX/UI e cálculos geoespaciais (Concluído).
- [x] **Fase 2: Relatórios PDF Nativos** - Geração de relatórios PDF com seleção de período (Concluído).
- [x] **Fase 3: Camada SaaS e Sincronização de Auditoria** - Sincronização em nuvem bidirecional da trilha de dados da "Caixa Preta" local, autenticação de usuários e modelos                 de assinatura recorrente (Concluído).
- [ ] **Fase 4: Bug fix e features**.

## Contato e Oportunidades
Sou Engenheiro de Software especializado em .NET MAUI, C# e arquitetura de sistemas auditáveis. Atualmente, estou aberto a oportunidades remotas internacionais ou parcerias estratégicas para este produto.

* **LinkedIn:** https://www.linkedin.com/in/jordan-fabian/
* **Email:** jordanmaycon@gmail.com
