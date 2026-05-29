> **Nota sobre Software Proprietário:** Este repositório serve como uma demonstração técnica e visão geral da arquitetura. O código-fonte completo faz parte de um projeto comercial proprietário. Para avaliações técnicas, demonstrações ao vivo ou propostas de parceria, por favor, entre em contato diretamente.

## Visão Geral
O LogbookApp (FlightHangar) é um SaaS mobile multiplataforma de alta performance projetado especificamente para a indústria da aviação. Ele resolve o problema crítico do registro manual de voos automatizando cálculos complexos de tempo de voo, gerando documentos oficiais auditáveis e fornecendo um ambiente robusto e *offline-first* para os pilotos gerenciarem seus dados aeronáuticos.

Atualmente, tanto o motor *offline* principal quanto a camada de sincronização Cloud/SaaS estão totalmente operacionais, entregando uma experiência de nível corporativo do hangar remoto até a nuvem, repleta de automação, telemetria e recursos estritos de *compliance*.

## Principais Funcionalidades Técnicas e Arquiteturais

### 1. Arquitetura Offline-First e Segurança de Nível Militar
A aviação exige softwares que funcionem perfeitamente em modo avião ou em hangares remotos. O LogbookApp implementa uma camada de dados local altamente segura:
* **SQLite Local com Criptografia AES:** O banco de dados no dispositivo é totalmente criptografado usando chaves de sessão dinâmicas, protegendo os dados sensíveis do piloto e impedindo a extração local não autorizada.
* **Derivação de Chaves PBKDF2 e Biometria:** O acesso local é protegido por um PIN com hash PBKDF2 de 100.000 iterações e autenticação biométrica nativa (Impressão Digital/FaceID).
* **Sincronização Bidirecional em Nuvem:** Um serviço resiliente em segundo plano (`SyncService`) comunica-se de forma segura com uma API ASP.NET Core via JWT, sincronizando diários de voo e dados da frota silenciosamente quando uma conexão estável é detectada.
* **Zero Bloqueio de Interface (UI):** Operações assíncronas de busca e CRUD usando padrões MVVM estritos garantem uma interface fluida independentemente da carga do banco de dados.

### 2. Engenharia Aeronáutica e Algoritmos Físicos
* **Motor Dinâmico de Peso e Balanceamento (W&B):** Calcula o Centro de Gravidade (CG) em tempo real com base em estações de passageiros/carga personalizáveis e no envelope específico da aeronave. Inclui avisos visuais de MTOW (Peso Máximo de Decolagem) para evitar sobrecarga estrutural.
* **Controle Inteligente de Manutenção (CTM):** Monitora automaticamente o total de horas da célula da aeronave em relação aos gatilhos de revisão de 50h/100h, sinalizando visualmente o status da aeronave (Disponível, Alerta de Manutenção ou Aterrada/AOG).
* **Fórmula de Haversine e Planejamento de Voo:** Usa métricas de performance da aeronave e distâncias esféricas exatas entre coordenadas ICAO para fornecer tempos de voo estimados em tempo real e cálculos de Proa Verdadeira.
* **Divisão Automática Diurno/Noturno:** Integra-se à API Sunrise-Sunset para calcular automaticamente a divisão exata das horas de voo diurnas e noturnas com base em coordenadas geográficas e no horário de decolagem.

### 3. Automação e Integrações de IA
* **Scanner OCR de Tacômetro:** Utiliza o `Plugin.Maui.OCR` para extrair horas de voo diretamente do painel físico da aeronave pela câmera do dispositivo, eliminando a entrada manual de dados.
* **Compartilhamento de Voo por QR Code:** Os pilotos podem compartilhar diários de voo complexos instantaneamente de forma *offline* usando *payloads* JSON gerados localmente e codificados em QR Codes.
* **Importação de Escalas .ics:** Um *parser* customizado que lê escalas mensais corporativas (arquivos iCalendar), extraindo automaticamente códigos ICAO, datas e horários para criar planos de voo em lote.
* **Tradutor de METAR:** Busca *strings* meteorológicas brutas de APIs externas e as converte em formatos decodificados e de fácil leitura (Vento, Temperatura, QNH) para avaliações rápidas na linha de voo.

### 4. Telemetria, UX Nativa e Monetização
* **HUD de Mapa Tático:** Uma `MapPage` customizada apresentando uma sobreposição translúcida estilo *Glass Cockpit*, renderizando rotas de voo singulares ou um mapa de calor global de toda a carreira do piloto utilizando blocos de aviação OpenAIP personalizados.
* **Dashboards com Microcharts:** Renderiza gráficos dinâmicos de rosca e barras de forma nativa usando SkiaSharp para visualizar métricas operacionais (VFR/IFR, Noturno/Diurno) e dados financeiros (Diárias, Custos de Combustível).
* **Serviço de Entitlement (Paywall):** Uma arquitetura de interceptação contínua que protege recursos *premium* por trás de um modelo de assinatura SaaS, maximizando a conversão por meio de uma estratégia de "Armadilha de Valor".

