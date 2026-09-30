# Revisão — Redes de Computadores

## 1. O que acontece quando abrimos uma página?

Imagine que uma pessoa abre o portal da escola pelo navegador.

O navegador atua como **cliente**: envia uma solicitação. Um programa em outro computador pode atuar como **servidor**: recebe a solicitação e envia uma resposta. O navegador usa essa resposta para mostrar a página. Um computador da sua própria casa ou escola pode ser um servidor.

Cliente e servidor são papéis na comunicação. Um mesmo computador pode executar programas com os dois papéis.

## 2. Quem é quem na conexão?

| Elemento | Explicação | Exemplo ou cuidado a ser tomado |
| --- | --- | --- |
| Interface de rede | Meio pelo qual o sistema envia e recebe dados na rede. Pode ser física ou virtual. | A conexão Ethernet e a conexão Wi-Fi utilizam interfaces de rede diferentes. |
| MAC | Endereço de enlace associado à interface, usado na comunicação local dessa tecnologia de rede. | Toda interface tem seu próprio endereço MAC. Este endereço pode ser modificado. |
| IPv4 | Endereço atribuído a uma interface, usado para identificar origem e destino de pacotes IP. | Ter um IPv4 configurado não significa que tem internet, apenas que há contato com a rede local. |
| Gateway padrão | Próximo equipamento para onde vão os pacotes quando não existe uma rota mais específica. | Em uma rede doméstica, normalmente é o roteador. É comum usá-lo para alcançar outras redes. A comunicação entre computadores da mesma rede local normalmente não precisa passar por ele. |
| DNS | Serviço que permite consultar informações de um nome, como o endereço IP correspondente. | Ajuda a descobrir qual IP está associado ao nome de um site. |

**Não confunda:** o DNS normalmente ajuda a ir do nome ao IP. Não é o serviço que entrega o conteúdo do site.

## 3. O caminho de uma solicitação e da resposta

Considere um acesso pelo nome a um servidor que está em outra rede:

1. O navegador precisa obter o IP correspondente ao nome. A informação pode estar em cache; se necessário, ocorre uma consulta DNS.
2. O sistema consulta suas rotas para decidir por onde enviar os pacotes. Quando aplicável, usa o gateway padrão.
3. Os dados saem pela interface escolhida e são transmitidos pela conexão, por exemplo, por cabo ou rádio.
4. Roteadores encaminham os pacotes entre redes até o destino.
5. O servidor recebe e interpreta a solicitação. Depois, envia uma resposta.
6. Os dados da resposta chegam ao cliente, e o navegador os interpreta para exibir o resultado.

O servidor não devolve apenas “o endereço pesquisado”: ele pode devolver conteúdo, como HTML, e informações sobre a resposta. O percurso de volta não precisa passar exatamente pelos mesmos roteadores.

## 4. Cabo, Wi-Fi e equipamentos

**Ethernet:** tecnologia de rede muito usada em conexões por cabo. **Wi-Fi:** tecnologia de rede sem fio que utiliza rádio.

**Switch:** conecta equipamentos em uma rede local cabeada e encaminha quadros entre suas portas. É uma opção quando precisamos de mais portas para computadores com cabo, respeitando a capacidade da rede.

**Roteador:** encaminha pacotes entre redes diferentes. É comum ser o gateway padrão.

**Ponto de acesso (AP):** permite a conexão de dispositivos Wi-Fi à rede local. Pode ajudar a ampliar o atendimento sem fio, mas cobertura e quantidade de clientes dependem do planejamento e da capacidade dos equipamentos.

Um aparelho chamado de “roteador Wi-Fi” pode reunir funções de roteador, switch e ponto de acesso. Isso não torna essas funções iguais.

## 5. Para que servem OSI e TCP/IP?

São modelos para organizar as funções da comunicação. A utilidade aqui é separar perguntas: o sinal chega? Existe caminho entre redes? O serviço respondeu? Não precisamos tratar qualquer erro como “a internet caiu”.

| Função | Modelo OSI | Modelo TCP/IP, na organização de quatro camadas |
| --- | --- | --- |
| Serviços usados pelos programas, representação dos dados e organização do diálogo | Aplicação, apresentação e sessão | Aplicação |
| Comunicação entre aplicações, com protocolos como TCP e UDP | Transporte | Transporte |
| Endereçamento IP e encaminhamento entre redes | Rede | Internet |
| Entrega no enlace local e transmissão de sinais | Enlace e física | Acesso à rede |

Essa correspondência é didática: as implementações reais não precisam ter um programa separado para cada camada OSI.

No envio, os dados recebem informações de controle dos protocolos antes da transmissão. No destino, essas informações são interpretadas para entregar os dados à aplicação. A resposta faz outro processo de envio e recebimento.

**TCP:** oferece um fluxo de bytes ordenado e confiável, com mecanismos como confirmação e retransmissão. **UDP:** envia datagramas sem oferecer, por si só, confirmação de entrega ou ordenação. Uma aplicação pode acrescentar mecanismos próprios sobre UDP. A escolha depende do protocolo e do projeto da aplicação; não é uma escolha manual feita a cada pesquisa no navegador.

## 6. Como investigar com evidências?

**Evidência** é aquilo que foi observado: uma mensagem, uma configuração, um resultado de teste. **Hipótese** é uma explicação possível. Um teste isolado raramente prova que tudo está funcionando ou revela sozinho a causa completa.

| Consulta ou ferramenta | O que observar | O que não concluir automaticamente |
| --- | --- | --- |
| Configuração de rede: `ipconfig` no Windows ou `ip addr` no Linux | Interfaces e endereços configurados | “Apareceu um IP, então a internet funciona.” |
| Rotas: `route print` no Windows ou `ip route` no Linux | Caminhos configurados e gateway padrão | “Existe uma rota, então o destino está funcionando.” |
| `ping` para um endereço IP | Se houve resposta ao teste e perdas naquele momento | “Se respondeu, todos os sites funcionam”; ou “se não respondeu, o computador está desligado”. O teste pode ser bloqueado. |
| `nslookup` para um nome | Resultado da consulta DNS | “O nome resolveu, então a página existe.” |
| Aba Network/Rede do navegador | Solicitações, status e cabeçalhos das respostas | “Qualquer erro mostrado significa que o cabo está desconectado.” |

Os comandos são referências de consulta. Nesta revisão, o principal é compreender o que o resultado permite afirmar, não decorar a escrita de todos eles.

**HTTP 200:** indica sucesso da solicitação naquele contexto; não comprova que todos os recursos da página estão corretos. **HTTP 404:** normalmente indica que o recurso solicitado não foi encontrado. Receber uma resposta HTTP é diferente de não conseguir se comunicar com o serviço.
