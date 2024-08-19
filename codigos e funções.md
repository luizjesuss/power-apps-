
1. **Fórmula para obter o nome de usuário do Office 365:**
   - `User().FullName`

2. **Fórmula para obter o endereço de e-mail do usuário do Office 365:**
   - `User().Email`

3. **Fórmula para exibir um controle somente se o usuário tiver uma função específica:**
   - `If(User().IsMemberOf("Admins"); true; false)`

4. **Fórmula para obter a localização do dispositivo:**
   - `Location.Latitude & "; " & Location.Longitude`

5. **Fórmula para obter a hora do dispositivo:**
   - `Text(DateTimeValue(Text(Now())); DateTimeFormat.LongTime)`

6. **Fórmula para converter uma data em um formato específico:**
   - `Text(Today(); "dd/mm/yyyy")`

7. **Fórmula para criar uma nova linha em uma tabela do SharePoint:**
   - `Collect(TabelaSharePoint; {Coluna1: "Valor1"; Coluna2: "Valor2"})`

8. **Fórmula para atualizar uma linha em uma tabela do SharePoint:**
   - `Patch(TabelaSharePoint; LookUp(TabelaSharePoint; ID = 1); {Coluna1: "NovoValor"})`

9. **Fórmula para excluir uma linha de uma tabela do SharePoint:**
   - `Remove(TabelaSharePoint; LookUp(TabelaSharePoint; ID = 1))`

10. **Fórmula para ordenar itens de uma galeria por uma coluna específica:**
    - `SortByColumns(NomeDaGaleria; "NomeDaColuna"; SortOrder.Descending)`

11. **Fórmula para filtrar itens de uma galeria por uma coluna específica:**
    - `Filter(NomeDaGaleria; NomeDaColuna = "ValorDesejado")`

12. **Fórmula para filtrar itens de uma galeria por vários valores em uma coluna específica:**
    - `Filter(NomeDaGaleria; Or(NomeDaColuna = "Valor1"; NomeDaColuna = "Valor2"; NomeDaColuna = "Valor3"))`

13. **Fórmula para filtrar itens de uma galeria por uma data específica:**
    - `Filter(NomeDaGaleria; Date(NomeDaColuna) = DateValorDesejado)`

14. **Fórmula para filtrar itens de uma galeria por uma data dentro de um intervalo:**
    - `Filter(NomeDaGaleria; DateValue(NomeDaColuna) >= DateValorInicial && DateValue(NomeDaColuna) <= DateValorFinal)`

15. **Fórmula para somar os valores de uma coluna em uma galeria:**
    - `Sum(NomeDaGaleria; NomeDaColuna)`

16. **Fórmula para contar os itens de uma galeria:**
    - `CountRows(NomeDaGaleria)`

17. **Fórmula para obter o índice de um item em uma galeria:**
    - `Gallery.AllItems.IndexOf(NomeDoItem)`

18. **Fórmula para alterar a cor de fundo de um item na galeria com base em uma condição:**
    - `If(NomeDaColuna = "ValorDesejado"; RGBA(255; 255; 0; 1); RGBA(255; 255; 255; 1))`

19. **Fórmula para ocultar um item na galeria com base em uma condição:**
    - `If(NomeDaColuna = "ValorDesejado"; false; true)`

20. **Fórmula para obter o valor do item selecionado em uma galeria:**
    - `NomeDaGaleria.Selected.NomeDaColuna`

21. **Fórmula para obter o valor de uma coluna de um item específico em uma galeria:**
    - `LookUp(NomeDaGaleria; ID = ValorDesejado; NomeDaColuna)`

22. **Fórmula para obter a quantidade de caracteres em uma coluna de uma galeria:**
    - `Len(NomeDaGaleria.NomeDaColuna)`

23. **Fórmula para adicionar um valor a uma coluna de uma galeria:**
    - `Patch(NomeDaGaleria; Defaults(NomeDaGaleria); {NomeDaColuna: "ValorDesejado"})`

24. **Fórmula para remover um valor de uma coluna de uma galeria:**
    - `RemoveIf(NomeDaGaleria; ID = ValorDesejado)`

25. **Fórmula para atualizar um valor em uma coluna de uma galeria:**
    - `UpdateIf(NomeDaGaleria; ID = ValorDesejado; {NomeDaColuna: "NovoValor"})`

26. **Fórmula para filtrar uma galeria com base em uma entrada de texto:**
    - `Filter(Gallery1.AllItems; TextSearchBox.Text in Nome)`

27. **Fórmula para exibir uma mensagem de confirmação:**
    - `Confirm("Tem certeza que deseja continuar?"; "Confirmação")`

28. **Fórmula para adicionar um novo registro a uma tabela:**
    - `Patch(Tabela; Defaults(Tabela); {Coluna1: Valor1; Coluna2: Valor2})`

29. **Fórmula para exibir uma mensagem de erro:**
    - `Error("Não foi possível concluir a operação.")`

30. **Fórmula para calcular a soma de uma coluna em uma tabela:**
    - `Sum(Tabela; Coluna)`

