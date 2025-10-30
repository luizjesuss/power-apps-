📱 Documentação Power Apps - Sistema de Bonificação PMZ

## 📋 Índice

1. [Visão Geral](#visão-geral)
2. [Arquitetura da Solução](#arquitetura-da-solução)
3. [Estrutura de Dados SharePoint](#estrutura-de-dados-sharepoint)
4. [Power Automate Flows](#power-automate-flows)
5. [Telas do Power Apps](#telas-do-power-apps)
6. [Componentes Reutilizáveis](#componentes-reutilizáveis)
7. [Fórmulas e Lógica](#fórmulas-e-lógica)
8. [Permissões e Segurança](#permissões-e-segurança)
9. [Guia de Implementação Passo a Passo](#guia-de-implementação-passo-a-passo)
10. [Troubleshooting](#troubleshooting)

---

## 🎯 Visão Geral

### Objetivo

Sistema automatizado para gerenciar o processo mensal de bonificação de operações da PMZ, eliminando tarefas manuais e reduzindo erros operacionais.

### Stakeholders

- **Coordenadores Regionais**: Aprovam/reprovam bonificações
- **RH**: Recebe dados aprovados para processamento
- **Colaboradores**: Recebem reconhecimento por metas atingidas
- **TI/Administradores**: Gerenciam e monitoram o sistema

### Tecnologias

- **Power Apps Canvas**: Interface do usuário
- **SharePoint Online**: Armazenamento de dados
- **Power Automate**: Automações e fluxos
- **Outlook/Teams**: Notificações e reconhecimento

---

## 🏗️ Arquitetura da Solução

```
┌─────────────────────────────────────────────────────────┐
│                    POWER APPS CANVAS                     │
│  ┌──────────┬──────────┬──────────┬──────────┬────────┐ │
│  │Dashboard │ Execução │ Aprovação│  Envio RH│Reconhe-│ │
│  │          │          │          │          │cimento │ │
│  └──────────┴──────────┴──────────┴──────────┴────────┘ │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│                   POWER AUTOMATE FLOWS                   │
│  ┌──────────────┬──────────────┬────────────────────┐  │
│  │ Execução     │ Validação    │ Envio de          │  │
│  │ Mensal (Dia 8)│ de Dados    │ Notificações      │  │
│  └──────────────┴──────────────┴────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│                  SHAREPOINT ONLINE LISTS                 │
│  ┌─────────────┬─────────────┬─────────────┬─────────┐ │
│  │ Colabora-   │ Aprovações  │ Histórico   │ Config. │ │
│  │ dores       │             │ Execuções   │         │ │
│  └─────────────┴─────────────┴─────────────┴─────────┘ │
└─────────────────────────────────────────────────────────┘
```

---

## 📊 Estrutura de Dados SharePoint

### Lista 1: **Colaboradores_Bonificacao**

| Nome da Coluna   | Tipo                | Obrigatório | Descrição                                     |
| ---------------- | ------------------- | ----------- | --------------------------------------------- |
| Title            | Single line of text | Sim         | Nome do colaborador                           |
| ID_Colaborador   | Single line of text | Sim         | ID único do colaborador                       |
| Funcao           | Choice              | Sim         | ADM de Loja, Líder de Caixa, Líder de Estoque |
| Loja             | Single line of text | Sim         | Nome da loja                                  |
| Regiao           | Choice              | Sim         | São Paulo, Rio de Janeiro, Minas Gerais, etc. |
| PercentualMeta   | Number              | Sim         | Percentual da meta atingida (0-200)           |
| ValorBonificacao | Currency            | Sim         | Valor da bonificação em R$                    |
| Status_Ativo     | Yes/No              | Sim         | Colaborador ativo/desligado                   |
| Mes_Ano          | Single line of text | Sim         | Formato: "2025-10"                            |
| Data_Criacao     | Date and Time       | Sim         | Data de criação do registro                   |
| Criado_Por       | Person or Group     | Sim         | Sistema/Administrador                         |

**Configurações:**

- Versionamento: Sim (histórico de alterações)
- Aprovação de conteúdo: Não
- Validação: PercentualMeta >= 0 AND PercentualMeta <= 200

---

### Lista 2: **Aprovacoes_Bonificacao**

| Nome da Coluna    | Tipo                   | Obrigatório | Descrição                                    |
| ----------------- | ---------------------- | ----------- | -------------------------------------------- |
| Title             | Single line of text    | Sim         | Auto-gerado (ID_Aprovacao)                   |
| ID_Colaborador    | Lookup                 | Sim         | Referência à lista Colaboradores_Bonificacao |
| Nome_Colaborador  | Single line of text    | Sim         | Nome do colaborador                          |
| Funcao            | Choice                 | Sim         | Função do colaborador                        |
| Loja              | Single line of text    | Sim         | Loja do colaborador                          |
| Regiao            | Choice                 | Sim         | Região do colaborador                        |
| PercentualMeta    | Number                 | Sim         | Percentual da meta                           |
| ValorBonificacao  | Currency               | Sim         | Valor da bonificação                         |
| Status_Aprovacao  | Choice                 | Sim         | Pendente, Aprovado, Reprovado                |
| Aprovador         | Person or Group        | Não         | Coordenador que aprovou/reprovou             |
| Data_Aprovacao    | Date and Time          | Não         | Data da aprovação/reprovação                 |
| Motivo_Reprovacao | Multiple lines of text | Não         | Justificativa obrigatória se reprovado       |
| Dados_Corrigidos  | Yes/No                 | Sim         | Indica se dados foram corrigidos             |
| Meta_Original     | Number                 | Não         | Valor original antes da correção             |
| Bonus_Original    | Currency               | Não         | Valor original antes da correção             |
| Mes_Ano           | Single line of text    | Sim         | Período de referência                        |

**Configurações:**

- Versionamento: Sim
- Validação: Se Status_Aprovacao = "Reprovado", Motivo_Reprovacao é obrigatório

---

### Lista 3: **Historico_Execucoes**

| Nome da Coluna        | Tipo                   | Obrigatório | Descrição                          |
| --------------------- | ---------------------- | ----------- | ---------------------------------- |
| Title                 | Single line of text    | Sim         | ID da execução                     |
| Data_Execucao         | Date and Time          | Sim         | Data e hora da execução            |
| Status_Execucao       | Choice                 | Sim         | Concluído, Erro, Em Andamento      |
| Registros_Processados | Number                 | Sim         | Total de registros processados     |
| Registros_Validados   | Number                 | Sim         | Registros ativos validados         |
| Registros_Excluidos   | Number                 | Sim         | Colaboradores desligados excluídos |
| Mes_Ano               | Single line of text    | Sim         | Período processado                 |
| Mensagens_Erro        | Multiple lines of text | Não         | Detalhes de erros, se houver       |
| Tempo_Execucao        | Single line of text    | Não         | Duração da execução                |
| Executado_Por         | Person or Group        | Sim         | Usuário/Sistema                    |

---

### Lista 4: **Envios_RH**

| Nome da Coluna       | Tipo                | Obrigatório | Descrição                         |
| -------------------- | ------------------- | ----------- | --------------------------------- |
| Title                | Single line of text | Sim         | ID do envio                       |
| Data_Envio           | Date and Time       | Sim         | Data e hora do envio              |
| Quantidade_Registros | Number              | Sim         | Quantidade de aprovações enviadas |
| Valor_Total          | Currency            | Sim         | Soma total das bonificações       |
| Status_Envio         | Choice              | Sim         | Pendente, Enviado, Confirmado     |
| Data_Confirmacao     | Date and Time       | Não         | Data de confirmação pelo RH       |
| Confirmado_Por       | Person or Group     | Não         | Pessoa do RH que confirmou        |
| Mes_Ano              | Single line of text | Sim         | Período de referência             |
| Arquivo_Anexo        | Attachment          | Não         | Excel/PDF com detalhes            |

---

### Lista 5: **Configuracoes_Sistema**

| Nome da Coluna   | Tipo                   | Obrigatório | Descrição                     |
| ---------------- | ---------------------- | ----------- | ----------------------------- |
| Title            | Single line of text    | Sim         | Nome da configuração          |
| Chave            | Single line of text    | Sim         | Chave única da configuração   |
| Valor            | Single line of text    | Sim         | Valor da configuração         |
| Tipo             | Choice                 | Sim         | Texto, Número, Data, Booleano |
| Descricao        | Multiple lines of text | Não         | Descrição da configuração     |
| Ultima_Alteracao | Date and Time          | Sim         | Data da última modificação    |
| Alterado_Por     | Person or Group        | Sim         | Quem alterou                  |

**Configurações Padrão:**

```
Chave: DiaExecucaoMensal, Valor: 8
Chave: HorarioExecucao, Valor: 06:00
Chave: EmailRH, Valor: rh@pmz.com.br
Chave: MetaMinima, Valor: 100
Chave: EmailAtivo, Valor: true
```

---

## ⚡ Power Automate Flows

### Flow 1: **[Recorrente] Execução Mensal Bonificação**

**Gatilho:** Recurrence (Mensal, dia 8, às 06:00)

**Passos:**

1. **Criar Registro de Execução**

   ```
   Ação: Create item (Historico_Execucoes)
   Status_Execucao: "Em Andamento"
   Data_Execucao: utcNow()
   ```

2. **Buscar Dados Operacionais**

   ```
   Ação: Get items (fonte de dados operacionais)
   Filter Query: Mes_Ano eq '@{formatDateTime(utcNow(), 'yyyy-MM')}'
   ```

3. **Filtrar Colaboradores Ativos**

   ```
   Ação: Filter array
   Condição: Status_Ativo eq true
   ```

4. **Loop: Processar Cada Colaborador**

   ```
   Ação: Apply to each
   - Criar/Atualizar em Colaboradores_Bonificacao
   - Criar registro em Aprovacoes_Bonificacao (Status: Pendente)
   ```

5. **Atualizar Registro de Execução**

   ```
   Ação: Update item (Historico_Execucoes)
   Status_Execucao: "Concluído"
   Registros_Processados: length(outputs('Buscar_Dados'))
   Registros_Validados: length(outputs('Filtrar_Ativos'))
   ```

6. **Notificar Coordenadores**
   ```
   Ação: Send an email (V2)
   Para: grupo.coordenadores@pmz.com.br
   Assunto: "Nova execução disponível para aprovação"
   ```

---

### Flow 2: **Validação de Dados ao Criar Registro**

**Gatilho:** When an item is created (Colaboradores_Bonificacao)

**Passos:**

1. **Validar Integridade**

   ```
   Condição: PercentualMeta >= 0 AND PercentualMeta <= 200
   Se Falso: Enviar alerta e deletar registro
   ```

2. **Calcular Bonificação Automaticamente**

   ```
   Se PercentualMeta >= 100:
     ValorBonificacao = baseBonus * (PercentualMeta / 100)
   Senão:
     ValorBonificacao = 0
   ```

3. **Verificar Duplicatas**
   ```
   Filter: ID_Colaborador eq @{triggerBody()?['ID_Colaborador']}
           AND Mes_Ano eq @{triggerBody()?['Mes_Ano']}
   Se length(outputs) > 1: Marcar como duplicata
   ```

---

### Flow 3: **Notificação de Aprovação/Reprovação**

**Gatilho:** When an item is created or modified (Aprovacoes_Bonificacao)

**Condição:** Status_Aprovacao mudou para "Aprovado" ou "Reprovado"

**Passos:**

1. **Se Aprovado:**

   ```
   Enviar email ao colaborador
   Assunto: "Sua bonificação foi aprovada!"
   Incluir: valor, percentual, data de pagamento prevista
   ```

2. **Se Reprovado:**
   ```
   Enviar email ao colaborador e gestor
   Assunto: "Bonificação em análise"
   Incluir: motivo da reprovação, próximos passos
   ```

---

### Flow 4: **Envio Consolidado ao RH**

**Gatilho:** Manual (botão no Power Apps)

**Parâmetros de Entrada:** MesAno (string)

**Passos:**

1. **Buscar Aprovações Aprovadas**

   ```
   Filter: Status_Aprovacao eq 'Aprovado' AND Mes_Ano eq '@{triggerBody()?['MesAno']}'
   ```

2. **Criar Excel com Dados**

   ```
   Ação: Create CSV table
   Colunas: Nome, Função, Loja, Região, Meta, Bonificação
   ```

3. **Criar Registro de Envio**

   ```
   Create item (Envios_RH)
   Status_Envio: "Enviado"
   Anexar Excel criado
   ```

4. **Enviar Email ao RH**

   ```
   Para: [Email do RH]
   Anexo: Excel com dados
   Corpo: Resumo executivo (quantidade, valor total)
   ```

5. **Atualizar Status das Aprovações**
   ```
   Apply to each: Update Status para "Enviado ao RH"
   ```

---

### Flow 5: **Envio de Cards de Reconhecimento**

**Gatilho:** Manual (botão no Power Apps)

**Parâmetros:** ID_Colaborador (string)

**Passos:**

1. **Buscar Dados do Colaborador**

   ```
   Get item (Aprovacoes_Bonificacao)
   Filter: ID eq '@{triggerBody()?['ID_Colaborador']}'
   ```

2. **Gerar Card HTML Personalizado**

   ```
   Variável HTML com:
   - Nome, função, loja
   - Badge de conquista (Platinum/Gold/Silver/Bronze)
   - Percentual da meta
   - Mensagem de parabéns
   ```

3. **Determinar Nível de Conquista**

   ```
   Se >= 115%: Platinum
   Se >= 110%: Gold
   Se >= 105%: Silver
   Se >= 100%: Bronze
   ```

4. **Enviar Email com Card**

   ```
   Para: email do colaborador
   Formato: HTML
   Incluir: Card visual + mensagem motivacional
   ```

5. **Registrar Envio**
   ```
   Update item: adicionar campo "Card_Enviado" = true
   Data_Envio_Card: utcNow()
   ```

---

## 📱 Telas do Power Apps

### Tela 1: **Home / Dashboard**

**Componentes:**

1. **Header**

   ```
   Componente: Rectangle
   Fill: RGBA(24, 34, 76, 1) // Cor #18224c
   Altura: 80px

   Label: "Sistema de Bonificação PMZ"
   Color: White
   Font: Segoe UI Bold, 24
   ```

2. **Cards de KPI** (6 cards em grid)

   ```
   Gallery: galKPIs
   Items:
   [
     {Titulo: "Pendentes", Valor: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Pendente")), Icone: "⏰", Cor: RGBA(245, 158, 11, 1)},
     {Titulo: "Aprovados", Valor: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Aprovado")), Icone: "✓", Cor: RGBA(34, 197, 94, 1)},
     // ... outros KPIs
   ]

   Template:
   - Rectangle (cor do card)
   - Label Título
   - Label Valor (fonte grande)
   - Icon
   ```

3. **Última Execução**

   ```
   Componente: Container

   Label: "Última Execução"
   Label Data: First(Sort(Historico_Execucoes, Data_Execucao, Descending)).Data_Execucao
   Label Processados: First(...).Registros_Processados
   Label Validados: First(...).Registros_Validados
   ```

**Fórmulas Principais:**

```javascript
// OnVisible da Tela
ClearCollect(
    colDashboardData,
    {
        Pendentes: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Pendente")),
        Aprovados: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Aprovado")),
        Reprovados: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Reprovado")),
        ValorTotal: Sum(Filter(Aprovacoes_Bonificacao, Status_Aprovacao = "Aprovado"), ValorBonificacao)
    }
);

// Refresh automático a cada 5 minutos
Set(varLastRefresh, Now());
```

---

### Tela 2: **Execução e Agendamento**

**Componentes:**

1. **Card de Próxima Execução**

   ```
   Label: "Próxima Execução Programada"
   Data: DateAdd(Today(), 8 - Day(Today()), Days) // Próximo dia 8

   Label Contagem: DateDiff(Today(), [Data Próxima], Days) & " dias"
   ```

2. **Botão Execução Manual**

   ```
   Button: btnExecutarManual
   OnSelect:
   [
     // Criar registro de execução
     Patch(Historico_Execucoes, Defaults(Historico_Execucoes),
     {
         Title: "EXEC-" & Text(Now(), "yyyyMMddHHmmss"),
         Data_Execucao: Now(),
         Status_Execucao: {Value: "Em Andamento"},
         Executado_Por: {Claims: "i:0#.f|membership|" & User().Email}
     });

     // Chamar Flow de execução
     [Seu Flow].Run(Text(Now(), "yyyy-MM"));

     // Mostrar notificação
     Notify("Execução iniciada com sucesso!", NotificationType.Success);
   ]
   ```

3. **Histórico de Execuções**

   ```
   Gallery: galHistoricoExecucoes
   Items: Sort(Historico_Execucoes, Data_Execucao, Descending)

   Template:
   - Data e hora
   - Status (com ícone colorido)
   - Registros processados/validados/excluídos
   ```

---

### Tela 3: **Interface de Aprovação**

**Componentes:**

1. **Filtros**

   ```
   Dropdown: ddFuncao
   Items: ["Todos", "ADM de Loja", "Líder de Caixa", "Líder de Estoque"]
   OnChange: Set(varFuncaoFiltro, Self.Selected.Value)

   Dropdown: ddRegiao
   Items: ["Todas"] & Distinct(Aprovacoes_Bonificacao, Regiao.Value)
   ```

2. **Gallery de Registros Pendentes**

   ```
   Gallery: galAprovacoes
   Items:
   Filter(
       Aprovacoes_Bonificacao,
       Status_Aprovacao.Value = "Pendente" &&
       (varFuncaoFiltro = "Todos" || Funcao.Value = varFuncaoFiltro)
   )

   Template Components:
   - Nome colaborador (Label, fonte 16, negrito)
   - Badge função
   - Loja e região
   - Percentual meta (com ícone de tendência)
   - Valor bonificação (destacado)
   - Botão Aprovar (verde)
   - Botão Reprovar (vermelho)
   - Botão Corrigir (cinza)
   ```

3. **Modal de Reprovação**

   ```
   Componente: Container + Rectangle (backdrop)
   Visible: varMostrarModalReprovacao

   TextInput: txtMotivoReprovacao
   HintText: "Digite o motivo da reprovação (obrigatório)"
   Required: true

   Button: btnConfirmarReprovacao
   OnSelect:
   If(
       Len(txtMotivoReprovacao.Text) > 10,
       Patch(
           Aprovacoes_Bonificacao,
           LookUp(Aprovacoes_Bonificacao, ID = varRegistroSelecionado),
           {
               Status_Aprovacao: {Value: "Reprovado"},
               Motivo_Reprovacao: txtMotivoReprovacao.Text,
               Aprovador: {Claims: "i:0#.f|membership|" & User().Email},
               Data_Aprovacao: Now()
           }
       );
       Notify("Registro reprovado com sucesso!", NotificationType.Success);
       Set(varMostrarModalReprovacao, false);
       ,
       Notify("Justificativa deve ter pelo menos 10 caracteres", NotificationType.Error)
   )
   ```

4. **Modal de Correção**

   ```
   TextInput: txtMetaCorrigida
   Format: Number
   Default: LookUp(Aprovacoes_Bonificacao, ID = varRegistroSelecionado).PercentualMeta

   TextInput: txtBonusCorrigido
   Format: Currency
   Default: LookUp(Aprovacoes_Bonificacao, ID = varRegistroSelecionado).ValorBonificacao

   Button: btnSalvarCorrecao
   OnSelect:
   Patch(
       Aprovacoes_Bonificacao,
       LookUp(Aprovacoes_Bonificacao, ID = varRegistroSelecionado),
       {
           Meta_Original: Self.PercentualMeta,
           Bonus_Original: Self.ValorBonificacao,
           PercentualMeta: Value(txtMetaCorrigida.Text),
           ValorBonificacao: Value(txtBonusCorrigido.Text),
           Dados_Corrigidos: true
       }
   );
   ```

**Fórmulas de Aprovação Rápida:**

```javascript
// Botão Aprovar (dentro do Gallery)
OnSelect:
Patch(
    Aprovacoes_Bonificacao,
    ThisItem,
    {
        Status_Aprovacao: {Value: "Aprovado"},
        Aprovador: {Claims: "i:0#.f|membership|" & User().Email},
        Data_Aprovacao: Now()
    }
);
Notify("✓ Aprovação registrada", NotificationType.Success, 2000);

// Botão Reprovar
OnSelect:
Set(varRegistroSelecionado, ThisItem.ID);
Set(varMostrarModalReprovacao, true);

// Botão Corrigir
OnSelect:
Set(varRegistroSelecionado, ThisItem.ID);
Set(varMostrarModalCorrecao, true);
```

---

### Tela 4: **Envio ao RH**

**Componentes:**

1. **Cards de Resumo**

   ```
   Labels:
   - Total de Registros Aprovados
   - Valor Total a Pagar
   - Quantidade de Envios Realizados
   ```

2. **Resumo por Função**

   ```
   Gallery: galResumoFuncao
   Items:
   AddColumns(
       GroupBy(
           Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"),
           "Funcao",
           "Dados"
       ),
       "Quantidade", CountRows(Dados),
       "Total", Sum(Dados, ValorBonificacao)
   )
   ```

3. **Botão Enviar ao RH**

   ```
   Button: btnEnviarRH
   DisplayMode:
   If(
       CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado")) > 0,
       DisplayMode.Edit,
       DisplayMode.Disabled
   )

   OnSelect:
   // Criar registro de envio
   Set(varNovoEnvio,
       Patch(Envios_RH, Defaults(Envios_RH), {
           Title: "ENV-" & Text(Now(), "yyyyMMdd-HHmmss"),
           Data_Envio: Now(),
           Quantidade_Registros: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado")),
           Valor_Total: Sum(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"), ValorBonificacao),
           Status_Envio: {Value: "Enviado"},
           Mes_Ano: Text(Today(), "yyyy-MM")
       })
   );

   // Chamar Flow de envio
   FlowEnvioRH.Run(Text(Today(), "yyyy-MM"));

   // Mostrar confirmação
   Set(varEnvioSucesso, true);
   Notify("Dados enviados ao RH com sucesso!", NotificationType.Success, 5000);
   ```

4. **Histórico de Envios**

   ```
   Gallery: galHistoricoEnvios
   Items: Sort(Envios_RH, Data_Envio, Descending)

   Badge de Status:
   Fill:
   Switch(
       ThisItem.Status_Envio.Value,
       "Confirmado", RGBA(34, 197, 94, 1),
       "Enviado", RGBA(59, 130, 246, 1),
       "Pendente", RGBA(156, 163, 175, 1)
   )
   ```

---

### Tela 5: **Reconhecimento de Colaboradores**

**Componentes:**

1. **Cards de Níveis de Conquista**

   ```
   Gallery Horizontal: galNiveis
   Items:
   [
       {Nivel: "Platinum", Meta: "≥115%", Cor: RGBA(147, 51, 234, 1), Qtd: CountRows(Filter(Aprovacoes_Bonificacao, PercentualMeta >= 115))},
       {Nivel: "Gold", Meta: "110-114%", Cor: RGBA(234, 179, 8, 1), Qtd: CountRows(Filter(Aprovacoes_Bonificacao, PercentualMeta >= 110 && PercentualMeta < 115))},
       {Nivel: "Silver", Meta: "105-109%", Cor: RGBA(156, 163, 175, 1), Qtd: ...},
       {Nivel: "Bronze", Meta: "100-104%", Cor: RGBA(249, 115, 22, 1), Qtd: ...}
   ]
   ```

2. **Gallery de Cards de Reconhecimento**

   ```
   Gallery: galCardsReconhecimento
   Items:
   Filter(
       Aprovacoes_Bonificacao,
       Status_Aprovacao.Value = "Aprovado" && PercentualMeta >= 100
   )

   Template (Card Personalizado):

   // Header com gradiente
   Rectangle:
   Fill: Switch(
       ThisItem.PercentualMeta,
       >= 115, ColorFade(RGBA(147, 51, 234, 1), -20%),
       >= 110, ColorFade(RGBA(234, 179, 8, 1), -20%),
       >= 105, ColorFade(RGBA(156, 163, 175, 1), -20%),
       ColorFade(RGBA(249, 115, 22, 1), -20%)
   )

   // Conteúdo do card
   Label Nome: ThisItem.Nome_Colaborador
   Label Função: ThisItem.Funcao.Value
   Label Loja: ThisItem.Loja
   Label Meta: ThisItem.PercentualMeta & "%"
   Label Bonificação: Text(ThisItem.ValorBonificacao, "[$-pt-BR]R$ #,##0.00")

   // Ícone de conquista
   Icon:
   Icon: Switch(
       ThisItem.PercentualMeta,
       >= 115, Icon.Trophy,
       >= 110, Icon.Medal,
       >= 105, Icon.Star,
       Icon.Like
   )
   ```

3. **Botão Enviar Email Individual**

   ```
   Button: btnEnviarCard
   OnSelect:
   // Chamar Flow de envio de card
   FlowEnvioCard.Run(ThisItem.ID_Colaborador);

   // Marcar como enviado
   Patch(
       Aprovacoes_Bonificacao,
       ThisItem,
       {Card_Enviado: true, Data_Envio_Card: Now()}
   );

   Notify("✉️ Card enviado para " & ThisItem.Nome_Colaborador, NotificationType.Success);
   ```

4. **Botão Enviar Todos**
   ```
   Button: btnEnviarTodosCards
   OnSelect:
   ForAll(
       Filter(galCardsReconhecimento.AllItems, !Card_Enviado),
       FlowEnvioCard.Run(ID_Colaborador);
       Patch(
           Aprovacoes_Bonificacao,
           LookUp(Aprovacoes_Bonificacao, ID = ID_Colaborador),
           {Card_Enviado: true, Data_Envio_Card: Now()}
       )
   );
   Notify("✉️ " & CountRows(Filter(...)) & " cards enviados com sucesso!", NotificationType.Success);
   ```

---

## 🧩 Componentes Reutilizáveis

### Componente 1: **KPI_Card**

**Propriedades de Entrada:**

- `Titulo` (Text): Título do KPI
- `Valor` (Text/Number): Valor a ser exibido
- `Icone` (Text): Nome do ícone
- `CorFundo` (Color): Cor de fundo do ícone

**Estrutura:**

```
Container:
├── Rectangle (fundo branco, sombra)
├── Rectangle (ícone - cor variável)
├── Icon
├── Label Título
└── Label Valor
```

**Uso:**

```
KPI_Card {
    Titulo: "Pendentes",
    Valor: Text(varPendentes),
    Icone: Icon.Clock,
    CorFundo: RGBA(245, 158, 11, 0.1)
}
```

---

### Componente 2: **Badge_Status**

**Propriedades:**

- `Status` (Text): Pendente, Aprovado, Reprovado
- `Tamanho` (Text): Pequeno, Médio, Grande

**Estrutura:**

```
Rectangle:
Fill: Switch(
    Status,
    "Aprovado", RGBA(34, 197, 94, 0.1),
    "Reprovado", RGBA(239, 68, 68, 0.1),
    RGBA(245, 158, 11, 0.1)
)

Label:
Color: Switch(
    Status,
    "Aprovado", RGBA(34, 197, 94, 1),
    "Reprovado", RGBA(239, 68, 68, 1),
    RGBA(245, 158, 11, 1)
)
```

---

### Componente 3: **Modal_Confirmacao**

**Propriedades:**

- `Visivel` (Boolean)
- `Titulo` (Text)
- `Mensagem` (Text)
- `OnConfirmar` (Action)
- `OnCancelar` (Action)

**Estrutura:**

```
Group:
├── Rectangle (backdrop escuro, semi-transparente)
├── Container (modal)
│   ├── Label Título
│   ├── Label Mensagem
│   ├── Button Confirmar
│   └── Button Cancelar
```

---

## 📐 Fórmulas e Lógica

### Cálculo Automático de Bonificação

```javascript
// Na lista Colaboradores_Bonificacao (coluna calculada)
If(
    PercentualMeta >= 100,
    Switch(
        Funcao,
        "ADM de Loja", 2000 * (PercentualMeta / 100),
        "Líder de Caixa", 1500 * (PercentualMeta / 100),
        "Líder de Estoque", 1500 * (PercentualMeta / 100),
        0
    ),
    0
)

// Com bônus escalonado
If(
    PercentualMeta >= 115, BaseValue * 1.5,
    If(PercentualMeta >= 110, BaseValue * 1.3,
    If(PercentualMeta >= 105, BaseValue * 1.15,
    If(PercentualMeta >= 100, BaseValue,
    0)))
)
```

### Filtro Avançado de Registros

```javascript
// Variável de filtro complexo
Set(
    varFiltroAvancado,
    Filter(
        Aprovacoes_Bonificacao,

        // Filtro por status
        (varStatusFiltro = "Todos" || Status_Aprovacao.Value = varStatusFiltro) &&

        // Filtro por função
        (varFuncaoFiltro = "Todos" || Funcao.Value = varFuncaoFiltro) &&

        // Filtro por região
        (varRegiaoFiltro = "Todas" || Regiao.Value = varRegiaoFiltro) &&

        // Filtro por período
        (IsBlank(varPeriodoFiltro) || Mes_Ano = varPeriodoFiltro) &&

        // Filtro por meta mínima
        PercentualMeta >= varMetaMinimaFiltro &&

        // Busca por texto (nome ou loja)
        (IsBlank(varTextoBusca) ||
         varTextoBusca in Nome_Colaborador ||
         varTextoBusca in Loja)
    )
);
```

### Validação de Dados

```javascript
// Validação antes de salvar
Set(
    varErrosValidacao,
    If(IsBlank(txtNome.Text), "Nome é obrigatório; ", "") &
    If(Value(txtMeta.Text) < 0 || Value(txtMeta.Text) > 200, "Meta deve estar entre 0 e 200; ", "") &
    If(IsBlank(ddFuncao.Selected.Value), "Função é obrigatória; ", "") &
    If(IsBlank(ddRegiao.Selected.Value), "Região é obrigatória; ", "")
);

If(
    Len(varErrosValidacao) = 0,
    // Salvar dados
    Patch(...),
    // Mostrar erros
    Notify(varErrosValidacao, NotificationType.Error)
)
```

### Agregações e Estatísticas

```javascript
// Dashboard OnVisible
ClearCollect(
    colEstatisticas,
    {
        // Contadores por status
        TotalPendentes: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Pendente")),
        TotalAprovados: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado")),
        TotalReprovados: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Reprovado")),

        // Valores financeiros
        ValorTotalAprovado: Sum(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"), ValorBonificacao),
        ValorMedio: Average(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"), ValorBonificacao),
        ValorMaximo: Max(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"), ValorBonificacao),

        // Percentuais
        PercentualAprovacao: CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado")) / CountRows(Aprovacoes_Bonificacao) * 100,

        // Top performers
        TopPerformer: First(Sort(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"), PercentualMeta, Descending)).Nome_Colaborador,
        MaiorMeta: Max(Aprovacoes_Bonificacao, PercentualMeta)
    }
);

// Agrupamento por região
ClearCollect(
    colPorRegiao,
    AddColumns(
        GroupBy(
            Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"),
            "Regiao",
            "Registros"
        ),
        "Quantidade", CountRows(Registros),
        "ValorTotal", Sum(Registros, ValorBonificacao),
        "MediaMeta", Average(Registros, PercentualMeta)
    )
);
```

### Formatação de Dados

```javascript
// Formatação de moeda
Text(ValorBonificacao, "[$-pt-BR]R$ #,##0.00")

// Formatação de percentual
Text(PercentualMeta / 100, "0.0%")

// Formatação de data
Text(Data_Aprovacao, "dd/mm/yyyy HH:mm")

// Formatação de data relativa
If(
    DateDiff(Data_Aprovacao, Now(), Days) = 0, "Hoje",
    If(DateDiff(Data_Aprovacao, Now(), Days) = 1, "Ontem",
    DateDiff(Data_Aprovacao, Now(), Days) & " dias atrás")
)
```

---

## 🔒 Permissões e Segurança

### Grupos de Segurança SharePoint

**1. PMZ_Coordenadores**

- Permissão: Contribute nas listas Aprovacoes_Bonificacao
- Ações: Aprovar, reprovar, corrigir dados
- Acesso: Leitura em todas as outras listas

**2. PMZ_RH**

- Permissão: Full Control em Envios_RH
- Ações: Confirmar recebimentos, acessar todos os dados
- Acesso: Leitura em Aprovacoes_Bonificacao e Colaboradores_Bonificacao

**3. PMZ_Administradores**

- Permissão: Full Control em todas as listas
- Ações: Gerenciar configurações, executar processos, acessar logs

**4. PMZ_Colaboradores**

- Permissão: Read apenas dos próprios registros
- Filtro: Created By = Current User

### Configuração no Power Apps

```javascript
// App OnStart - Identificar papel do usuário
Set(
    varUsuarioAtual,
    {
        Email: User().Email,
        Nome: User().FullName,
        IsCoordenador: User().Email in colCoordenadores.Email,
        IsRH: User().Email in colRH.Email,
        IsAdmin: User().Email in colAdministradores.Email
    }
);

// Controle de acesso em botões
Button Aprovar:
DisplayMode: If(varUsuarioAtual.IsCoordenador, DisplayMode.Edit, DisplayMode.Disabled)

// Filtro de dados baseado em permissão
Gallery Items:
If(
    varUsuarioAtual.IsAdmin || varUsuarioAtual.IsRH,
    Aprovacoes_Bonificacao, // Ver todos
    Filter(Aprovacoes_Bonificacao, Regiao.Value = varUsuarioAtual.Regiao) // Ver apenas sua região
)
```

### Auditoria e Logs

```javascript
// Registrar ação do usuário
Patch(
    Log_Auditoria,
    Defaults(Log_Auditoria),
    {
        Usuario: User().Email,
        Acao: "Aprovação de bonificação",
        Registro_ID: ThisItem.ID,
        Data_Hora: Now(),
        Detalhes: "Aprovado registro de " & ThisItem.Nome_Colaborador
    }
);
```

---

## 🚀 Guia de Implementação Passo a Passo

### Fase 1: Preparação (1-2 dias)

**Passo 1.1: Criar Site SharePoint**

1. Acesse SharePoint Admin Center
2. Crie novo site: "PMZ Bonificação"
3. URL sugerida: /sites/pmz-bonificacao
4. Template: Team Site

**Passo 1.2: Criar Listas SharePoint**

1. Siga a estrutura de dados documentada acima
2. Crie as 5 listas principais:
   - Colaboradores_Bonificacao
   - Aprovacoes_Bonificacao
   - Historico_Execucoes
   - Envios_RH
   - Configuracoes_Sistema

3. Configure colunas conforme especificado
4. Habilite versionamento nas listas principais

**Passo 1.3: Configurar Grupos de Segurança**

1. Site Settings > Site Permissions
2. Criar grupos:
   - PMZ_Coordenadores
   - PMZ_RH
   - PMZ_Administradores
   - PMZ_Colaboradores
3. Atribuir permissões conforme documentado
4. Adicionar usuários aos grupos

**Passo 1.4: Importar Dados Iniciais**

1. Prepare planilha Excel com colaboradores ativos
2. Importe para Colaboradores_Bonificacao
3. Valide integridade dos dados

### Fase 2: Power Automate (2-3 dias)

**Passo 2.1: Criar Flow de Execução Mensal**

1. Power Automate > Criar > Scheduled Cloud Flow
2. Nome: "[PMZ] Execução Mensal Bonificação"
3. Recurrence: Monthly, Day 8, 6:00 AM
4. Adicionar ações conforme documentado
5. Testar em ambiente de desenvolvimento

**Passo 2.2: Criar Flow de Validação**

1. Criar Automated Cloud Flow
2. Trigger: "When an item is created" (Colaboradores_Bonificacao)
3. Implementar lógica de validação
4. Testar com dados de exemplo

**Passo 2.3: Criar Flow de Notificações**

1. Trigger: "When an item is created or modified" (Aprovacoes_Bonificacao)
2. Condição: Status_Aprovacao mudou
3. Implementar envio de emails
4. Personalizar templates HTML

**Passo 2.4: Criar Flow de Envio RH**

1. Manual trigger flow
2. Criar Excel com dados
3. Enviar email com anexo
4. Atualizar status

**Passo 2.5: Criar Flow de Cards**

1. Manual trigger com parâmetro ID
2. Gerar HTML do card
3. Enviar email personalizado
4. Registrar envio

### Fase 3: Power Apps (3-4 dias)

**Passo 3.1: Criar Aplicação Canvas**

1. Power Apps > Criar app > Canvas app
2. Nome: "PMZ Sistema de Bonificação"
3. Formato: Tablet (16:9) ou Phone (9:16)

**Passo 3.2: Conectar Fontes de Dados**

```
Add data:
- SharePoint (todas as 5 listas)
- Office 365 Users (para informações de usuários)
- Office 365 Outlook (para envio de emails, se necessário)
```

**Passo 3.3: Criar Tela Home**

1. Inserir componentes conforme design
2. Implementar fórmulas de KPIs
3. Adicionar navegação
4. Testar responsividade

**Passo 3.4: Criar Telas Restantes**

- Execução e Agendamento
- Interface de Aprovação
- Envio ao RH
- Reconhecimento

**Passo 3.5: Criar Componentes Reutilizáveis**

- KPI_Card
- Badge_Status
- Modal_Confirmacao

**Passo 3.6: Implementar Navegação**

```javascript
// App OnStart
Set(varTelaAtual, "Home");

// Botões de navegação
Navigate(scrAprovacao, ScreenTransition.Fade);
Set(varTelaAtual, "Aprovacao");
```

**Passo 3.7: Configurar App Settings**

- Nome de exibição
- Ícone da aplicação
- Cor de tema: #18224c
- Fundo: Branco/Cinza claro

### Fase 4: Testes (2-3 dias)

**Passo 4.1: Testes Unitários**

- Testar cada fórmula individualmente
- Validar cálculos de bonificação
- Verificar filtros e buscas
- Testar navegação

**Passo 4.2: Testes de Integração**

- Executar fluxo completo: execução → aprovação → envio RH
- Testar Flows em ambiente real
- Validar emails enviados
- Verificar dados no SharePoint

**Passo 4.3: Testes de Usuário (UAT)**

- Convidar 3-5 usuários de cada grupo
- Fornecer cenários de teste
- Coletar feedback
- Documentar bugs e melhorias

**Passo 4.4: Testes de Desempenho**

- Testar com 500+ registros
- Medir tempo de carregamento
- Otimizar queries lentas
- Implementar delegação quando possível

### Fase 5: Implantação (1 dia)

**Passo 5.1: Preparar Produção**

- Criar site SharePoint de produção
- Migrar estrutura de listas
- Configurar permissões
- Importar dados reais

**Passo 5.2: Publicar Power Apps**

1. Salvar versão final
2. Publicar aplicação
3. Compartilhar com grupos de segurança
4. Definir como "Featured App"

**Passo 5.3: Ativar Flows em Produção**

1. Exportar flows de desenvolvimento
2. Importar em produção
3. Atualizar conexões
4. Habilitar flows

**Passo 5.4: Treinamento**

- Preparar material de treinamento
- Realizar sessões com coordenadores
- Gravar vídeos tutoriais
- Criar FAQ

### Fase 6: Suporte e Monitoramento (Contínuo)

**Passo 6.1: Monitoramento**

- Configurar alertas de erro nos Flows
- Monitorar logs de execução
- Acompanhar feedback dos usuários

**Passo 6.2: Manutenção**

- Backup semanal das listas
- Revisão mensal de dados
- Atualização de documentação
- Otimização contínua

---

## ⚙️ Configurações Avançadas

### Delegação no Power Apps

**Problemas Comuns:**

```javascript
// ❌ NÃO delegável (processamento local, limite de 500 registros)
CountRows(Filter(Aprovacoes_Bonificacao, PercentualMeta > 100))

// ✅ Delegável (processado no servidor)
CountRows(Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Aprovado"))
```

**Soluções:**

1. Use colunas indexadas no SharePoint
2. Evite funções complexas em Filter
3. Use coleções para dados estáticos
4. Implemente paginação

**Configuração de Limite de Dados:**

```
App Settings > Advanced Settings > Data row limit: 2000
```

### Performance Optimization

**OnStart Otimizado:**

```javascript
Concurrent(
    // Carregar dados em paralelo
    ClearCollect(colCoord, Office365Users.SearchUser({searchTerm: "coordenador"})),
    ClearCollect(colConfig, Configuracoes_Sistema),
    Set(varUsuarioAtual, User())
);

// Cache de dados frequentes
ClearCollect(
    colAprovacoesCache,
    Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Pendente")
);

// Atualizar cache a cada 5 minutos
Set(timerRefresh, true);
```

**Timer de Refresh:**

```
Timer Control:
Duration: 300000 (5 minutos)
AutoStart: true
Repeat: true
OnTimerEnd: Refresh(Aprovacoes_Bonificacao)
```

### Notificações Push

**Configurar Microsoft Teams:**

```javascript
// No Power Automate, adicionar ação:
"Post message in a chat or channel"

Recipient: [User email from Aprovacoes]
Message:
"🎉 Parabéns {Nome_Colaborador}!
Sua bonificação de R$ {ValorBonificacao} foi aprovada.
Meta atingida: {PercentualMeta}%"
```

### Backup Automático

**Flow de Backup Semanal:**

1. Recurrence: Weekly, Sunday, 2:00 AM
2. Get items de todas as listas
3. Create CSV table
4. Create file no OneDrive/SharePoint
5. Manter últimas 4 semanas

---

## 🐛 Troubleshooting

### Erro: "Delegation Warning"

**Problema:** Fórmula não pode ser delegada, dados podem estar incompletos.

**Solução:**

```javascript
// Antes (com warning)
Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Pendente" && PercentualMeta > 100)

// Depois (sem warning)
Filter(
    Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Pendente"),
    PercentualMeta > 100
)

// Ou usar coleções
ClearCollect(colTemp, Filter(Aprovacoes_Bonificacao, Status_Aprovacao.Value = "Pendente"));
Filter(colTemp, PercentualMeta > 100)
```

### Erro: "The specified item could not be found"

**Problema:** Tentando acessar item que não existe ou foi deletado.

**Solução:**

```javascript
// Adicionar verificação
If(
    !IsBlank(LookUp(Aprovacoes_Bonificacao, ID = varID)),
    Patch(...),
    Notify("Registro não encontrado", NotificationType.Error)
)
```

### Erro: "Name isn't valid"

**Problema:** Nome de variável ou controle inválido.

**Solução:**

- Não usar espaços em nomes
- Não começar com número
- Evitar caracteres especiais
- Use camelCase ou PascalCase

### Erro: Flow não executa

**Diagnóstico:**

1. Verificar histórico de execuções
2. Checar conexões
3. Validar permissões
4. Revisar condições

**Solução:**

```
Power Automate > Flow > Run History
- Ver detalhes de erro
- Re-autenticar conexões se necessário
- Adicionar error handling (Try-Catch)
```

### Performance Lenta

**Diagnóstico:**

- Monitor > Network traffic
- Verificar quantidade de controles na tela (< 500)
- Analisar fórmulas complexas

**Soluções:**

1. Usar coleções para dados estáticos
2. Implementar lazy loading
3. Reduzir controles visuais
4. Otimizar imagens
5. Usar OnVisible em vez de OnStart quando possível

---

## 📚 Recursos Adicionais

### Templates de Email

**Email de Aprovação:**

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .container { max-width: 600px; margin: 0 auto; padding: 20px; }
        .header { background: linear-gradient(135deg, #18224c 0%, #2a3565 100%); color: white; padding: 30px; text-align: center; }
        .content { background: #ffffff; padding: 30px; border: 1px solid #e5e7eb; }
        .footer { background: #f3f4f6; padding: 20px; text-align: center; font-size: 12px; color: #6b7280; }
        .button { background: #18224c; color: white; padding: 15px 30px; text-decoration: none; display: inline-block; border-radius: 5px; }
        .badge { display: inline-block; padding: 5px 15px; border-radius: 20px; font-size: 12px; }
        .badge-success { background: #d1fae5; color: #065f46; }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎉 Bonificação Aprovada!</h1>
        </div>
        <div class="content">
            <p>Olá <strong>{Nome_Colaborador}</strong>,</p>
            <p>Parabéns! Sua bonificação foi aprovada.</p>

            <table style="width: 100%; margin: 20px 0;">
                <tr>
                    <td><strong>Meta Atingida:</strong></td>
                    <td><span class="badge badge-success">{PercentualMeta}%</span></td>
                </tr>
                <tr>
                    <td><strong>Bonificação:</strong></td>
                    <td><strong style="color: #065f46; font-size: 24px;">R$ {ValorBonificacao}</strong></td>
                </tr>
                <tr>
                    <td><strong>Previsão de Pagamento:</strong></td>
                    <td>{DataPagamento}</td>
                </tr>
            </table>

            <p>Continue com o excelente trabalho! 💪</p>

            <p style="text-align: center; margin-top: 30px;">
                <a href="#" class="button">Ver Detalhes</a>
            </p>
        </div>
        <div class="footer">
            © 2025 PMZ - Sistema de Bonificação Automática
        </div>
    </div>
</body>
</html>
```

### Fórmulas Úteis

**Verificar se é dia de execução:**

```javascript
If(
    Day(Today()) = 8 &&
    LookUp(Configuracoes_Sistema, Chave = "ExecutarAuto").Valor = "true",
    // Executar processo
    FlowExecucao.Run(),
    // Não fazer nada
    false
)
```

**Calcular dias úteis até próximo pagamento:**

```javascript
// Assumindo pagamento no dia 25 do mês
Set(
    varDiasPagamento,
    WorkdayDiff(Today(), Date(Year(Today()), Month(Today()), 25))
);
```

**Exportar para Excel:**

```javascript
// No Power Automate
Create CSV table:
From: [Array de dados]
Columns: Automatic

Create file:
File name: "Bonificacao_" & formatDateTime(utcNow(), 'yyyyMMdd') & ".csv"
File content: body('Create_CSV_table')
```

---

## 📞 Suporte e Contatos

### Equipe de Desenvolvimento

- **Tech Lead**: [Nome] - [email]
- **Developer**: [Nome] - [email]
- **BA**: [Nome] - [email]

### Documentação e Recursos

- Portal de documentação interna
- Vídeos de treinamento
- FAQ e Knowledge Base

### Atualizações

- Versão atual: 1.0.0
- Última atualização: 30/10/2025
- Próxima revisão: 30/01/2026

---

## 📊 Anexos

### Checklist de Implementação

```
□ Fase 1: Preparação
  □ Site SharePoint criado
  □ 5 listas criadas e configuradas
  □ Grupos de segurança configurados
  □ Dados iniciais importados

□ Fase 2: Power Automate
  □ Flow execução mensal
  □ Flow validação
  □ Flow notificações
  □ Flow envio RH
  □ Flow cards reconhecimento

□ Fase 3: Power Apps
  □ App criado e publicado
  □ 5 telas implementadas
  □ Componentes reutilizáveis
  □ Navegação funcionando
  □ Permissões configuradas

□ Fase 4: Testes
  □ Testes unitários concluídos
  □ Testes integração concluídos
  □ UAT aprovado
  □ Performance validada

□ Fase 5: Implantação
  □ Produção configurada
  □ App publicado
  □ Flows ativos
  □ Treinamento realizado

□ Fase 6: Pós-Implantação
  □ Monitoramento ativo
  □ Documentação entregue
  □ Suporte estabelecido
```

### Glossário

- **Canvas App**: Aplicação Power Apps com interface personalizável
- **Delegação**: Processamento de dados no servidor (não local)
- **Flow**: Automação do Power Automate
- **Gallery**: Controle de lista repetitiva no Power Apps
- **Lookup**: Função para buscar um único registro
- **Patch**: Função para criar/atualizar registros

---

**Fim da Documentação**

_Esta documentação é um guia vivo e deve ser atualizada conforme o sistema evolui._
