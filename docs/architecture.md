# 🏗️ UniCity-RP — Arquitetura do Sistema de Empregos

## 🎯 Objetivo

Definir a arquitetura oficial do sistema de empregos, progressão e economia do jogo,
garantindo organização, escalabilidade e separação clara entre Client e Server.

---

# 📦 Estrutura Geral do Projeto

src/
├── server/
│ ├── services/
│ ├── modules/
│ └── datastore/
│
├── client/
│ ├── ui/
│ └── controllers/
│
└── shared/
├── config/
└── types/


---

# 🔐 Separação de Responsabilidades

## 🖥️ Server (Autoridade Total)

Responsável por:

- Persistência (DataStore)
- Cálculo de XP
- Cálculo de Level
- Pagamento de salários
- Validação anti-exploit
- Registro de empregos
- Controle de economia

⚠️ O client nunca decide valores.

---

## 🎮 Client

Responsável apenas por:

- Interface (UI)
- Feedback visual
- Envio de requisições ao servidor
- Atualização visual em tempo real

---

# 🧠 Fluxo de Dados

## 📌 Seleção de Emprego

1. Player abre UI
2. Player seleciona job
3. Client envia `RemoteEvent: JobSelected`
4. Server valida
5. Server salva Job atual
6. Server retorna confirmação

---

## ⭐ Sistema de XP

1. Emprego executa ação (ex: entrega concluída)
2. Server chama `AddXP(player, jobId, amount)`
3. Server calcula novo total
4. Se XP ≥ necessário:
   - Level++
   - Dispara evento `LevelUp`
5. Dados são salvos

---

## 💰 Sistema de Salários

1. Tarefa concluída
2. Server calcula:
   - Salário base
   - Bônus por level
3. Server adiciona dinheiro ao PlayerData
4. Server dispara evento de atualização
5. Dados são salvos

---

# 📊 Estrutura Oficial do PlayerData

```lua
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