31. **Fórmula para criar uma nova tabela:**
    - `ClearCollect(NovaTabela; {Coluna1: Valor1; Coluna2: Valor2})`

32. **Fórmula para filtrar uma tabela com base em uma entrada de texto:**
    - `Filter(Tabela; TextSearchBox.Text in Nome)`

33. **Fórmula para exibir uma caixa de diálogo de entrada de texto:**
    - `Set(NovoNome; TextInput1.Text); Notify("O novo nome é " & NovoNome)`

34. **Fórmula para classificar uma tabela em ordem crescente:**
    - `Sort(Tabela; Coluna; Ascending)`

35. **Fórmula para obter a data atual:**
    - `Today()`

36. **Fórmula para criar um botão de voltar para a página anterior:**
    - `Back()`

37. **Fórmula para atualizar uma tabela com base em uma entrada de texto:**
    - `Patch(Tabela; Lookup(Tabela; ID = Selected.ID); {Coluna: TextInput1.Text})`

38. **Fórmula para exibir uma mensagem de êxito:**
    - `Notify("A operação foi concluída com sucesso.")`

39. **Fórmula para classificar uma tabela em ordem decrescente:**
    - `Sort(Tabela; Coluna; Descending)`

40. **Fórmula para exibir uma mensagem de informação:**
    - `Notify("Esta é uma mensagem de informação.")`

41. **Fórmula para filtrar uma tabela com base em vários critérios:**
    - `Filter(Tabela; Coluna1 = Valor1 && Coluna2 = Valor2)`

42. **Fórmula para exibir uma mensagem de aviso:**
    - `Notify("Esta é uma mensagem de aviso."; Warning)`

43. **Fórmula para contar o número de registros em uma tabela:**
    - `CountRows(Tabela)`

44. **Fórmula para atualizar uma galeria com base em uma entrada de texto:**
    - `Patch(Gallery1.Selected; {Coluna: TextInput1.Text})`

45. **Fórmula para exibir uma caixa de diálogo de confirmação:**
    - `If(Confirm("Tem certeza que deseja continuar?"; "Confirmação"); Patch(Tabela; Selected.ID; {Coluna: Valor}))`

46. **Fórmula para filtrar uma tabela com base em uma data:**
    - `Filter(Tabela; Date = DatePicker1.SelectedDate)`

47. **Fórmula para obter a hora atual:**
    - `Now()`

48. **Fórmula para calcular a média de uma coluna em uma tabela:**
    - `Average(Tabela; Coluna)`

49. **Fórmula para calcular a soma de vários valores:**
    - `Sum(Valor1; Valor2; Valor3)`

50. **Fórmula para calcular o valor máximo de uma coluna em uma tabela:**
    - `Max(Tabela; Coluna)`

51. **Fórmula para contar o número de registros em uma galeria:**
    - `CountRows(Gallery1.AllItems)`

52. **Fórmula para obter o número máximo de uma série de valores:**
    - `Max(Valor1; Valor2; Valor3)`

53. **Fórmula para obter o número mínimo de uma série de valores:**
    - `Min(Valor1; Valor2; Valor3)`

54. **Fórmula para verificar se uma entrada de texto é um número:**
    - `IsNumeric(TextInput1.Text)`

55. **Fórmula para arredondar um número para um número específico de casas decimais:**
    - `Round(Valor; CasasDecimais)`

56. **Fórmula para converter uma data em um formato de texto personalizado:**
    - `Text(Date

Picker1.SelectedDate; "dd/mm/yyyy")`

57. **Fórmula para calcular a diferença entre duas datas:**
    - `DateDiff(DataInicial; DataFinal)`

58. **Fórmula para adicionar um valor a uma tabela:**
    - `Patch(Tabela; Defaults(Tabela); {Coluna1: Valor1; Coluna2: Valor2})`

59. **Fórmula para remover um valor de uma tabela:**
    - `Remove(Tabela; LookUp(Tabela; ID = ValorDesejado))`

60. **Fórmula para exibir uma mensagem de erro personalizada:**
    - `Notify("Erro: Ocorreu um problema com a operação."; NotificationType.Error)`
Aqui estão as fórmulas restantes formatadas em texto:

61. **Concatenar uma série de valores em uma única cadeia de caracteres:**
    - `Concat(TabelaDeValores; ColunaDeValores; ", ")`

62. **Definir a largura de um controle de galeria:**
    - `Gallery1.Width = Parent.Width`

63. **Obter a seleção atual de uma galeria:**
    - `Gallery1.Selected`

64. **Definir um controle de galeria como a seleção atual:**
    - `Gallery1.Selected = ThisItem`

65. **Atualizar uma fonte de dados:**
    - `Refresh(DataSource)`

66. **Definir o valor de uma propriedade de um controle de galeria:**
    - `Gallery1.TemplateFill = RGBA(255; 255; 255; 0)`

67. **Converter uma cadeia de caracteres em uma data:**
    - `DateValue(TextoDaData)`

68. **Calcular a diferença entre duas datas:**
    - `DateDiff(Data1; Data2; Duração)`

