🏗️ UniCity-RP — Arquitetura do Sistema de Empregos
🎯 Objetivo

Definir a arquitetura oficial do sistema de empregos, progressão e economia do jogo, garantindo organização, escalabilidade e separação clara entre Client e Server.

📦 Estrutura Geral do Projeto
src/
├── server/
│   ├── services/
│   ├── modules/
│   └── datastore/
│
├── client/
│   ├── ui/
│   └── controllers/
│
└── shared/
    ├── config/
    └── types/

🔐 Separação de Responsabilidades
🖥️ Server (Autoridade Total)

Responsável por:

Persistência (DataStore)

Cálculo de XP

Cálculo de Level

Pagamento de salários

Validação anti-exploit

Registro de empregos

Controle de economia

⚠️ O Client nunca decide valores críticos.

🎮 Client

Responsável apenas por:

Interface (UI)

Feedback visual

Envio de requisições ao servidor

Atualização visual em tempo real

🧠 Fluxo de Dados
📌 Seleção de Emprego

Player abre UI

Player seleciona job

Client envia RemoteEvent: JobSelected

Server valida

Server salva currentJob

Server retorna confirmação

⭐ Sistema de XP

Emprego executa ação (ex: entrega concluída)

Server chama AddXP(player, jobId, amount)

Server calcula novo total

Se XP ≥ necessário:

Level++

Dispara evento LevelUp

Dados são salvos

💰 Sistema de Salários

Tarefa concluída

Server calcula:

Salário base

Bônus por level

Server adiciona dinheiro ao PlayerData

Server dispara evento de atualização

Dados são salvos

📊 Estrutura Oficial do PlayerData
PlayerData = {
    money = number,
    currentJob = string,

    jobs = {
        ["bus_driver"] = {
            xp = number,
            level = number
        },

        ["trucker"] = {
            xp = number,
            level = number
        },

        ["taxi"] = {
            xp = number,
            level = number
        }
    }
}


Essa estrutura permite:

Escalar para novos empregos

Separar progressão por job

Evitar conflitos de dados

Adicionar novas profissões sem refatoração estrutural

🔗 Sistema de Comunicação
RemoteEvents

JobSelected

AddXP

PaySalary

LevelUp

RemoteFunction

GetPlayerData

⚠️ Todas as validações devem acontecer no Server.

🛠️ Sistema Modular de Jobs

Cada emprego será registrado via:

RegisterJob(jobData)


Estrutura esperada:

jobData = {
    id = "bus_driver",
    displayName = "Motorista de Ônibus",
    maxLevel = 50,
    baseSalary = 100,
    xpPerTask = 25
}

📈 Escalabilidade Futura

Arquitetura preparada para:

Adicionar novos empregos

Sistema de veículos próprios

Sistema de upgrades por profissão

Sistema de ranking global

Sistema de eventos especiais

Sistema de empresas privadas

Sistema de bônus temporários

Sistema de conquistas por job

🔒 Segurança

Nunca confiar no Client

Validar todos os valores no Server

Proteger DataStore contra spam

Implementar cooldowns em RemoteEvents

Sanitizar entradas de dados

Bloquear requisições inválidas

🚀 Ordem Oficial de Implementação

Estrutura de Dados (#5)

DataStore (#6)

XP e Level (#7)

Salários (#8)

Comunicação (#9)

Registro de Jobs (#10)

UI Seleção (#11)

HUD (#12)

⚠️ Nenhum emprego MVP deve ser iniciado antes da conclusão total da Infraestrutura Base.

📌 Conclusão

Esta arquitetura define a base sólida do sistema de empregos do UniCity-RP.

Ela garante:

Organização

Segurança

Escalabilidade

Manutenção facilitada

Crescimento sustentável do projeto

