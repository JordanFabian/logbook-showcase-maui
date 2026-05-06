# LogbookApp: SaaS de Aviação Offline-First (Showcase)

*Leia em outros idiomas: [English](README.md), [Português](README.pt-BR.md).*

> **Nota sobre Software Proprietário:** Este repositório serve como um portfólio técnico e uma visão geral da arquitetura. O código-fonte completo faz parte de um projeto comercial proprietário. Para avaliações técnicas, demonstrações ao vivo ou propostas de parceria, por favor, entre em contato diretamente.

## Visão Geral
O LogbookApp é um SaaS móvel multiplataforma de alto desempenho, projetado especificamente para a indústria da aviação. Ele resolve o problema crítico do registro manual de voos automatizando cálculos complexos de tempo de voo, gerando documentos oficiais auditáveis e fornecendo um ambiente robusto e *offline-first* para que os pilotos gerenciem seus dados aeronáuticos.

Atualmente, tanto o motor offline central quanto a camada de sincronização em nuvem/SaaS estão totalmente operacionais, entregando uma experiência de nível corporativo, desde o hangar sem internet até a nuvem.

## Principais Funcionalidades Técnicas e Arquiteturais

### 1. Arquitetura Offline-First & Gerenciamento de Dados Criptografados
A aviação exige softwares que funcionem perfeitamente em modo avião. O LogbookApp implementa uma camada de dados local altamente segura e com capacidade offline:
* **SQLite Local Criptografado (AES):** O banco de dados no dispositivo é totalmente criptografado usando chaves de sessão dinâmicas, protegendo os dados sensíveis do piloto e impedindo a extração local não autorizada.
* **Sincronização em Nuvem Bidirecional:** Um serviço em segundo plano resiliente (`SyncService`) se comunica com segurança com uma API ASP.NET Core via JWT, sincronizando os registros de voo e dados da frota apenas quando uma conexão de internet estável é detectada.
* **Manipulação de Grandes Volumes de Dados:** Estrutura de banco de dados altamente otimizada para consultar instantaneamente mais de **4.000 registros oficiais de aeródromos da ANAC** sem travar a interface do usuário (UI thread).

### 2. Engenharia Aeronáutica & Algoritmos Físicos
* **Motor Dinâmico de Peso e Balanceamento (W&B):** Calcula o Centro de Gravidade (CG) em tempo real com base em estações personalizáveis (passageiros/carga) e no envelope específico da aeronave. Inclui alertas visuais de MTOW (Peso Máximo de Decolagem) para evitar decolagens com sobrepeso.
* **Controle Técnico de Manutenção (CTM) Inteligente:** Monitora automaticamente as horas totais da célula em relação aos gatilhos de revisão, sinalizando visualmente o status da aeronave (Disponível, Alerta de Manutenção ou AOG/Grounded) no hangar digital.
* **Fórmula de Haversine & Planejamento de Voo:** Utiliza métricas de performance da aeronave (velocidade de cruzeiro) e distâncias esféricas exatas entre coordenadas ICAO para fornecer tempos de voo estimados em tempo real.
* **Tradutor de METAR:** Busca dados meteorológicos brutos de APIs externas e os converte em formatos decodificados e de fácil leitura para uma rápida avaliação na linha de voo.

### 3. Gestos Nativos, UX & Relatórios Oficiais
* **Geração com QuestPDF:** Inclui manipulação nativa de streams utilizando o motor QuestPDF para exportar relatórios de Logbook (Caderneta Individual de Voo - CIV) com precisão de pixels no padrão exigido pela ANAC, diretamente do dispositivo.
* **Interação de Alta Fidelidade:** Implementa gestos de deslizar (swipe) para excluir/editar baseados em física com sensação nativa na lista principal, fornecendo feedback imediato e fluido ao usuário.
* **Captura de Dados Rica:** Possui captura de assinatura digital para endossos de instrutores e fotografia de recibos no próprio dispositivo.