69. **Definir o formato de exibição de uma data:**
    - `Text(Data; "dd/mm/yyyy")`

70. **Arredondar um valor para um número de casas decimais específico:**
    - `Round(Valor; NumeroDeCasasDecimais)`

71. **Obter a versão atual do aplicativo:**
    - `Version()`

72. **Definir o valor de uma propriedade de uma tabela:**
    - `UpdateContext({TabelaDeValores: Table({Column1: Valor1; Column2: Valor2})})`

73. **Obter o valor de uma propriedade de uma tabela:**
    - `TabelaDeValores.ColumnName`

74. **Definir o valor de uma propriedade de um controle de formulário baseado em um valor selecionado em uma galeria:**
    - `Dropdown1.Default = Gallery1.Selected.Column1`

75. **Definir o valor de uma propriedade de um controle de galeria baseado em um valor selecionado em outro controle de galeria:**
    - `Gallery2.Default = Gallery1.Selected`

76. **Definir a altura de um controle de galeria baseado no número de itens na galeria:**
    - `Gallery1.Height = CountRows(TabelaDeDados) * AlturaDoItem`

77. **Ordenar uma tabela baseada em uma coluna específica:**
    - `Sort(TabelaDeDados; NomeDaColuna; Ascending/Descending)`

78. **Filtrar uma tabela baseada em uma condição específica:**
    - `Filter(TabelaDeDados; Condição)`

79. **Adicionar um novo registro a uma fonte de dados:**
    - `Collect(TabelaDeDados; {Column1: Valor1; Column2: Valor2})`

80. **Atualizar um registro existente em uma fonte de dados:**
    - `Patch(TabelaDeDados; {ID: IDDoRegistro}; {Column1: NovoValor1; Column2: NovoValor2})`

81. **Excluir um registro de uma fonte de dados:**
    - `Remove(TabelaDeDados; {ID: IDDoRegistro})`

82. **Obter a contagem total de itens em uma fonte de dados:**
    - `CountRows(TabelaDeDados)`

83. **Verificar se uma condição é verdadeira para todos os itens em uma fonte de dados:**
    - `ForAll(TabelaDeDados; Condição)`

84. **Verificar se uma condição é verdadeira para algum item em uma fonte de dados:**
    - `Exists(TabelaDeDados; Condição)`

85. **Obter um subconjunto de uma tabela baseado em um intervalo específico:**
    - `FirstN(TabelaDeDados; QuantidadeDeItens)`

86. **Adicionar uma borda a um controle:**
    - `BorderColor = RGB(255; 0; 0)`

87. **Definir o tamanho de um controle com base em outro controle:**
    - `Width = OutroControle.Width`

88. **Definir o preenchimento de um controle:**
    - `Padding = 20`

89. **Definir a cor de fundo de um controle:**
    - `Fill = RGBA(255; 255; 255; 0.5)`

90. **Definir a fonte de um controle:**
    - `Font = Arial`

91. **Definir o tamanho da fonte de um controle:**
    - `Size = 18`

92. **Definir o alinhamento de texto de um controle:**
    - `Align = Center`

93. **Definir a cor do texto de um controle:**
    - `Color = RGB(0; 0; 255)`

94. **Obter o valor de um controle de entrada de texto:**
    - `TextInput1.Text`

95. **Definir o valor de um controle de entrada de texto:**
    - `TextInput1.Default = "Valor padrão"`

96. **Verificar se um controle de entrada de texto está vazio:**
    - `IsBlank(TextInput1.Text)`

97. **Definir o valor de um controle de caixa de seleção:**
    - `CheckBox1.Default = true`

98. **Verificar se um controle de caixa de seleção está marcado:**
    - `CheckBox1.Value`

99. **Definir o valor de um controle de opção de botão:**
    - `RadioButton1.Default = "Opção 1"`

100. **Verificar qual opção de botão foi selecionada:**  
    - `RadioButton1.Selected.Value`

101. **Definir o valor mínimo de um controle de deslize:**  
    - `Slider1.Min = 0`

102. **Definir o valor máximo de um controle de deslize:**  
    - `Slider1.Max = 100`
  
103. **Obter o valor atual de um controle de deslize:**  
    - `Slider1.Value`

104. **Definir o valor padrão de um controle de deslize:**  
    - `Slider1.Default = 50`

105. **Definir o intervalo de valores de um controle de deslize:**  
    - `Slider1.Step = 5`

106. **Definir a orientação de um controle de deslize:**  
    - `Slider1.Orientation = Vertical`

107. **Definir o valor mínimo de um controle de entrada numérica:**  
    - `NumericUpDown1.Min = 0`

108. **Definir o valor máximo de um controle de entrada numérica:**  
    - `NumericUpDown1.Max = 100`

109. **Obter o valor atual de um controle de entrada numérica:**  
    - `NumericUpDown1.Value`

110. **Definir o valor padrão de um controle de entrada numérica:**  
    - `NumericUpDown1.Default = 50`

111. **Definir o intervalo de valores de um controle de entrada numérica:**  
    - `NumericUpDown1.Step = 5`