### 5. Auditoria de Nível Corporativo e Compliance (ANAC/FAA)
* **Geração de PDF com QuestPDF:** Manipulação nativa de *streams* utilizando o motor QuestPDF para exportar Relatórios de Diário de Bordo (CIV) padrão ANAC e Currículos de Voo profissionais, com precisão de pixels, diretamente do dispositivo.
* **Trilha de Auditoria "Caixa Preta":** O sistema utiliza exclusões lógicas (**soft-deletes**) e mantém um 'Log de Auditoria' dedicado. Todos os dados originais do voo permanecem preservados no dispositivo e na nuvem para *compliance* de auditoria, independentemente das modificações do piloto.
* **Captura de Dados Enriquecida:** Apresenta captura de assinatura digital para endossos de instrutores e fotografia de recibos no próprio dispositivo para despesas de combustível/taxas.
* **Sanitização de Entradas:** Forte validação de entrada e sanitização algorítmica são aplicadas em todos os formulários para prevenir ataques de injeção e garantir a integridade do banco de dados.

## Stack Tecnológico
**Frontend (Mobile SaaS):**
* **Framework:** .NET 8 / MAUI / C# / XAML
* **Arquitetura:** MVVM Estrito (Model-View-ViewModel)
* **Banco de Dados Local:** SQLite-net-sqlcipher Criptografado
* **Integrações de Hardware:** Biometria, Câmera (OCR/QR), Geolocalização, SecureStorage
* **UI/UX:** Microcharts (SkiaSharp), ZXing, QuestPDF
* **Plataformas Alvo:** Android, iOS (Multiplataforma)

**Backend (Cloud Sync & API):**
* **Framework:** ASP.NET Core Web API
* **ORM:** Entity Framework Core (EF Core)
* **Banco de Dados:** PostgreSQL
* **Segurança:** Autenticação JWT, Hashing de Senha BCrypt

## Showcase de Fluxo de Trabalho e Arquitetura

*(Este primeiro GIF demonstra a jornada completa do usuário e a arquitetura de auditoria. Começa com o login seguro na nuvem e a validação do PIN, segue pela configuração inicial do piloto e aeronave, e mostra a inteligência da criação de um novo voo — apresentando preenchimento automático, busca em tempo real de METAR, cálculos automáticos de Haversine e captura de assinatura. Conclui destacando os gestos de deslize de alta fidelidade e a funcionalidade de auditoria "Caixa Preta" não-destrutiva, onde as edições exigem uma justificativa compatível com a ANAC e as exclusões são arquivadas com segurança em segundo plano.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)

*(Este segundo GIF foca inteiramente no Dashboard interativo e no motor de relatórios. Destaca a agregação em tempo real de métricas de voo e estatísticas financeiras. Também demonstra como os pilotos podem selecionar intervalos de datas específicos para gerar e exportar instantaneamente relatórios PDF formatados diretamente do armazenamento local do dispositivo.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)

## Status Atual de Desenvolvimento
- [x] **Fase 1: MVP Offline Principal** - Banco de dados, CRUD, UI/UX em MVVM e cálculos de Haversine.
- [x] **Fase 2: Física de Aviação** - Peso e Balanceamento Dinâmico (CG), rastreamento de Manutenção (CTM) e divisão automática de Diurno/Noturno.
- [x] **Fase 3: Automação e Telemetria** - Scanner OCR de Tacômetro, compartilhamento por QR Code, importação de escalas corporativas `.ics` e Mapas Táticos em HUD.
- [x] **Fase 4: Camada SaaS e Sincronização em Nuvem** - Serviço de acesso Paywall, segurança com PBKDF2/Biometria e sincronização bidirecional em segundo plano.
- [x] **Fase 5: Relatórios Oficiais** - Geração no dispositivo de relatórios PDF e CVs padrão ANAC usando QuestPDF com assinaturas digitais.
- [ ] **Fase 6: Reformulação de UI/UX e Lançamento** - Redesign visual e implantação na App Store / Google Play.

## Contato e Oportunidades
Para avaliações técnicas, demonstrações ao vivo ou dúvidas sobre parcerias estratégicas e licenciamento, por favor, entre em contato diretamente. Estou aberto a explorar integrações de software em nível corporativo e oportunidades de desenvolvimento remoto.


jordanmaycon@gmail.com