### 4. Segurança de Nível Corporativo & Auditoria Não Destrutiva (Compliance ANAC)
A segurança e a integridade dos dados são tratadas como pilares arquiteturais. Um princípio central é a **manipulação não destrutiva de dados**.
* **Trilha de Auditoria "Caixa Preta":** O sistema utiliza **soft-deletes** e mantém um 'Log de Auditoria' dedicado. Todos os dados originais de voo permanecem preservados no dispositivo e na nuvem para compliance de auditoria da ANAC, independentemente das modificações feitas pelo piloto.
* **Justificativa de Edição Obrigatória:** Um fluxo de 'Edição' não pode ser finalizado até que o piloto insira uma justificativa, garantindo uma trilha de alterações clara e auditável para cada registro assinado que for modificado.
* **Sanitização de Entradas:** Validação rigorosa de entradas e sanitização algorítmica são aplicadas em todos os formulários para prevenir ataques de injeção e garantir a integridade do banco de dados.

## Stack Tecnológico
**Frontend (Mobile App):**
* **Framework:** .NET MAUI / C# / XAML
* **Arquitetura:** MVVM rigoroso (Model-View-ViewModel)
* **Banco de Dados Local:** SQLite Criptografado (AES)
* **Relatórios:** QuestPDF

**Backend (Cloud SaaS):**
* **Framework:** ASP.NET Core Web API
* **ORM:** Entity Framework Core (EF Core)
* **Banco de Dados:** PostgreSQL
* **Segurança:** Autenticação JWT, Hashing de Senhas com BCrypt

**Engenharia de Dados:**
* **Geoespacial:** Algoritmos de Haversine
* **Pipeline:** Apache Hop & Python para importação de dados aeronáuticos oficiais da ANAC

## Demonstração de Fluxo de Trabalho & Arquitetura

*(Este primeiro GIF demonstra a jornada completa do usuário e a arquitetura de auditoria. Ele começa com o login seguro na nuvem e validação de PIN, passa pela configuração inicial do piloto e da aeronave, e mostra a inteligência da criação de um novo voo — apresentando preenchimento automático, busca de METAR em tempo real, cálculos automáticos de Haversine e captura de assinatura. Ele conclui destacando os gestos de deslizar de alta fidelidade e o recurso de auditoria não destrutiva "Caixa Preta", onde as edições exigem uma justificativa em conformidade com a ANAC e as exclusões são arquivadas com segurança em segundo plano.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)


*(Este segundo GIF foca inteiramente no Dashboard interativo e no motor de relatórios. Ele destaca a agregação em tempo real de métricas de voo e estatísticas financeiras. Também demonstra como os pilotos podem selecionar intervalos de datas específicos para gerar instantaneamente e exportar relatórios em PDF formatados diretamente do armazenamento local do dispositivo.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)


## Status Atual de Desenvolvimento
- [x] **Fase 1: MVP Offline Core** - Banco de dados, CRUD, UI/UX e cálculos geoespaciais.
- [x] **Fase 2: Relatórios PDF no Dispositivo** - Geração de relatórios PDF oficiais usando QuestPDF.
- [x] **Fase 3: Camada SaaS & Sincronização em Nuvem** - Sincronização bidirecional, autenticação JWT e implementação do backend em PostgreSQL.
- [x] **Fase 4: Integração de Física da Aviação** - Calculadora dinâmica de Peso e Balanceamento (W&B) e rastreamento de Manutenção (CTM).
- [ ] **Fase 5: Expansão de Features & Monetização** - Níveis de assinatura corporativa e algoritmos automáticos de divisão diurno/noturno.

## Contato & Oportunidades
Sou um Engenheiro de Software especializado em .NET MAUI, C# e arquiteturas de sistemas auditáveis. Atualmente estou aberto a oportunidades remotas internacionais ou parcerias estratégicas para este produto.

* **LinkedIn:** https://www.linkedin.com/in/jordan-fabian/
* **Email:** jordanmaycon@gmail.com