112. **Definir o número de casas decimais de um controle de entrada numérica:**  
    - `NumericUpDown1.Precision = 2`

113. **Definir o formato de data de um controle de calendário:**  
    - `DatePicker1.Format = DateFormat.ShortDate`

114. **Obter a data selecionada em um controle de calendário:**  
    - `DatePicker1.SelectedDate`

115. **Definir a data padrão de um controle de calendário:**  
    - `DatePicker1.DefaultDate = Today()`

116. **Definir os itens em um controle de caixa de listagem:**  
    - `ListBox1.Items = ["Item 1"; "Item 2"; "Item 3"]`

117. **Definir o item padrão em um controle de caixa de listagem:**  
    - `ListBox1.Default = ["Item 1"]`

118. **Obter os valores selecionados em um controle de galeria:**  
    - `Gallery1.Selected.Value`

119. **Definir a fonte de dados de uma galeria:**  
    - `Gallery1.Items = DataSource1`

120. **Definir o modelo de exibição de uma galeria:**  
    - `Gallery1.Template = GalleryTemplate1`

121. **Definir a ordem de classificação de uma galeria:**  
    - `Gallery1.SortByColumns(DataSource1; "Field1"; Ascending)`

122. **Filtrar uma galeria com base em uma condição:**  
    - `Filter(DataSource1; Field1 = "Value1")`

123. **Obter o valor de um campo de uma tabela em uma galeria:**  
    - `ThisItem.Field1`

124. **Definir o valor de um campo de uma tabela em uma galeria:**  
    - `Patch(DataSource1; Lookup(DataSource1; ID = ThisItem.ID); {Field1: "New Value"})`

125. **Filtrar uma galeria com base na entrada de um usuário:**  
    - `Filter(DataSource1; SearchBox1.Text in Field1)`

126. **Definir o texto de um controle de rótulo:**  
    - `Label1.Text = "Texto do rótulo"`

127. **Definir o tamanho do texto de um controle de rótulo:**  
    - `Label1.Size = FontSize.Large`

128. **Definir a cor do texto de um controle de rótulo:**  
    - `Label1.Color = Color.Red`

129. **Definir o estilo do texto de um controle de rótulo:**  
    - `Label1.FontWeight = FontWeight.Bold`

130. **Definir o alinhamento do texto de um controle de rótulo:**  
    - `Label1.Align = TextAlign.Center`

131. **Obter o valor de um controle de caixa de texto:**  
    - `TextInput1.Text`

132. **Definir o valor de um controle de caixa de texto:**  
    - `TextInput1.Text = "Novo valor"`

133. **Limpar o valor de um controle de caixa de texto:**  
    - `TextInput1.Text = ""`

134. **Validar um controle de caixa de texto:**  
    - `If(IsBlank(TextInput1.Text); Notify("Digite um valor válido"; NotificationType.Error))`

135. **Definir o formato de um controle de número:**  
    - `NumericUpDown1.Format = "0.00"`

136. **Definir o valor mínimo de um controle de número:**  
    - `NumericUpDown1.MinValue = 0`

137. **Definir o valor máximo de um controle de número:**  
    - `NumericUpDown1.MaxValue = 100`

138. **Obter o valor de um controle de alternância:**  
    - `Toggle1.Value`

139. **Definir o valor de um controle de alternância:**  
    - `Toggle1.Value = true`

140. **Definir a cor de fundo de um controle:**  
    - `Control1.Fill = Color.Yellow`

141. **Definir a cor da borda de um controle:**  
    - `Control1.BorderColor = Color.Blue`

142. **Definir o estilo da borda de um controle:**  
    - `Control1.BorderStyle = BorderStyle.Solid`

143. **Definir a largura da borda de um controle:**  
    - `Control1.BorderThickness = Thickness.Medium`

144. **Definir a visibilidade de um controle:**  
    - `Control1.Visible = false`

145. **Definir a habilitação de um controle:**  
    - `Control1.Enabled = false`

146. **Definir a exibição de um controle:**  
    - `Control1.DisplayMode = DisplayMode.Edit`

147. **Obter a seleção atual de um controle de lista suspensa:**  
    - `Dropdown1.Selected.Value`

148. **Definir a seleção atual de um controle de lista suspensa:**  
    - `Dropdown1.Selected = {Value: "Valor selecionado"}`

149. **Definir os itens de um controle de lista suspensa:**  
    - `Dropdown1.Items = ["Opção 1"; "Opção 2"; "Opção 3"]`

150. **Definir os valores de um controle de lista suspensa:**  
    - `Dropdown1.Items = [{Value: "opcao1"; Text: "Opção 1"}; {Value: "opcao2"; Text: "Opção 2"}; {Value: "opcao3"; Text: "Opção 3"}]`

151. **Obter a seleção atual de um controle de caixa de seleção:**  
    - `Checkbox1.Value`

152. **Definir a seleção atual de um controle de caixa de seleção:**  
    - `Checkbox1.Value = true`

153. **Obter a seleção atual de um controle de botão de opção:**  
    - `Radio1.Selected.Value`

