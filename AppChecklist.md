# Guia Completo: Checklist de Avaliação de Loja no Power Apps

## 📋 Índice
1. [Estrutura do Projeto](#estrutura-do-projeto)
2. [Configuração do SharePoint/Dataverse](#configuração-do-sharepointdataverse)
3. [Telas e Navegação](#telas-e-navegação)
4. [Códigos e Fórmulas](#códigos-e-fórmulas)
5. [Componentes Reutilizáveis](#componentes-reutilizáveis)

---

## 1. Estrutura do Projeto

### Telas Necessárias
1. **TelaLogin** - Autenticação do usuário
2. **TelaHome** - Dashboard principal
3. **TelaSelecaoChecklist** - Seleção do checklist e nome da loja
4. **TelaGrupos** - Cards com grupos de perguntas
5. **TelaTodasPerguntas** - Lista completa de perguntas
6. **TelaPergunta** - Pergunta individual com resposta
7. **TelaFinalizacao** - Resumo e salvamento
8. **TelaHistorico** - Histórico de checklists
9. **TelaDetalheHistorico** - Detalhes de um checklist específico

---

## 2. Configuração do SharePoint/Dataverse

### Lista: Perguntas
```
Nome da Lista: ChecklistPerguntas

Colunas:
- ID (Número - automático)
- Titulo (Texto - 255 caracteres)
- Grupo (Escolha: "SETOR ESTOQUE", "SETOR CAIXA", "SETOR VENDAS", "MANUTENÇÃO LOJAS")
- Peso (Número)
- Frequencia (Escolha: "DIARIO", "SEMANAL", "MENSAL")
- Responsavel (Texto)
- Ordem (Número)
```

### Lista: Respostas
```
Nome da Lista: ChecklistRespostas

Colunas:
- ID (Número - automático)
- ChecklistID (Texto - GUID)
- PerguntaID (Número - lookup para Perguntas)
- Status (Escolha: "pleno", "medio", "baixa", "ausente")
- Pontuacao (Número: 100, 75, 25, 0)
- Observacao (Texto - múltiplas linhas)
- FotoURL (Texto - URL da imagem)
- DataResposta (Data/Hora)
- Resolvido (Sim/Não)
- DataResolucao (Data/Hora)
```

### Lista: Checklists
```
Nome da Lista: Checklists

Colunas:
- ID (Número - automático)
- ChecklistID (Texto - GUID)
- NomeLoja (Texto)
- NomeAvaliador (Texto)
- DataInicio (Data/Hora)
- DataFinalizacao (Data/Hora)
- PontuacaoTotal (Número)
- PontuacaoMaxima (Número)
- Percentual (Número)
- Status (Escolha: "Em Andamento", "Finalizado")
```

### Dados das Perguntas (para importar)
```csv
Titulo,Grupo,Peso,Frequencia,Responsavel,Ordem
"Rota - Tem rota acumulada na loja sem conferencia acima de 2 dias","SETOR ESTOQUE",40,"DIARIO","ADM",1
"Sobra de Rota - Tem sobra de rota inserida no aplicativo e pendente acima de 3 dias sem resolução","SETOR ESTOQUE",20,"DIARIO","ADM",2
"Conferência de Rota - A conferência de rota está sendo feita com assertividade acima de 98%","SETOR ESTOQUE",40,"DIARIO","ADM",3
"Acuracidade - A loja está garantindo a separação dos itens de vendas acima de 99,5%","SETOR ESTOQUE",40,"DIARIO","ADM",4
"Itens sem endereço - Estão garantindo que o estoque não tenha peças sem endereço","SETOR ESTOQUE",40,"DIARIO","ADM",5
"Itens com mais de um endereço - Está sendo feita diário a correção dos itens com mais de uma locação","SETOR ESTOQUE",10,"DIARIO","ADM",6
"Sucata de bateria - As baterias de sucatas está sendo enviada para o cd","SETOR ESTOQUE",10,"DIARIO","ADM",7
"Avaria - Tem peças avariadas na lojas pendente de solução acima de 10 dias","SETOR ESTOQUE",20,"SEMANAL","ADM",8
"Garantia - Tem garantia pendente de envio para o cd acima de 2 dias","SETOR ESTOQUE",20,"DIARIO","ADM",9
"Limpeza e organização - A area de rota, prateleira e banheiro estão limpo e organizado","SETOR ESTOQUE",30,"DIARIO","ADM",10
"Equipamento celulares e scanners - Os equipamentos estão em perfeitas condições de uso","SETOR ESTOQUE",40,"DIARIO","ADM",11
"Tempo de conferencia - Os caixas da loja está conferindo abaixo 3:50 minutos","SETOR CAIXA",20,"DIARIO","ADM",12
"Entrega pendentes - Todas as mercadorias de transferencia estão conferidas e prontas","SETOR CAIXA",30,"DIARIO","ADM",13
"Motoboy - O indice de motoboy está completo, utilizando baú, uniformizados","SETOR CAIXA",30,"DIARIO","ADM",14
"Documentos - Está sendo conferido o caixa no mesmo dia","SETOR CAIXA",10,"DIARIO","ADM",15
"Equipamento informática - Todos computadores estão funcionando","SETOR CAIXA",40,"DIARIO","ADM",16
"Limpeza e organização - A area de caixa está limpa e organizada","SETOR CAIXA",20,"DIARIO","ADM",17
"EPI de segurança - Todos os caixas e estoquistas estão utilizando Bota","SETOR CAIXA",10,"DIARIO","ADM",18
"Mostruario - O showroom está limpo, com variedade de peças, precificados","SETOR VENDAS",25,"DIARIO","GERENTE/ADM",19
"Limpeza e organização - A area interna do balcão está limpo e organizado","SETOR VENDAS",20,"DIARIO","GERENTE/ADM",20
"Maquina de cartão - Possui alguma maquina danificada ou com problema","SETOR VENDAS",20,"SEMANAL","GERENTE/ADM",21
"Equipamento informática - Todos computadores estão funcionando","SETOR VENDAS",40,"SEMANAL","GERENTE/ADM",22
"Atendimento telefone - Atender no maximo em 3 chamadas","SETOR VENDAS",40,"SEMANAL","GERENTE/ADM",23
"Atendimento padrão 10 passos - Os vendedores estão fazendo os dez passos","SETOR VENDAS",40,"SEMANAL","GERENTE/ADM",24
"Atendimento vizinhança - Os vendedores estão fazendo os dez passos","SETOR VENDAS",40,"SEMANAL","GERENTE/ADM",25
"Ofertas vigentes - Os vendedores sabem quais são os produtos em oferta","SETOR VENDAS",20,"SEMANAL","GERENTE/ADM",26
"Indicação para compra no concorrente - Onde o vendedor indica para comprar","SETOR VENDAS",20,"SEMANAL","GERENTE/ADM",27
"Aparência visual - Todos estão de acordo com o código de vestimenta","SETOR VENDAS",20,"SEMANAL","GERENTE/ADM",28
"Índice de colaboradores - O índice está completo","SETOR VENDAS",30,"SEMANAL","GERENTE/ADM",29
"Universidade PMZ - Todos os colaboradores estão acima de 70%","SETOR VENDAS",40,"SEMANAL","GERENTE/ADM",30
"Hidráulica - Existe algum vazamento em torneiras, pias, bebedouros","MANUTENÇÃO LOJAS",40,"MENSAL","GERENTE/ADM",31
"Equipamento celulares e scanners - Os equipamentos estão em perfeitas condições","MANUTENÇÃO LOJAS",40,"MENSAL","GERENTE/ADM",32
"Expositores - Os expositores estão em perfeita condições de uso","MANUTENÇÃO LOJAS",25,"MENSAL","GERENTE/ADM",33
"Painel - O adesivo e a estrutura do painel está em perfeito estado","MANUTENÇÃO LOJAS",25,"MENSAL","GERENTE/ADM",34
"Lampadas e Sensores - A loja possui lâmpadas/sensores queimados","MANUTENÇÃO LOJAS",25,"SEMANAL","GERENTE/ADM",35
"Prateleiras e corredores - A loja possui algum corredor danificado","MANUTENÇÃO LOJAS",25,"SEMANAL","GERENTE/ADM",36
```

---

## 3. Telas e Navegação

### TelaLogin

**Propriedades da Tela:**
```
Fill: RGBA(24, 36, 76, 1)
```

**Elementos:**
- Logo da empresa (Imagem)
- Campo de Email (TextInput)
- Campo de Senha (TextInput com Mode: Password)
- Botão Entrar

**Código do Botão Entrar:**
```
OnSelect:
If(
    !IsBlank(txtEmail.Text) && !IsBlank(txtSenha.Text),
    Set(varUsuarioLogado, txtEmail.Text);
    Navigate(TelaHome, ScreenTransition.Fade),
    Notify("Preencha todos os campos", NotificationType.Error)
)
```

---

### TelaHome

**Variáveis Globais (App.OnStart):**
```
// Inicializar variáveis globais
Set(varUsuarioLogado, "");
Set(varChecklistAtual, GUID());
Set(varNomeLoja, "");
Set(varGrupoAtual, "");
Set(varPerguntaAtualIndex, 1);

// Coleção de respostas temporárias
ClearCollect(colRespostasTemp, Blank());

// Cores do tema
Set(corPrimaria, RGBA(24, 36, 76, 1));
Set(corSecundaria, RGBA(34, 46, 86, 1));
Set(corAcento, RGBA(59, 130, 246, 1));
Set(corSucesso, RGBA(34, 197, 94, 1));
Set(corAlerta, RGBA(234, 179, 8, 1));
Set(corErro, RGBA(239, 68, 68, 1));
Set(corTexto, RGBA(255, 255, 255, 1));
```

**Elementos:**
- Saudação ao usuário
- Card "Iniciar Checklist" 
- Card "Ver Histórico"
- Estatísticas resumidas

**Código Card Iniciar:**
```
OnSelect:
Set(varChecklistAtual, GUID());
ClearCollect(colRespostasTemp, Blank());
Navigate(TelaSelecaoChecklist, ScreenTransition.Fade)
```

**Código Card Histórico:**
```
OnSelect:
Navigate(TelaHistorico, ScreenTransition.Fade)
```

---

### TelaSelecaoChecklist

**Elementos:**
- Campo Nome da Loja (TextInput)
- Gallery com cards dos grupos

**Gallery de Grupos:**
```
Items:
Distinct(ChecklistPerguntas, Grupo)

// Template do Card
Text: ThisItem.Result
OnSelect: 
Set(varNomeLoja, txtNomeLoja.Text);
Set(varGrupoAtual, ThisItem.Result);
Set(varPerguntaAtualIndex, 1);
Navigate(TelaGrupos, ScreenTransition.Fade)
```

**Estatísticas por Grupo:**
```
// Total de perguntas no grupo
CountRows(Filter(ChecklistPerguntas, Grupo = ThisItem.Result))

// Peso total do grupo
Sum(Filter(ChecklistPerguntas, Grupo = ThisItem.Result), Peso)
```

---

### TelaGrupos

**Gallery de Cards de Grupos:**
```
Items:
["SETOR ESTOQUE", "SETOR CAIXA", "SETOR VENDAS", "MANUTENÇÃO LOJAS"]

// Ícone por grupo
Switch(
    ThisItem.Value,
    "SETOR ESTOQUE", Icon.BoxOpen,
    "SETOR CAIXA", Icon.Money,
    "SETOR VENDAS", Icon.People,
    "MANUTENÇÃO LOJAS", Icon.Settings,
    Icon.Document
)

// Contagem de perguntas
CountRows(Filter(ChecklistPerguntas, Grupo = ThisItem.Value))

// Progresso do grupo
With(
    {
        total: CountRows(Filter(ChecklistPerguntas, Grupo = ThisItem.Value)),
        respondidas: CountRows(Filter(colRespostasTemp, Grupo = ThisItem.Value && !IsBlank(Status)))
    },
    If(total > 0, respondidas / total, 0)
)
```

**OnSelect do Card:**
```
Set(varGrupoAtual, ThisItem.Value);
Set(varPerguntaAtualIndex, 1);
Navigate(TelaPergunta, ScreenTransition.Fade)
```

**Botão Ver Todas as Perguntas:**
```
OnSelect:
Navigate(TelaTodasPerguntas, ScreenTransition.Fade)
```

---

### TelaTodasPerguntas

**Gallery de Perguntas:**
```
Items:
SortByColumns(
    Search(ChecklistPerguntas, txtBusca.Text, "Titulo"),
    "Ordem", Ascending
)

// Template mostra:
// - Número da pergunta
// - Título
// - Grupo (badge colorido)
// - Status se já respondido
```

**Badge de Status:**
```
// Cor do badge baseado no status
With(
    {
        resposta: LookUp(colRespostasTemp, PerguntaID = ThisItem.ID)
    },
    Switch(
        resposta.Status,
        "pleno", corSucesso,
        "medio", corAcento,
        "baixa", corAlerta,
        "ausente", corErro,
        RGBA(100, 100, 100, 1)
    )
)

// Texto do badge
With(
    {
        resposta: LookUp(colRespostasTemp, PerguntaID = ThisItem.ID)
    },
    Switch(
        resposta.Status,
        "pleno", "PLENO 100%",
        "medio", "MÉDIO 75%",
        "baixa", "BAIXA 25%",
        "ausente", "AUSENTE 0%",
        "Pendente"
    )
)
```

**OnSelect da Pergunta:**
```
Set(varGrupoAtual, ThisItem.Grupo);
Set(varPerguntaAtualIndex, 
    CountRows(
        Filter(ChecklistPerguntas, 
            Grupo = ThisItem.Grupo && 
            Ordem <= ThisItem.Ordem
        )
    )
);
Navigate(TelaPergunta, ScreenTransition.Fade)
```

---

### TelaPergunta

**Variáveis Locais (OnVisible):**
```
Set(varPerguntasDoGrupo, 
    SortByColumns(
        Filter(ChecklistPerguntas, Grupo = varGrupoAtual),
        "Ordem", Ascending
    )
);
Set(varPerguntaAtual, 
    Index(varPerguntasDoGrupo, varPerguntaAtualIndex)
);
Set(varRespostaAtual, 
    LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)
);
```

**Barra de Progresso:**
```
// Largura proporcional ao progresso
Width: (varPerguntaAtualIndex / CountRows(varPerguntasDoGrupo)) * Parent.Width
```

**Texto do Progresso:**
```
varPerguntaAtualIndex & " / " & CountRows(varPerguntasDoGrupo)
```

**Exibição da Pergunta:**
```
// Número e peso
"#" & varPerguntaAtual.Ordem & " • Peso: " & varPerguntaAtual.Peso

// Título da pergunta
varPerguntaAtual.Titulo

// Frequência e Responsável
varPerguntaAtual.Frequencia & " • " & varPerguntaAtual.Responsavel
```

**Botões de Status (4 botões):**

```
// Botão PLENO (100%)
Fill: If(varRespostaAtual.Status = "pleno", corSucesso, corSecundaria)
OnSelect:
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {Status: "pleno", Pontuacao: 100}
);
If(
    CountRows(Filter(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)) = 0,
    Collect(colRespostasTemp, 
        {
            PerguntaID: varPerguntaAtual.ID,
            Grupo: varGrupoAtual,
            Status: "pleno",
            Pontuacao: 100,
            Peso: varPerguntaAtual.Peso,
            Observacao: "",
            FotoURL: ""
        }
    )
);
Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID))

// Botão MÉDIO (75%)
Fill: If(varRespostaAtual.Status = "medio", corAcento, corSecundaria)
OnSelect:
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {Status: "medio", Pontuacao: 75}
);
If(
    CountRows(Filter(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)) = 0,
    Collect(colRespostasTemp, 
        {
            PerguntaID: varPerguntaAtual.ID,
            Grupo: varGrupoAtual,
            Status: "medio",
            Pontuacao: 75,
            Peso: varPerguntaAtual.Peso,
            Observacao: "",
            FotoURL: ""
        }
    )
);
Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID))

// Botão BAIXA (25%)
Fill: If(varRespostaAtual.Status = "baixa", corAlerta, corSecundaria)
OnSelect:
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {Status: "baixa", Pontuacao: 25}
);
If(
    CountRows(Filter(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)) = 0,
    Collect(colRespostasTemp, 
        {
            PerguntaID: varPerguntaAtual.ID,
            Grupo: varGrupoAtual,
            Status: "baixa",
            Pontuacao: 25,
            Peso: varPerguntaAtual.Peso,
            Observacao: "",
            FotoURL: ""
        }
    )
);
Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID))

// Botão AUSENTE (0%)
Fill: If(varRespostaAtual.Status = "ausente", corErro, corSecundaria)
OnSelect:
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {Status: "ausente", Pontuacao: 0}
);
If(
    CountRows(Filter(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)) = 0,
    Collect(colRespostasTemp, 
        {
            PerguntaID: varPerguntaAtual.ID,
            Grupo: varGrupoAtual,
            Status: "ausente",
            Pontuacao: 0,
            Peso: varPerguntaAtual.Peso,
            Observacao: "",
            FotoURL: ""
        }
    )
);
Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID))
```

**Campo de Observação:**
```
Default: varRespostaAtual.Observacao
OnChange:
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {Observacao: Self.Text}
)
```

**Botão Adicionar Foto:**
```
OnSelect:
// Usar controle de câmera ou upload
Set(varMostrarCamera, true)
```

**Controle de Câmera:**
```
// Adicionar Camera control
OnSelect (do botão capturar):
Set(varFotoTemp, Camera1.Photo);
UpdateIf(
    colRespostasTemp,
    PerguntaID = varPerguntaAtual.ID,
    {FotoURL: varFotoTemp}
);
Set(varMostrarCamera, false)
```

**Navegação:**
```
// Botão Voltar
OnSelect:
If(
    varPerguntaAtualIndex > 1,
    Set(varPerguntaAtualIndex, varPerguntaAtualIndex - 1);
    Set(varPerguntaAtual, Index(varPerguntasDoGrupo, varPerguntaAtualIndex));
    Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)),
    Navigate(TelaGrupos, ScreenTransition.Fade)
)

// Botão Próximo
OnSelect:
If(
    varPerguntaAtualIndex < CountRows(varPerguntasDoGrupo),
    Set(varPerguntaAtualIndex, varPerguntaAtualIndex + 1);
    Set(varPerguntaAtual, Index(varPerguntasDoGrupo, varPerguntaAtualIndex));
    Set(varRespostaAtual, LookUp(colRespostasTemp, PerguntaID = varPerguntaAtual.ID)),
    Navigate(TelaGrupos, ScreenTransition.Fade)
)
```

---

### TelaFinalizacao

**Cálculos de Resumo:**
```
// Total de perguntas respondidas
CountRows(Filter(colRespostasTemp, !IsBlank(Status)))

// Pontuação obtida (ponderada)
Sum(
    colRespostasTemp,
    (Pontuacao / 100) * Peso
)

// Pontuação máxima possível
Sum(ChecklistPerguntas, Peso)

// Percentual geral
With(
    {
        obtido: Sum(colRespostasTemp, (Pontuacao / 100) * Peso),
        maximo: Sum(ChecklistPerguntas, Peso)
    },
    If(maximo > 0, Round((obtido / maximo) * 100, 1), 0)
)
```

**Resumo por Grupo:**
```
// Gallery com grupos e suas pontuações
Items: ["SETOR ESTOQUE", "SETOR CAIXA", "SETOR VENDAS", "MANUTENÇÃO LOJAS"]

// Pontuação do grupo
With(
    {
        respostasGrupo: Filter(colRespostasTemp, Grupo = ThisItem.Value),
        perguntasGrupo: Filter(ChecklistPerguntas, Grupo = ThisItem.Value)
    },
    With(
        {
            obtido: Sum(respostasGrupo, (Pontuacao / 100) * Peso),
            maximo: Sum(perguntasGrupo, Peso)
        },
        If(maximo > 0, Round((obtido / maximo) * 100, 1), 0) & "%"
    )
)
```

**Lista de Problemas:**
```
// Perguntas com status baixa ou ausente
Items:
AddColumns(
    Filter(colRespostasTemp, Status = "baixa" || Status = "ausente"),
    "Titulo", LookUp(ChecklistPerguntas, ID = PerguntaID).Titulo
)
```

**Campo Nome do Avaliador:**
```
txtAvaliador (TextInput)
```

**Botão Salvar Checklist:**
```
OnSelect:
// Salvar checklist principal
Patch(
    Checklists,
    Defaults(Checklists),
    {
        ChecklistID: Text(varChecklistAtual),
        NomeLoja: varNomeLoja,
        NomeAvaliador: txtAvaliador.Text,
        DataInicio: Now(),
        DataFinalizacao: Now(),
        PontuacaoTotal: Sum(colRespostasTemp, (Pontuacao / 100) * Peso),
        PontuacaoMaxima: Sum(ChecklistPerguntas, Peso),
        Percentual: With(
            {
                obtido: Sum(colRespostasTemp, (Pontuacao / 100) * Peso),
                maximo: Sum(ChecklistPerguntas, Peso)
            },
            If(maximo > 0, Round((obtido / maximo) * 100, 1), 0)
        ),
        Status: "Finalizado"
    }
);

// Salvar todas as respostas
ForAll(
    colRespostasTemp,
    Patch(
        ChecklistRespostas,
        Defaults(ChecklistRespostas),
        {
            ChecklistID: Text(varChecklistAtual),
            PerguntaID: ThisRecord.PerguntaID,
            Status: ThisRecord.Status,
            Pontuacao: ThisRecord.Pontuacao,
            Observacao: ThisRecord.Observacao,
            FotoURL: ThisRecord.FotoURL,
            DataResposta: Now(),
            Resolvido: false
        }
    )
);

Notify("Checklist salvo com sucesso!", NotificationType.Success);
Navigate(TelaHome, ScreenTransition.Fade)
```

---

### TelaHistorico

**Gallery de Checklists:**
```
Items:
SortByColumns(
    Search(Checklists, txtBuscaHistorico.Text, "NomeLoja", "NomeAvaliador"),
    "DataFinalizacao", Descending
)

// Template mostra:
// - Nome da loja
// - Data
// - Percentual (com cor baseada no valor)
// - Nome do avaliador
```

**Cor do Percentual:**
```
If(
    ThisItem.Percentual >= 90, corSucesso,
    ThisItem.Percentual >= 70, corAcento,
    ThisItem.Percentual >= 50, corAlerta,
    corErro
)
```

**OnSelect do Item:**
```
Set(varChecklistSelecionado, ThisItem);
Navigate(TelaDetalheHistorico, ScreenTransition.Fade)
```

---

### TelaDetalheHistorico

**Informações do Checklist:**
```
// Cabeçalho
varChecklistSelecionado.NomeLoja
Text(varChecklistSelecionado.DataFinalizacao, "dd/mm/yyyy hh:mm")
varChecklistSelecionado.NomeAvaliador
varChecklistSelecionado.Percentual & "%"
```

**Gallery de Respostas:**
```
Items:
AddColumns(
    SortByColumns(
        Filter(ChecklistRespostas, ChecklistID = varChecklistSelecionado.ChecklistID),
        "PerguntaID", Ascending
    ),
    "Pergunta", LookUp(ChecklistPerguntas, ID = PerguntaID)
)
```

**Lista de Problemas Pendentes:**
```
Items:
Filter(
    AddColumns(
        Filter(ChecklistRespostas, 
            ChecklistID = varChecklistSelecionado.ChecklistID && 
            (Status = "baixa" || Status = "ausente") &&
            !Resolvido
        ),
        "Pergunta", LookUp(ChecklistPerguntas, ID = PerguntaID)
    ),
    true
)
```

**Botão Marcar como Resolvido:**
```
OnSelect:
Patch(
    ChecklistRespostas,
    ThisItem,
    {
        Resolvido: true,
        DataResolucao: Now()
    }
);
Notify("Problema marcado como resolvido!", NotificationType.Success)
```

---

## 4. Componentes Reutilizáveis

### Componente: BotaoStatus

**Propriedades Customizadas:**
- StatusValue (Text): "pleno", "medio", "baixa", "ausente"
- StatusSelecionado (Text): status atual selecionado
- Pontuacao (Number): valor da pontuação
- Label (Text): texto do botão

**Código:**
```
// Fill
If(StatusSelecionado = StatusValue,
    Switch(StatusValue,
        "pleno", corSucesso,
        "medio", corAcento,
        "baixa", corAlerta,
        "ausente", corErro
    ),
    corSecundaria
)

// Border
If(StatusSelecionado = StatusValue, 2, 0)
```

### Componente: CardGrupo

**Propriedades Customizadas:**
- NomeGrupo (Text)
- Icone (Icon)
- TotalPerguntas (Number)
- PerguntasRespondidas (Number)
- PontuacaoGrupo (Number)

**Código da Barra de Progresso:**
```
Width: If(TotalPerguntas > 0, 
    (PerguntasRespondidas / TotalPerguntas) * Parent.Width, 
    0
)
```

### Componente: BadgeStatus

**Propriedades:**
- Status (Text)

**Código:**
```
// Cor de fundo
Switch(Status,
    "pleno", RGBA(34, 197, 94, 0.2),
    "medio", RGBA(59, 130, 246, 0.2),
    "baixa", RGBA(234, 179, 8, 0.2),
    "ausente", RGBA(239, 68, 68, 0.2),
    RGBA(100, 100, 100, 0.2)
)

// Cor do texto
Switch(Status,
    "pleno", RGBA(34, 197, 94, 1),
    "medio", RGBA(59, 130, 246, 1),
    "baixa", RGBA(234, 179, 8, 1),
    "ausente", RGBA(239, 68, 68, 1),
    RGBA(200, 200, 200, 1)
)

// Texto
Switch(Status,
    "pleno", "PLENO 100%",
    "medio", "MÉDIO 75%",
    "baixa", "BAIXA 25%",
    "ausente", "AUSENTE 0%",
    "Pendente"
)
```

---

## 5. Dicas de Implementação

### Performance
1. Use `Concurrent()` para operações paralelas
2. Limite os dados carregados com `FirstN()` e `Filter()`
3. Use delegação quando possível (SharePoint suporta até 2000 itens por padrão)

### UX Mobile
1. Defina `App.Width = 640` e `App.Height = 1136` para layout mobile
2. Use `App.ActiveScreen.Width` para responsividade
3. Adicione feedback visual em todas as ações

### Offline
1. Use `SaveData()` e `LoadData()` para cache local
2. Sincronize quando online com `Connection.Connected`

```
// Salvar offline
SaveData(colRespostasTemp, "RespostasOffline")

// Carregar ao iniciar
If(
    !IsEmpty(LoadData("RespostasOffline", true)),
    ClearCollect(colRespostasTemp, LoadData("RespostasOffline", true))
)

// Sincronizar quando online
If(
    Connection.Connected,
    ForAll(
        colRespostasTemp,
        Patch(ChecklistRespostas, Defaults(ChecklistRespostas), ThisRecord)
    );
    ClearCollect(colRespostasTemp, Blank())
)
```

---

## 6. Temas e Cores

```
// Definir no App.OnStart
Set(Tema, {
    Primaria: RGBA(24, 36, 76, 1),
    Secundaria: RGBA(34, 46, 86, 1),
    Terciaria: RGBA(44, 56, 96, 1),
    Acento: RGBA(59, 130, 246, 1),
    Sucesso: RGBA(34, 197, 94, 1),
    Alerta: RGBA(234, 179, 8, 1),
    Erro: RGBA(239, 68, 68, 1),
    TextoPrimario: RGBA(255, 255, 255, 1),
    TextoSecundario: RGBA(156, 163, 175, 1),
    Borda: RGBA(55, 65, 81, 1)
})
```

---

## 7. Publicação

1. **Teste no dispositivo**: Use o app Power Apps Mobile
2. **Compartilhe**: Compartilhe com o grupo de segurança adequado
3. **Defina permissões**: Configure as permissões das listas SharePoint

---

## Suporte

Para dúvidas sobre implementação, consulte:
- [Documentação Power Apps](https://docs.microsoft.com/pt-br/powerapps/)
- [Power Apps Community](https://powerusers.microsoft.com/t5/Power-Apps-Community/ct-p/PowerApps1)
