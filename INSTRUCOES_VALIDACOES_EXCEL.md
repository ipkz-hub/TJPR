# Instruções para Configuração da Planilha de Escala do Plantão Judiciário

## 1. Validações por Aba

### Unidades Judiciárias
- **Nome da Unidade**: Texto livre
- **Comarca**: Lista suspensa
  ```excel
  Dados > Validação de Dados > Lista
  Origem: "Colombo,Campina Grande do Sul,Quatro Barras"
  ```
- **Telefone Fixo**: Máscara de entrada
  ```excel
  Dados > Validação de Dados > Personalizado
  Fórmula: =E(LEN(B2)=14;ÉERRO(VALOR(SUBSTITUIR(SUBSTITUIR(SUBSTITUIR(B2;"(";"");";"-";""))))=FALSO)
  ```

### Magistrados
- **Designação**: Lista suspensa "Titular,Substituto"
- **Unidade**: Lista dinâmica vinculada à aba Unidades_Judiciarias
- **Antiguidade**: Apenas números inteiros positivos
- **Datas**: Formato data com calendário
- **Afastamentos**: Texto com validação de formato "DD/MM/AAAA a DD/MM/AAAA"

### Servidores
- **Cargo**: Lista suspensa predefinida
- **Unidade**: Lista dinâmica vinculada
- **Telefone**: Mesma validação dos telefones fixos

### OJs (ambas abas)
- **Telefone**: Mesma validação dos telefones fixos
- **Datas**: Formato data com calendário

### Feriados
- **Data**: Formato data com calendário
- **Tipo**: Lista suspensa "Feriado,Ponto facultativo"
- **Comarca**: Lista suspensa igual à da primeira aba

### Escala do Plantão
- **Início/Fim**: Fórmulas para cálculo automático
  ```excel
  Início: =SOMASE(Linha_anterior;1;"18:00")
  Fim: =SOMASE(Início;7;"12:00")
  ```
- **Magistrado**: Lista dinâmica da aba Magistrados
- **Demais campos**: Fórmulas PROCV para preenchimento automático

## 2. Formatação Condicional

### Conflitos de Afastamento
```excel
Fórmula: =E(NÃO(ÉVAZIO([@Magistrado]));PROCURAR([@Inicio_Plantao];Magistrados[Afastamentos]))
Formatação: Preenchimento Vermelho
```

### Feriados
```excel
Fórmula: =NÃO(ÉVAZIO(PROCV([@Data];Feriados[Data];1;FALSO)))
Formatação: Preenchimento Amarelo
```

## 3. Fórmulas para Estatísticas

### Contagem de Plantões por Magistrado
```excel
=CONT.SES(Escala_Plantao[Magistrado];[@Magistrado];Escala_Plantao[Inicio_Plantao];">="&ANO_ATUAL)
```

### Plantões com Feriado
```excel
=CONT.SES(Escala_Plantao[Magistrado];[@Magistrado];Escala_Plantao[Tem_Feriado];"Sim")
```

## 4. Dicas Importantes
- Sempre nomeie os intervalos de dados para facilitar as fórmulas
- Use PROCV com ÍNDICE/CORRESP para buscas mais robustas
- Mantenha backup regular dos dados
- Proteja as células com fórmulas para evitar alterações acidentais

## 5. Suporte
Em caso de dúvidas ou problemas na configuração, entre em contato com a equipe de suporte.