154. **Definir a seleção atual de um controle de botão de opção:**  
    - `Radio1.Selected = {Value: "Valor selecionado"}`

155. **Definir a fonte de dados de um controle de galeria:**  
    - `Gallery1.Items = Table1`

156. **Ordenar os itens de uma galeria por ordem alfabética crescente:**  
    - `Sort(Gallery1.Items; TextColumn1.Text; Ascending)`

157. **Ordenar os itens de uma galeria por ordem alfabética decrescente:**  
    - `Sort(Gallery1.Items; TextColumn1.Text; Descending)`

158. **Filtrar os itens de uma galeria com base em uma condição:**  
    - `Filter(Gallery1.Items; Value > 5)`

159. **Contar o número de itens em uma galeria que atendem a uma condição:**  
    - `CountRows(Filter(Gallery1.Items; Value > 5))`

160. **Obter o valor de um controle de tela:**  
    - `Screen1.Width`

161. **Definir o título de uma tela:**  
    - `Screen1.Title = "Título da tela"`

162. **Definir a cor de fundo de uma tela:**  
    - `Screen1.Fill = Color.Blue`

163. **Definir a imagem de fundo de uma tela:**  
    - `Screen1.BackgroundImage = Image1.Image`

164. **Converter uma string em um número:**  
    - `Value("123")`

165. **Converter um número em uma string:**  
    - `Text(123)`

166. **Arredondar um número para o inteiro mais próximo:**  
    - `Round(3.6)`

167. **Arredondar um número para o número especificado de casas decimais:**  
    - `Round(3.14159; 2)`

168. **Calcular a soma de uma coluna em uma tabela:**  
    - `Sum(Table1; Value)`

169. **Calcular a média de uma coluna em uma tabela:**  
    - `Average(Table1; Value)`

170. **Calcular o mínimo de uma coluna em uma tabela:**  
    - `Min(Table1; Value)`

171. **Calcular o máximo de uma coluna em uma tabela:**  
    - `Max(Table1; Value)`

172. **Encontrar o valor mais comum em uma coluna em uma tabela:**  
    - `Mode(Table1; Value)`

173. **Encontrar a mediana de uma coluna em uma tabela:**  
    - `Median(Table1; Value)`

174. **Obter a localização atual do dispositivo do usuário:**  
    - `Location.Latitude` `Location.Longitude`

175. **Abrir o aplicativo de mapas padrão do dispositivo com as coordenadas da localização atual:**  
    - `Launch("bingmaps:?cp=" & Location.Latitude & "~" & Location.Longitude)`

176. **Exibir uma notificação na tela do usuário:**  
    - `Notify("Mensagem de notificação"; NotificationType.Warning)`

177. **Obter a data e hora em um fuso horário específico:**  
    - `UtcToZone(Now(); "Pacific Standard Time")`

178. **Converter uma data e hora em um formato de texto personalizado:**  
    - `Text(Now(); "dd/MM/yyyy hh:mm:ss")`

179. **Verificar se um valor está presente em uma lista:**  
    - `"Valor" in ["Valor1"; "Valor2"; "Valor3"]`

180. **Concatenar duas strings:**  
    - `"Olá" & "Mundo"`

181. **Obter o comprimento de uma string:**  
    - `Len("Texto")`

182. **Substituir um trecho de uma string por outro:**  
    - `Replace("Texto original"; "original"; "novo")`

183. **Transformar uma string em maiúsculas:**  
    - `Upper("Texto em maiúsculas")`
     
185. **Obter a data e hora atuais:**    
   - `Now()`

185. **Obter o valor de um controle de botão de alternância:**    
  - `Toggle1.Value`

186. **Definir o valor de um controle de botão de alternância:**    
   - `UpdateContext({Toggle1Value: true})`

187. **Adicionar um item a uma coleção:**    
   - `Collect(Colecao, {Nome: "NovoItem", Valor: 123})`

188. **Limpar todos os itens de uma coleção:**  
   - `Clear(Colecao)`

189. **Obter o número de itens em uma coleção:**  
   - `CountRows(Colecao)`

190. **Filtrar itens de uma coleção com base em uma condição:**  
  -  `Filter(Colecao, Nome = "Item1")`

191. **Concatenar valores de uma coleção em uma string:**  
  -  `Concat(Colecao, Nome, ", ")`

191. **Verificar se um valor existe em uma coleção:**  
  -  `If(CountRows(Filter(Colecao, Nome = "Item1")) > 0, true, false)`

192. **Ordenar itens em uma coleção:**  
  -  `Sort(Colecao, Nome, Ascending)`

193. **Adicionar um novo item a uma galeria:**  
   - `Patch(NomeDaGaleria, Defaults(NomeDaGaleria), {Nome: "NovoItem", Valor: 123})`

194. **Verificar se um controle de entrada de texto tem um valor numérico:**  
   - `IsNumeric(TextInput1.Text)`

195. **Verificar se um controle de entrada de texto é uma data válida:**  
  -  `IsDate(TextInput1.Text)`

196. **Obter o primeiro item de uma coleção:**  
   - `First(Colecao)`

197. **Obter o último item de uma coleção:**  
  -  `Last(Colecao)`

