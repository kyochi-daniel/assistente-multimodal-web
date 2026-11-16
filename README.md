# Assistente multimodal no navegador

Aplicação de estudo que combina uma pergunta em texto com uma imagem ou um áudio e apresenta a resposta em português. O objetivo é explorar inferência com as APIs de IA do navegador, respostas em streaming e tradução, sem configurar um servidor de inferência ou uma chave de API externa.

Projeto acadêmico realizado durante minha pós-graduação em Engenharia de Software com IA Aplicada, a partir do material didático da Unipds.

## Como funciona

1. A aplicação verifica a presença das APIs `LanguageModel`, `Translator` e `LanguageDetector` e tenta preparar os recursos necessários.
2. O usuário escreve uma pergunta, ajusta temperatura e top K e, opcionalmente, seleciona uma imagem ou um áudio.
3. O serviço cria uma sessão com entradas de texto, imagem e áudio e solicita uma resposta textual em inglês.
4. A interface apresenta os trechos recebidos em streaming.
5. Ao concluir a geração, o serviço detecta o idioma e tenta traduzir a resposta de inglês para português. Se a tradução falhar, mantém o texto original.

O botão **Parar** solicita o cancelamento da geração. Cada envio cria uma nova sessão; o histórico das perguntas anteriores não é mantido.

## Tecnologias

- JavaScript com ES Modules, HTML e CSS.
- APIs de IA do navegador: `LanguageModel`, `Translator` e `LanguageDetector`.
- `AbortController` para cancelamento da geração.
- `http-server` para servir os arquivos localmente.

## Pré-requisitos

- Node.js e npm para instalar e iniciar o servidor estático.
- Chrome ou Chrome Canary com as três APIs utilizadas pelo código disponíveis, incluindo suporte às entradas multimodais.
- Dispositivo com recursos de processamento, memória e armazenamento compatíveis com os modelos disponibilizados pelo navegador.
- Conexão com a internet para instalação e eventual download dos modelos e recursos de idioma.

A compatibilidade depende da versão do navegador, da disponibilidade das APIs e do dispositivo. Não há uma matriz de versões e equipamentos validada neste repositório. As indicações de flags exibidas pela aplicação refletem o material didático e podem variar conforme a versão instalada. A presença da interface, por si só, não confirma suporte à inferência multimodal.

## Executar localmente

Na pasta deste projeto:

```bash
npm ci
npm start -- -p 8080
```

Abra [http://localhost:8080](http://localhost:8080). O argumento `-p 8080` fixa a porta do `http-server`; escolha outra porta se ela estiver ocupada. Sirva os arquivos por HTTP local, em vez de abrir `index.html` diretamente.

## Experimentar

1. Aguarde a verificação inicial. Se a aplicação informar indisponibilidade, confira as APIs e os requisitos do navegador. Os downloads informam progresso no console.
2. Comece com uma pergunta textual em inglês, como `Explain what a JavaScript Promise does.`
3. Anexe uma imagem e pergunte `Describe the main objects in this image.`, ou anexe um áudio em inglês e peça um resumo.
4. Compare respostas ao alterar temperatura e top K. Observe o texto durante a geração e a tradução ao final.

O texto da pergunta é obrigatório, mesmo com um arquivo anexado. A interface aceita um arquivo de imagem ou áudio por envio.

## Estrutura

```text
index.html                       Interface
index.js                         Inicialização e verificação dos serviços
controllers/formController.js    Envio, streaming e estado do formulário
services/aiService.js             Disponibilidade, sessão e inferência
services/translationService.js    Detecção de idioma e tradução
views/view.js                    DOM, parâmetros e prévia de arquivos
style.css                        Estilos
```

## Limitações

- É uma demonstração de integração com modelos existentes; não treina ou ajusta um LLM.
- Não oferece alternativa de inferência quando as APIs do navegador estão indisponíveis. A inicialização exige também tradução e detecção de idioma.
- Não há limites próprios de tamanho ou duração dos arquivos, nem uma lista de formatos multimodais testados.
- O cancelamento cobre a geração, mas não interrompe uma tradução já iniciada. Uma tradução pendente pode atualizar a saída posteriormente.
- Não há suíte automatizada de testes ou resultados medidos de qualidade, latência e consumo de recursos.

## Origem e licença

Baseado no exemplo `exemplo-05-webai03-multimodal` do material de Erick Wendel / Unipds, disponível no [repositório original da pós-graduação](https://github.com/unipds-engenharia-de-ia-aplicada/engenharia-de-software-com-ia-aplicada).

O código didático mantém seus créditos originais. Esta organização de portfólio acrescenta documentação de propósito, execução e limitações; não reivindica autoria original da implementação. Consulte a [licença original preservada](LICENSE.md) para os termos de uso e distribuição.
