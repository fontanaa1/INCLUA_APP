# 🏢 Especificação de Informações de Acessibilidade das Academias

* **Projeto:** INCLUA (Plataforma de Segurança, Acessibilidade e Inclusão em Academias)

* **Módulos Relacionados:** Módulo 4 (Mapa de Acessibilidade), Módulo 5 (Solicitações de Acessibilidade) e Módulo 8 (Avaliação das Academias)

* **Responsável:** Lorena Fontana Souza

## 🎯 1. Objetivo

Definir a estrutura de dados, os critérios técnicos e os selos de verificação necessários para cadastrar e exibir as condições de acessibilidade das academias no **INCLUA**. O objetivo é garantir transparência total para que pessoas com deficiência ou mobilidade reduzida encontrem estabelecimentos realmente preparados para recebê-las.

## 📍 2. Dados Gerais de Identificação da Academia

Informações básicas exibidas no card e na página de perfil da academia dentro do mapa interativo:

| Campo | Tipo de Dado | Obrigatório? | Descrição | 
| ----- | ----- | ----- | ----- | 
| `id` | UUID | Sim (Auto) | Identificador único da academia no sistema. | 
| `nome_fantasia` | Texto | Sim | Nome comercial da academia. | 
| `cnpj` | Texto | Sim | Registro da empresa para validação administrativa. | 
| `endereco_completo` | Texto | Sim | Rua, número, complemento, bairro, cidade, estado e CEP. | 
| `latitude` / `longitude` | Decimal | Sim | Coordenadas geográficas para plotagem no mapa e cálculo de distância. | 
| `telefone_whatsapp` | Texto | Sim | Contato direto para dúvidas sobre estrutura ou agendamento de visita. | 
| `horario_funcionamento` | JSON / Texto | Sim | Dias e horários de abertura e fechamento. | 
| `fotos_estrutura` | Array (URLs) | Sim (Mín. 3) | Fotos reais das entradas, banheiros, corredores e aparelhos adaptados. | 
| `status_verificacao` | Enum | Sim | Indica confiabilidade: `autodeclarada`, `em_analise`, `verificada`. | 

## ♿ 3. Checklist Detalhado de Recursos de Acessibilidade (Filtros do Mapa)

Para que o sistema de filtros funcione com precisão, cada academia deve preencher as seguintes categorias, detalhando o que realmente oferece:

### 3.1. Acesso e Circulação (`acesso_cadeirantes` & `espaco_circulacao`)

* **Entrada Acessível:** Possui entrada em nível (sem degraus) ou rampa de acesso com inclinação adequada e corrimão.

* **Elevador / Plataforma Elevatória:** Obrigatório caso a academia possua mais de um pavimento com áreas de treino ou vestiários.

* **Largura dos Corredores e Vãos:** Portas e passagens entre equipamentos com largura mínima livre que permita o trânsito e o giro de cadeiras de rodas.

* **Piso Seguro:** Piso plano, regular, antiderrapante e livre de obstáculos soltos (anilhas, halteres ou cabos atravessados).

* **Vaga de Estacionamento Reservada:** Disponibilidade de vagas demarcadas para PcD/Idosos próximas à entrada principal.

### 3.2. Equipamentos e Adaptações de Treino (`aparelhos_adaptados`)

* **Máquinas com Assento Removível / Articulado:** Permitem que o usuário cadeirante treine diretamente da própria cadeira de rodas (ex.: *puxada alta, supino articulado, crossover adaptado*).

* **Equipamentos com Facilidade de Transferência:** Bancos com altura acessível e apoios laterais firmes para transferência segura.

* **Acessórios de Adaptação Disponíveis:**

  * Ganchos de pulso / luvas de tração (para amputados ou pessoas com pouca força de preensão);

  * Cintos de estabilização de tronco/quadril;

  * Halteres, barras e anilhas de fácil manuseio;

  * Máquina de cabo (polia) com regulagem ampla de altura.

* **Lista de Aparelhos Adaptados:** Campo descritivo onde a academia informa exatamente quais máquinas adaptadas possui no salão.

### 3.3. Banheiros e Vestiários (`banheiros_adaptados` & `vestiarios_acessiveis`)

* **Sanitário Acessível:** Cabine ampla para manobra de cadeira de rodas, vaso sanitário com altura adequada e barras de apoio laterais e de fundo.

* **Pia e Espelho Acessíveis:** Lavatório suspenso (sem coluna que impeça a aproximação frontal da cadeira) e torneira de fácil acionamento.

* **Chuveiro Adaptado:** Área de banho em nível (sem degrau/box estreito), equipada com banco articulado e barras de apoio.

* **Armários em Altura Acessível:** Guarda-volumes posicionados ao alcance de pessoas sentadas ou de baixa estatura.

### 3.4. Atendimento e Comunicação Inclusiva (`profissionais_capacitados`)

* **Instrutores com Formação em Treino Adaptado:** Professores de Educação Física capacitados para prescrever e acompanhar treinos de pessoas com deficiência física, sensorial ou intelectual.

* **Comunicação em Libras:** Presença de funcionários ou instrutores com conhecimento em Língua Brasileira de Sinais, ou suporte visual para atendimento.

* **Sinalização Tátil e Visual:**

  * Piso tátil direcional e de alerta (entrada, escadas, recepção);

  * Identificação em Braille ou alto-relevo nos vestiários e equipamentos;

  * Sinalização visual de emergência (alertas luminosos além dos sonoros).

## ✅ 4. Sistema de Verificação e Confiabilidade

Conforme previsto na regra de negócio do **INCLUA**, para evitar que uma academia se declare "100% acessível" sem comprovação real, a plataforma exibirá três níveis de selo no perfil do estabelecimento:

1. **🟡 Autodeclarada (`autodeclarada`):** A academia preencheu o checklist de acessibilidade no cadastro, mas as informações ainda não passaram por validação.

2. **🔵 Verificada por Evidências (`verificada_evidencia`):** A academia anexou fotos comprobatórias de cada item marcado (rampas, banheiros, aparelhos) e teve os dados aprovados pela moderação da plataforma.

3. **🟢 Validada pela Comunidade PcD (`validada_comunidade`):** Além das evidências, a academia possui média igual ou superior a **4.0 estrelas** nas avaliações específicas de acessibilidade feitas por usuários PcD cadastrados.

## 📊 5. Indicadores de Avaliação e Solicitações Vinculadas

No perfil público da academia, os usuários também visualizarão métricas dinâmicas alimentadas pelos módulos de **Avaliação** e **Solicitações de Acessibilidade**:

* **Nota Geral de Acessibilidade (0 a 5 estrelas):** Média calculada com base em:

  * *Condições dos equipamentos adaptados*;

  * *Acessibilidade do espaço físico e banheiros*;

  * *Atendimento e preparo dos profissionais*.

* **Índice de Resolução de Acessibilidade:** Percentual de solicitações de melhoria (ex.: *pedido de rampa, ajuste de espaço, sinalização*) que foram respondidas e resolvidas pela administração da academia.