198. **Navegar para outra tela:**  
   - `Navigate(Screen2)`

199. **Obter o valor mínimo de uma coluna em uma coleção:**  
  -  `Min(Colecao, Valor)`

200. **Obter o valor máximo de uma coluna em uma coleção:**  
   - `Max(Colecao, Valor)`

201. **Verificar se um valor está dentro de um intervalo:**  
   - `If(Value >= MinValue And Value <= MaxValue, true, false)`
   - 
- **Transformar uma string em minúsculas:**
  `Lower("Texto em minúsculas")`

- **Capitalizar a primeira letra de uma string:**
  `Proper("texto com a primeira letra em maiúscula")`

- **Dividir uma string em uma lista com base em um caractere delimitador:**
  `Split("Maçã, Laranja, Banana", ",")`

- **Verificar se uma string começa com um determinado texto:**
  `StartsWith("Hello world", "Hello")`

- **Verificar se uma string termina com um determinado texto:**
  `EndsWith("Hello world", "world")`

- **Obter o índice da primeira ocorrência de um determinado texto em uma string:**
  `Find("world", "Hello world")`

- **Obter uma substring de uma string com base em um índice inicial e um comprimento:**
  `Mid("Texto original", 7, 4)`

- **Obter a posição de um caractere dentro de uma string:**
  `CharPosition("Texto", "o")`

- **Calcular a diferença em dias entre duas datas:**
  `DateDiff(FirstDate, SecondDate, Day)`

- **Calcular a diferença em horas entre duas datas:**
  `DateDiff(FirstDate, SecondDate, Hour)`

- **Calcular a diferença em minutos entre duas datas:**
  `DateDiff(FirstDate, SecondDate, Minute)`

- **Calcular a diferença em segundos entre duas datas:**
  `DateDiff(FirstDate, SecondDate, Second)`

- **Obter a data atual:**
  `Today()`

- **Adicionar um número de dias a uma data:**
  `AddDays(Today(), 7)`

- **Adicionar um número de horas a uma data e hora:**
  `AddHours(Now(), 3)`

- **Adicionar um número de minutos a uma data e hora:**
  `AddMinutes(Now(), 15)`

- **Adicionar um número de segundos a uma data e hora:**
  `AddSeconds(Now(), 30)`

- **Obter o nome do usuário atual:**
  `User().FullName`

- **Obter o endereço de e-mail do usuário atual:**
  `User().Email`

- **Abrir um URL em um navegador:**
  `Launch("https://www.exemplo.com")`

- **Obter o valor de um controle de rótulo:**
  `Label1.Text`

- **Definir o valor de um controle de rótulo:**
  `UpdateContext({LabelText: "Novo valor"})`

- **Obter o valor de um controle de galeria:**
  `Gallery1.Selected.ID`

- **Definir o valor de um controle de galeria:**
  `UpdateContext({SelectedID: Gallery1.Selected.ID})`

- **Filtrar uma galeria com base em uma condição:**
  `Filter(Gallery1.AllItems, Nome = "João")`

- **Classificar uma galeria por ordem crescente de um campo:**
  `SortByColumns(Gallery1.AllItems, "Nome", Ascending)`

- **Classificar uma galeria por ordem decrescente de um campo:**
  `SortByColumns(Gallery1.AllItems, "Nome", Descending)`

- **Contar o número de itens em uma galeria:**
  `CountRows(Gallery1.AllItems)`

- **Definir o foco em um controle:**
  `SetFocus(TextInput1)`

- **Limpar o valor de um controle de entrada de texto:**
  `UpdateContext({TextInput1: ""})`

- **Adicionar uma nova linha a uma tabela do SharePoint:**
  `Patch(NomeDaLista, Defaults(NomeDaLista), {Coluna1: "Valor1", Coluna2: "Valor2"})`

- **Atualizar uma linha existente em uma tabela do SharePoint:**
  `Patch(NomeDaLista, Gallery1.Selected, {Coluna1: "NovoValor1", Coluna2: "NovoValor2"})`

- **Excluir uma linha de uma tabela do SharePoint:**
  `Remove(NomeDaLista, Gallery1.Selected)`

- **Obter o valor de um controle de lista suspensa filtrado por outra lista suspensa:**
  `Filter(ListaDeItens, IDdaOutraLista = Dropdown1.Selected.ID)`

- **Obter o valor de um controle de lista suspensa filtrado por uma galeria:**
  `Filter(ListaDeItens, IDdaGaleria = Gallery1.Selected.ID)`

- **Obter o valor de um controle de lista suspensa filtrado por um controle de entrada de texto:**
  `Filter(ListaDeItens, Nome = TextInput1.Text)`

- **Obter o valor de um controle de lista suspensa filtrado por uma data:**
  `Filter(ListaDeItens, Data >= DatePicker1.SelectedDate)`

- **Obter o valor de um controle de lista suspensa filtrado por um controle de alternância:**
  `Filter(ListaDeItens, Ativo = Toggle1.Value)`

