# 🪪 Definição de Informações Necessárias no Perfil dos Usuários

* **Projeto:** INCLUA (Plataforma de Segurança, Acessibilidade e Inclusão em Academias)
* **Módulo Relacionado:** Módulo 1 – Cadastro, Autenticação e Perfis de Usuários
* **Responsável:** Lorena Fontana Souza

## 🎯 1. Objetivo

Especificar os dados obrigatórios e opcionais necessários para o cadastro e a gestão de perfil de cada tipo de usuário na plataforma **INCLUA** (Alunos, Alunos PcD, Professores/Funcionários e Administradores), garantindo personalização da experiência, segurança e permissões de acesso adequadas.

---

## 🔐 2. Dados Comuns a Todos os Perfis (Autenticação e Identificação Básica)

Estes campos formam a base da tabela de usuários no banco de dados (Supabase) e são exigidos no momento do registro ou edição de perfil:

| Campo | Tipo de Dado | Obrigatório? | Descrição / Regra de Negócio |
| :--- | :--- | :---: | :--- |
| `id` | UUID | Sim (Auto) | Identificador único gerado automaticamente pelo sistema. |
| `nome_completo` | Texto | Sim | Nome social ou civil completo do usuário. |
| `email` | Texto | Sim | E-mail válido e único, utilizado para login e recuperação de senha. |
| `senha` | Hash (Senha) | Sim | Senha criptografada (mínimo de 8 caracteres, letras e números). |
| `telefone` | Texto | Opcional | Número com DDD para contato em caso de emergências ou suporte. |
| `foto_perfil` | URL / Imagem | Opcional | Avatar do usuário para exibição em comentários, avaliações e ocorrências. |
| `tipo_perfil` | Enum | Sim | Define o nível de permissão: `aluno`, `professor`, `admin`. |
| `data_criacao` | Timestamp | Sim (Auto) | Data e hora do cadastro na plataforma. |

---

## 🧩 3. Informações Específicas por Tipo de Usuário

Para atender às particularidades de inclusão, segurança e gestão do **INCLUA**, cada perfil possui campos complementares:

### 3.1. Perfil: Aluno (Geral e Pessoa com Deficiência / Mobilidade Reduzida)

Focado na personalização dos filtros do **Mapa de Acessibilidade**, segurança no treino e comunicação de ocorrências.

* **Dados Pessoais e de Localização:**
  * `data_nascimento` *(Data - Obrigatório)*: Validação de faixa etária e adequação de atendimento.
  * `cidade` e `cep` *(Texto - Opcional)*: Facilita a busca inicial de academias próximas caso a geolocalização esteja desativada.
* **Perfil de Acessibilidade (Opcional / Recomendado para PcD):**
  * `possui_deficiencia` *(Booleano - Sim/Não)*: Habilita os campos específicos de adaptação.
  * `tipo_deficiencia` *(Seleção Múltipla)*: Ex.: *Física / Mobilidade Reduzida (Cadeirante, Amputado, Próteses)*, *Visual*, *Auditiva*, *Intelectual*, *Outra*.
  * `necessidades_especificas` *(Tags / Checkboxes)*: Espelha os filtros do mapa para recomendar academias compatíveis:
    * Precisa de acesso para cadeirante (rampas/elevadores);
    * Necessita de aparelhos adaptados;
    * Necessita de banheiro/vestiário adaptado;
    * Necessita de acompanhamento profissional capacitado (ex.: Libras, treino adaptado);
    * Necessita de sinalização tátil ou audiodescrição.
  * `observacoes_seguranca` *(Texto Livre - Opcional)*: Espaço para o aluno relatar restrições médicas relevantes ou adaptações específicas de treino.
* **Contato de Emergência (Opcional):**
  * `nome_contato_emergencia` e `telefone_emergencia` *(Texto)*: Informação de segurança para acionamento rápido em caso de acidentes na academia.

