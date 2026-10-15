# Histórico de Versões

Todas as mudanças relevantes do Geotag & EXIF Studio.

## [2.0] — 2026-10-15

Versão de reestruturação: correções de confiabilidade, metadados preservados, nova bússola, controle de aparência, identidade visual do IBAMA e lotes grandes. Continua em um único arquivo HTML, 100% offline.

### Novidades
- **Metadados preservados na exportação:** os blocos EXIF (GPS, data, câmera) e XMP originais são copiados para os JPEGs exportados, com `Orientation` ajustada e miniatura antiga desreferenciada. Opção ativada por padrão.
- **Bússola com modos Seta Norte e Rumo da Câmera**, com seleção automática pela inclinação do gimbal DJI (`GimbalPitchDegree`).
- **Fundo e transparência** com padrão global para texto, bússola e logotipo, ajuste individual por elemento, opacidade do conteúdo (marca d'água no logo) e contorno de legibilidade automático.
- **Logotipo oficial do IBAMA** embutido como padrão (base64, pixels idênticos ao original, sem alterações), com botão para restaurá-lo.
- **Paleta azul harmonizada com o logotipo do IBAMA** (`#15306E` e `#0093DD`) na interface e no preset padrão **Azul IBAMA**.
- **Altura relativa à decolagem** (`RelativeAltitude`) e escolha da altitude estampada.
- **Data em `DD/MM/AAAA` e hora em `HH:MM`**, com exibição independente.
- **Gravação do lote direto em pasta** (Chrome/Edge), sem ZIP e sem sobrescrever arquivos.
- **ZIP dividido em partes** de até 2 GB em lotes grandes.
- Painel mostra altura relativa e inclinação do gimbal.
- **Ajuda embutida:** botão **Ajuda** no cabeçalho (ou `F1`) abre o guia de uso dentro do app, sem internet.

### Melhorias
- Pré-visualização em resolução reduzida (até 2560 px), subindo até a resolução total ao ampliar; exportação sempre em resolução total.
- Gerador de ZIP próprio (método STORE), sem recompressão de JPEG e sem cópias em memória; a biblioteca JSZip foi removida.
- Leitura de metadados apenas no trecho inicial do arquivo, com tempo limite.
- Equipamento em modo automático usa o modelo de cada foto no lote.
- ZIP preserva subpastas e gera nomes únicos.
- Relatório de fotos com falha ao final do lote.
- Redesenhos agrupados por quadro de tela.
- Card do logotipo antes da bússola; tamanho padrão de 15% para ambos.
- Rodapé redesenhado em três linhas (autoria, link para o repositório no GitHub e licença), nas cores do projeto.
- Novos padrões: faixa no centro inferior, texto superior em branco, azimute no texto e selo de ângulo desligados.

### Correções
- Regex do XMP da DJI perdia os escapes (`\s`) e falhava com espaços ao redor do `=`.
- Altitude abaixo do nível do mar (`GPSAltitudeRef = 1`) aparecia positiva.
- O lote podia travar para sempre (leitura EXIF sem resposta ou `toBlob` nulo) e ficar preso no estado "processando".
- Cliques rápidos na fila podiam exibir uma foto com metadados de outra.
- A fila podia ser alterada durante o lote.
- Fotos de mesmo nome em subpastas diferentes se sobrescreviam no ZIP.
- O texto digitado no campo de equipamento era sobrescrito ao trocar de foto.
- Rótulos dos controles perdiam o símbolo `%`.
- Remover uma foto acima da selecionada mudava a seleção.
- Nome do equipamento duplicado ou truncado ("DJI DJI ...", "FC3582" sem fabricante).
- Avisos seguidos se escondiam antes do tempo.
- Referência externa de source map removida da biblioteca EXIF-js.

## [1.1]

Versão inicial publicada (commit `aa26018`): estampa de GPS, altitude, azimute, equipamento e data/hora; bússola vetorial em 6 posições; logotipo; presets; zoom e arrasto; processamento em lote com ZIP.

[2.0]: https://github.com/KuriakinToscan/Geotag_Studio/releases/tag/2.0
[1.1]: https://github.com/KuriakinToscan/Geotag_Studio/tree/aa260184e8cae2073a96e8a5e0b1b03f014c90af
