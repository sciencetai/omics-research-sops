# POP-MET-001

Título: Conversão de dados RAW para mzML

Software: MSConvert

Elaborado por: Tainá Schons

Data de criação:: 01/06/2026

Última atualização:: 01/06/2026

Nível: Básico

## Objetivo

<aside>
Descrever o procedimento para converter arquivos brutos de espectrometria de massas (.raw) para o formato aberto mzML utilizando o software MSConvert, garantindo compatibilidade com plataformas de processamento de dados metabolômicos, lipidômicos e proteômicos, como MZmine, MS-DIAL, GNPS e SIRIUS.

</aside>

## Fundamentação teórica

> O formato mzML é um padrão aberto desenvolvido pela Human Proteome Organization Proteomics Standards Initiative (HUPO-PSI), amplamente utilizado para o armazenamento e o intercâmbio de dados de espectrometria de massas. A conversão de arquivos proprietários para mzML aumenta a reprodutibilidade, facilita o compartilhamento de dados e permite o uso de diferentes plataformas de processamento e análise.
> 

## Aplicação:

> Dados adquiridos em instrumentos Waters, Thermo, Agilent, Bruker e SCIEX compatíveis com o ProteoWizard
> 

## Softwares e Ferramentas necessários

| Software | Versão |
| --- | --- |
| ProteoWizard MSConvert |  |
| Sistema Operacional Windows | 10 ou superior |

## Arquivos de Entrada

- `.raw` (Waters)
- `.RAW` (Thermo)
- Outros formatos compatíveis

## Procedimento

### Passo 1 - Abrir o MSConvert

Abrir o software.



### Passo 2 - Selecionar os arquivos

Selecionar os arquivos RAW desejados.

- Clicar em **Browse**
- Navegar até a pasta contendo os arquivos
- Selecionar os arquivos a serem convertidos



### Passo 3 – Definir diretório de saída

Selecionar a pasta onde os arquivos mzML serão salvos. “Output directory”

### Passo 4 – Configurar formato de saída

Em "Output format":

- Selecionar **mzML**

---

### Passo 5 – Configurar filtros

### Dados centroidados

Não aplicar filtros adicionais.

### Dados em modo contínuo (profile)

Aplicar:

> Peak Picking
Vendor
MS Levels: 1-
> 

Demais opções podem ser mantidas como estão.



<aside>
⚠️ Dados adquiridos no modo profile (contínuo) devem ser centroidizados durante a conversão para mzML. Recomenda-se o uso do filtro Peak Picking (Vendor), que aplica o algoritmo de centroidização fornecido pelo fabricante do instrumento. Após a conversão, os arquivos serão reconhecidos como centroidados pelos softwares de processamento.

</aside>

### Passo 6 – Iniciar conversão

Clicar em: “start” e aguardar a conclusão do processamento.

## Controle de Qualidade

Após a conversão verificar:

- Todos os arquivos foram convertidos
- Nenhum erro foi reportado pelo software
- O tamanho dos arquivos é compatível com o esperado
- Os arquivos mzML podem ser abertos no MZmine

---

## Saídas Esperadas

- Arquivos .mzML

## Problemas Comuns

| Problema | Possível causa | Solução |
| --- | --- | --- |
| Reader Fail | Caracteres especiais no caminho (á, ç, ã) | Mover arquivos para diretório sem caracteres especiais |
| Arquivo não abre no MZmine | Conversão incompleta | Repetir processo |
| Arquivo muito pequeno | Configuração incorreta de filtros | Revisar parâmetros |
|  |  |  |

## Referências

ProteoWizard Documentation - [https://proteowizard.sourceforge.io/](https://proteowizard.sourceforge.io/)

HUPO-PSI mzML Specification - [https://www.psidev.info/mzml](https://www.psidev.info/mzml)

## Observações

- Evitar nomes de pastas contendo caracteres especiais (á, é, í, ó, ú, ç, ã).
- Manter os arquivos RAW originais sem modificações.
- Armazenar os arquivos mzML em diretório separado dos arquivos brutos.

---

## Histórico de Revisões

| Data | Alteração | Responsável |
| --- | --- | --- |
| 01/06/2026 | Elaboração | Tainá Schons |