### 3.2. Perfil: Professor / Funcionário

Focado na identificação profissional dentro da academia, acompanhamento de alunos e registro de ocorrências/manutenções.

* **Vínculo Institucional:**
  * `academia_vinculada_id` *(UUID - Obrigatório)*: Relaciona o profissional à academia onde trabalha.
  * `cargo` *(Seleção - Obrigatório)*: Ex.: *Instrutor / Educador Físico*, *Recepção*, *Equipe de Manutenção*.
* **Qualificação Profissional:**
  * `registro_cref` *(Texto - Opcional/Condicional)*: Obrigatório caso o cargo seja *Instrutor / Educador Físico*, garantindo credibilidade técnica.
  * `especialidades_inclusao` *(Seleção Múltipla - Opcional)*: Ex.: *Treinamento Adaptado para PcD*, *Comunicação em Libras*, *Primeiros Socorros*. (Ajuda a validar o selo de "Profissionais capacitados" da academia).

### 3.3. Perfil: Administrador (Gestor da Academia)

Focado na gestão da unidade, validação de infraestrutura acessível e acesso ao **Dashboard Administrativo**.

* **Vínculo e Responsabilidade:**
  * `academia_gerenciada_id` *(UUID - Obrigatório)*: Academia sob responsabilidade do gestor.
  * `cargo_gestao` *(Texto - Obrigatório)*: Ex.: *Proprietário*, *Gerente Geral*, *Coordenador Técnico*.
  * `documento_responsavel` *(Texto - Obrigatório)*: CPF ou CNPJ vinculado à administração para validação de autenticidade do estabelecimento.
* **Permissões Exclusivas:**
  * Editar perfil da academia no **Mapa de Acessibilidade**;
  * Cadastrar equipamentos e gerar **QR Codes**;
  * Alterar status de ocorrências e solicitações de acessibilidade (*Pendente*, *Em análise*, *Resolvida*);
  * Visualizar relatórios e estatísticas de segurança.

---

## 🛡️ 4. Matriz de Permissões por Perfil

| Funcionalidade do INCLUA | Aluno | Professor / Funcionário | Administrador |
| :--- | :---: | :---: | :---: |
| Consultar Mapa de Acessibilidade e Avaliações | ✅ | ✅ | ✅ |
| Registrar Ocorrências e Acidentes | ✅ | ✅ | ✅ |
| Escanear QR Code para reportar defeito em aparelho | ✅ | ✅ | ✅ |
| Enviar Solicitações de Acessibilidade | ✅ | ✅ | ✅ |
| Avaliar Academias | ✅ | ❌ | ❌ |
| Interditar Aparelho Temporariamente | ❌ | ✅ | ✅ |
| Gerenciar Manutenções e Cadastro de Aparelhos | ❌ | ❌ | ✅ |
| Atualizar Status de Ocorrências / Solicitações | ❌ | ❌ | ✅ |
| Acessar Dashboard Administrativo e Relatórios | ❌ | ❌ | ✅ |

---

## 🔒 5. Boas Práticas de Privacidade e UX (LGPD e Inclusão)

1. **Privacidade de Dados Sensíveis (LGPD):** Informações sobre deficiência e saúde (`tipo_deficiencia`, `observacoes_seguranca`) são consideradas dados sensíveis. O preenchimento deve ser **opcional**, com consentimento explícito do usuário, e visível apenas para o próprio aluno (ou para profissionais da academia mediante autorização).
2. **Cadastro Progressivo:** Para não tornar o primeiro acesso cansativo, o cadastro inicial solicita apenas *Nome, E-mail, Senha e Tipo de Perfil*. Os dados complementares de acessibilidade ou vínculo profissional são preenchidos na tela **"Meu Perfil"**.
3. **Interface Acessível:** Todos os formulários de perfil devem conter rótulos claros (`<label>`), suporte a navegação por teclado, compatibilidade com leitores de tela e alto contraste.