- **Obter o valor de um controle de lista suspensa filtrado por uma caixa de seleção:**
  `Filter(ListaDeItens, Selecionado = Checkbox1.Value)`

- **Calcular a soma de uma coluna em uma galeria:**
  `Sum(Gallery1.AllItems, ValorDaColuna)`

- **Ordenar uma galeria por uma coluna:**
  `Sort(Gallery1.AllItems, NomeDaColuna, Descending)`

- **Filtrar uma galeria por uma data:**
  `Filter(Gallery1.AllItems, Data >= DatePicker1.SelectedDate)`

- **Filtrar uma galeria por um controle de entrada de texto:**
  `Filter(Gallery1.AllItems, StartsWith(NomeDaColuna, TextInput1.Text))`

- **Verificar se uma entrada de texto está vazia:**
  `IsBlank(TextInput1.Text)`

- **Converter um valor de texto em um valor numérico:**
  `Value(TextInput1.Text)`

- **Definir o valor padrão de um controle de entrada de texto:**
  `Set(TextInput1.Default, "ValorPadrão")`

- **Definir o foco em um controle de entrada de texto:**
  `SetFocus(TextInput1)`

- **Definir o texto de um controle de rótulo:**
  `Set(Label1.Text, "NovoTexto")`

- **Ocultar um controle de rótulo:**
  `Set(Label1.Visible, false)`

- **Exibir uma mensagem de aviso em uma caixa de diálogo:**
  `Notify("Mensagem de aviso", NotificationType.Warning)`


---

**Calcular o valor total de uma coluna em uma tabela:**
Para somar todos os valores de uma coluna específica em uma tabela, você usa a fórmula:
`Sum(Tabela, Valor)`, onde "Tabela" é o nome da tabela e "Valor" é o nome da coluna que contém os valores a serem somados.

**Filtrar uma tabela com base em uma condição:**
Para obter apenas as linhas de uma tabela que atendem a uma condição específica, você pode usar:
`Filter(Tabela, Condição)`, onde "Tabela" é o nome da tabela e "Condição" é o critério usado para filtrar os registros.

**Classificar uma tabela em ordem crescente ou decrescente:**
Para ordenar os registros de uma tabela com base em uma coluna específica, utilize:
`Sort(Tabela, NomeDaColuna, Ascending/Descending)`, onde "Tabela" é o nome da tabela, "NomeDaColuna" é a coluna pela qual você deseja classificar, e "Ascending" ou "Descending" define a ordem da classificação.

**Encontrar o valor máximo de uma coluna em uma tabela:**
Para identificar o valor máximo em uma coluna específica de uma tabela, você usa:
`Max(Tabela, NomeDaColuna)`, onde "Tabela" é o nome da tabela e "NomeDaColuna" é o nome da coluna para encontrar o valor máximo.

**Encontrar o valor mínimo de uma coluna em uma tabela:**
Para identificar o valor mínimo em uma coluna específica de uma tabela, utilize:
`Min(Tabela, NomeDaColuna)`, onde "Tabela" é o nome da tabela e "NomeDaColuna" é o nome da coluna para encontrar o valor mínimo.

**Encontrar a média de uma coluna em uma tabela:**
Para calcular a média dos valores em uma coluna específica de uma tabela, você pode usar:
`Average(Tabela, NomeDaColuna)`, onde "Tabela" é o nome da tabela e "NomeDaColuna" é o nome da coluna para calcular a média.

**Contar o número de linhas em uma tabela:**
Para contar quantas linhas existem em uma tabela, use:
`CountRows(Tabela)`, onde "Tabela" é o nome da tabela a ser contada.

**Atualizar um registro em uma tabela:**
Para atualizar um registro específico em uma tabela, você pode usar:
`Patch(Tabela, LookUp(Tabela, Id = IdDoRegistro), {NomeDaColuna: NovoValor})`, onde "Tabela" é o nome da tabela, "Id" é o nome da coluna de identificação única, "IdDoRegistro" é o identificador do registro a ser atualizado, e "NomeDaColuna" é a coluna a ser atualizada com "NovoValor".

**Concatenar o valor de duas colunas em uma tabela:**
Para concatenar valores de duas colunas diferentes em uma tabela, você usa:
`Concatenate(Tabela.NomeDaColuna1, " ", Tabela.NomeDaColuna2)`, onde "Tabela" é o nome da tabela e "NomeDaColuna1" e "NomeDaColuna2" são os nomes das colunas a serem concatenadas.

**Converter um valor de texto em número:**
Para converter um valor de texto em um número, utilize:
`Value(Texto)`, onde "Texto" é o valor a ser convertido.

**Formatar uma data como uma string:**
Para formatar um valor de data como uma string com um formato específico, use:
`Text(Data, "dd/mm/yyyy")`, onde "Data" é o valor da data e "dd/mm/yyyy" é o formato desejado.

**Verificar se um registro existe em uma tabela com base em uma condição:**
Para verificar se um registro existe na tabela conforme uma condição, utilize:
`If(IsBlank(LookUp(Tabela, Condição)), false, true)`, onde "Tabela" é o nome da tabela e "Condição" é o critério usado para verificar a existência do registro.

