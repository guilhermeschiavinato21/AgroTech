# 🌱 AgroTech | Transformação Digital para Hortas Comunitárias

> Plataforma digital centralizada para controlar colheitas, estoque, voluntários e doações das hortas comunitárias gerenciadas pelo Sr. Carlos.

Este repositório contém o **relatório de protótipo de projeto (escopo)** do **AgroTech**: a visão geral, os atores, os casos de uso, a estrutura de dados (diagrama de classes) e o fluxo do processo de doação (BPMN). O documento é uma página HTML estática, pronta para visualização no navegador ou impressão.
 
![Status](https://img.shields.io/badge/status-prot%C3%B3tipo%20de%20escopo-yellow?style=flat)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![UML](https://img.shields.io/badge/UML-casos%20de%20uso%20e%20classes-blue?style=flat)
![BPMN](https://img.shields.io/badge/BPMN-processo%20de%20doa%C3%A7%C3%A3o-green?style=flat)

---

## 🎯 Sobre o projeto

| Componente | Descrição |
|---|---|
| **Objetivo principal** | Realizar a transformação digital das hortas comunitárias gerenciadas pelo Sr. Carlos. |
| **Problema atual** | Processos manuais de colheita, estoque, voluntários e doações, gerando perda de dados e falta de histórico. |
| **Solução proposta** | Plataforma digital segura e centralizada, garantindo controle operacional e eficiência administrativa. |

---

## 👥 Atores

### Principais
- **Administrador:** gestão geral do sistema (cadastrar hortas, registrar colheitas, controlar estoques, gerenciar doações e emitir relatórios).
- **Voluntários:** acesso a cronogramas de atividades, confirmação em colheitas e registro de tarefas realizadas.
- **Beneficiários:** recebimento das doações realizadas pela comunidade.

### Secundários
- **Fornecedores e parceiros comunitários:** stakeholders que influenciam diretamente a operação.

---

## ⚙️ Funcionalidades (Casos de Uso)

- Cadastrar Voluntário
- Agendar Colheita
- Registrar Colheita
- Controlar Estoque
- Realizar Doação
- Emitir Relatórios

### Relacionamentos entre casos de uso

| Tipo | Caso de uso base | Caso de uso relacionado | Regra |
|---|---|---|---|
| `<<include>>` (obrigatório) | Realizar Doação | Verificar Estoque | Garante a disponibilidade dos itens antes da conclusão da doação. |
| `<<extend>>` (opcional) | Agendar Colheita | Enviar Notificação | Ocorre quando há voluntários cadastrados para receber mensagens automáticas. |

---

## 🗂️ Modelo de dados (Diagrama de Classes)

| Classe | Atributos | Métodos | Relacionamentos |
|---|---|---|---|
| **Voluntário** | `idVoluntario`, `nome`, `telefone`, `disponibilidade` | `cadastrar()`, `atualizarDados()`, `confirmarParticipacao()` | **N:N com Colheita**, por meio da classe intermediária **Participação** (`horasTrabalhadas`, `funçãoExercida`) |
| **Horta** | `idHorta`, `localização`, `capacidadeProdução` | `gerenciarEstoque()`, `cadastrarColheita()` | **1:N com Colheita**: uma horta possui várias colheitas, e cada colheita pertence a uma única horta |
| **Colheita** | `data`, `quantidadeProduzida`, `status` | `registrarColheita()` | Ligada à Horta (1:N) e ao Voluntário (N:N) |
| **Doação** | `quantidade`, `destinatário`, `dataEntrega` | *a definir* | Registra a saída de itens e o histórico operacional das doações |
| **Estoque** | *a definir* | *a definir* | Entidade essencial para a validação de saídas e doações |

### Visão simplificada dos relacionamentos

```
Horta (1) ────────< (N) Colheita (N) >──── Participação ────< (N) Voluntário
                                   
Estoque ──── validado em ───▶ Doação
```

> **Pendências do modelo:** os atributos e métodos de **Estoque** e os métodos de **Doação** ainda não foram especificados no escopo.

---

## 🔄 Fluxo do processo: Realizar Doação (BPMN)

| Etapa | Elemento BPMN | Comportamento |
|---|---|---|
| **Início** | Evento inicial | O processo começa com o recebimento de uma solicitação de doação. |
| **Validação** | Tarefa de sistema | O sistema verifica automaticamente o estoque disponível. |
| **Decisão** | Gateway exclusivo (XOR) | **Estoque insuficiente:** o processo é encerrado com notificação de indisponibilidade.<br>**Estoque suficiente:** segue para a separação dos itens. |
| **Ações simultâneas** | Gateway paralelo (AND) | Em paralelo: atualização automática do estoque e envio de confirmação ao beneficiário. |
| **Fim** | Evento final | Entrega física da doação e registro histórico definitivo da operação no sistema. |
| **Intermediários** | Eventos intermediários | Representam notificações automáticas, confirmações de recebimento e validações internas. |

### Fluxo resumido

```
[Solicitação recebida]
        │
        ▼
[Verificar estoque] ──▶ ◇ XOR
                        ├─ Insuficiente ──▶ [Notificar indisponibilidade] ──▶ (Fim)
                        └─ Suficiente ────▶ [Separar itens]
                                                   │
                                                   ▼
                                                 ◇ AND
                                          ┌────────┴────────┐
                                          ▼                 ▼
                              [Atualizar estoque]  [Confirmar ao beneficiário]
                                          └────────┬────────┘
                                                   ▼
                                  [Entregar doação e registrar histórico] ──▶ (Fim)
```

---

## 📁 Estrutura do repositório

```
agrotech/
├── relatorio-escopo.html   # Relatório de escopo (visão geral, casos de uso, classes e BPMN)
└── README.md
```

---

## 🚀 Como visualizar

Não é necessário instalar nada.

```bash
git clone https://github.com/seu-usuario/agrotech.git
cd agrotech
# Abra o relatorio-escopo.html no navegador
```

Para gerar um PDF, abra o arquivo no navegador e use **Imprimir → Salvar como PDF**. As tabelas já estão configuradas para não quebrar entre páginas.

---

## 🗺️ Próximos passos

- [ ] Definir atributos e métodos da classe **Estoque** e os métodos de **Doação**
- [ ] Produzir os diagramas visuais (casos de uso, classes e BPMN)
- [ ] Definir requisitos funcionais e não funcionais detalhados
- [ ] Desenhar protótipos de tela para Administrador, Voluntário e Beneficiário
- [ ] Escolher a stack tecnológica e iniciar o desenvolvimento
- [ ] Implementar autenticação e controle de acesso por perfil

---

## 📄 Licença

Distribuído sob a licença MIT. Veja o arquivo `LICENSE` para mais informações.

---

<p align="center">Feito com 💚 para fortalecer as hortas e as comunidades que elas alimentam.</p>