**Verificar se uma tabela está vazia:**
Para determinar se uma tabela não contém registros, use:
`If(IsEmpty(Tabela), true, false)`, onde "Tabela" é o nome da tabela a ser verificada.

**Definir o valor padrão de um controle de entrada de dados:**
Para definir um valor padrão em um controle de entrada de dados, você pode usar:
`If(IsBlank(Controle), "ValorPadrão", Controle.Text)`, onde "Controle" é o nome do controle e "ValorPadrão" é o valor que será exibido se o controle estiver vazio.

**Exibir uma mensagem de erro se um controle de entrada de dados estiver vazio:**
Para exibir uma mensagem de erro quando um controle de entrada de dados estiver vazio, utilize:
`If(IsBlank(Controle), Notify("Por favor, preencha este campo.", NotificationType.Error), SubmitForm(Formulario))`, onde "Controle" é o nome do controle e "Formulario" é o formulário a ser enviado após a validação.

**Criar uma lista de itens selecionados em um controle de galeria:**
Para gerar uma lista de IDs de itens selecionados em uma galeria, use:
`Concat(Gallery.Selected, ID & ",")`, onde "Gallery" é o nome do controle de galeria e "ID" é o nome da coluna ID na fonte de dados da galeria.

**Filtrar uma tabela com base em uma lista de valores:**
Para filtrar uma tabela com base em uma lista de valores, você usa:
`Filter(Tabela, Not(IsBlank(Value)) And Value in Lista)`, onde "Tabela" é o nome da tabela, "Value" é a coluna a ser filtrada e "Lista" é a lista de valores para filtrar.

**Classificar uma tabela com base em uma coluna:**
Para ordenar os registros de uma tabela com base em uma coluna, utilize:
`SortByColumns(Tabela, "NomeDaColuna", Descending)`, onde "Tabela" é o nome da tabela e "NomeDaColuna" é a coluna pela qual a tabela será classificada.

**Verificar se um valor está dentro de um intervalo:**
Para verificar se um valor está dentro de um intervalo, você pode usar:
`And(Valor >= ValorMinimo, Valor <= ValorMaximo)`, onde "Valor" é o valor a ser verificado, "ValorMinimo" é o valor mínimo permitido e "ValorMaximo" é o valor máximo permitido.

**Encontrar a posição de um item em uma tabela:**
Para encontrar a posição de um item específico em uma tabela, use:
`Position(Tabela, Item)`, onde "Tabela" é o nome da tabela e "Item" é o valor do item para encontrar a posição.

**Encontrar o índice de um item em uma tabela:**
Para localizar o índice de um item em uma tabela, utilize:
`IndexOf(Tabela, Item)`, onde "Tabela" é o nome da tabela e "Item" é o valor do item para encontrar o índice.

**Encontrar o primeiro item em uma tabela que atende a uma condição:**
Para encontrar o primeiro item em uma tabela que atende a uma condição específica, você pode usar:
`First(Filter(Tabela, Condição))`, onde "Tabela" é o nome da tabela e "Condição" é o critério usado para filtrar.

**Encontrar o último item em uma tabela que atende a uma condição:**
Para encontrar o último item em uma tabela que atende a uma condição, utilize:
`Last(Filter(Tabela, Condição))`, onde "Tabela" é o nome da tabela e "Condição" é o critério usado para filtrar.

**Concatenar valores de colunas em uma tabela:**
Para concatenar valores de diferentes colunas em uma tabela, use:
`Concatenate(Table1.NomeDaColuna1, Table1.NomeDaColuna2, Table2.NomeDaColuna3)`, onde "Table1" e "Table2" são os nomes das tabelas e "NomeDaColuna1", "NomeDaColuna2" e "NomeDaColuna3" são os nomes das colunas a serem concatenadas.

**Verificar se um item existe em uma tabela:**
Para verificar se um item específico existe em uma tabela, utilize:
`If(CountRows(Filter(Tabela, ID = ID_do_Item)) = 1, true, false)`, onde "Tabela" é o nome da tabela, "ID" é o nome da coluna que contém o ID, e "ID_do_Item" é o valor do ID do item a ser verificado.

**Verificar se um registro foi alterado em uma tabela:**
Para verificar se um registro foi alterado desde a última modificação, você pode usar:
`If(LookUp(Tabela, ID = ID_do_Item, Modified) > LastSubmit(Tabela).Modified, true, false)`, onde "Tabela" é o nome da tabela, "ID" é o nome da coluna que contém o ID, "ID_do_Item" é o valor do ID do item a ser verificado, e "Modified" é o nome da coluna que contém a data de modificação.

**Verificar se uma data está dentro de um intervalo:**
Para verificar se uma data está dentro de um intervalo específico, use:
`If(And(Data >= Data_Inicial, Data <= Data_Final), true, false)`, onde "Data" é a data a ser verificada, e "Data_Inicial" e "Data_Final" são as datas que definem o intervalo.

**Contar o número de caracteres em uma string:**
Para contar quantos caracteres existem em uma string, utilize:
`Len(String)`, onde "String"


