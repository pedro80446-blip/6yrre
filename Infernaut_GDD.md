# Game Design Document — "Infernaut"

**Versão do build:** alpha-0.78
**Engine:** Godot 4.7 (GL Compatibility)
**Plataformas:** PC (teclado/mouse/joystick) e Mobile (controles de toque)

---

## 1. Descrição Geral

Infernaut é um jogo estilo **Vampire Survivors misturado com sidescroller**. Ondas de inimigos aparecem e devem ser derrotadas em um sistema de ondas que avança progressivamente até o confronto com o chefão.

---

## 2. História

Fireman acorda em uma floresta no meio do nada, que está em chamas. Ele é bombeiro, mas não lembra de hoje ter vestido sua farda. Desnorteado, percebe que está rodeado de caçadores, chamas vivas e outros inimigos. O objetivo é sobreviver a todo custo.

---

## 3. Objetivo da Fase 1
Sobreviver a 6 ondas de inimigos e derrotar o boss na onda 6.
- **Vitória:** "FASE 1 CONCLUÍDA!"
- **Derrota:** vida do jogador chega a zero → "VOCÊ MORREU!"

---

## 4. Controles

| Ação | Input |
|---|---|
| Mover | OS direcionais do teclado ou botões touch(dependendo da plataforma) |
| Pular | Space ou botão touch pular | 
| Atirar água | botão touch atirar / clique esquerdo / botão 2 do joystick |
| Defender | botão touch defender / clique direito / botão 3 do joystick |

Controles de toque aparecem automaticamente em dispositivos touchscreen.

---

## 5. Jogador — Fireman-Bombeiro

Design:Um bombeiro de cabelo curto e barbudo,farda azul e amarela e com uma arma d'água.
"Imagem do Fireman"

**Ações:** mover, pular, atirar água (desabilitado ao defender), defender (imobiliza o jogador, bloqueia 80% de qualquer dano — incluindo queimadura e stomp do boss — e reduz o knockback recebido para 20%).

**Dano/morte:** ao levar dano, entra em invencibilidade temporária (exceto dano de queimadura) e sofre knockback.

**Animações:** idle, run, jump, shoot, shooted, burning, defend, smashed, dying.

---

## 6. Projétil do Jogador (água)

| Atributo | Valor |
|---|---|
| Velocidade | 700.0 |
| Dano | 1 |
| Tempo de vida | 1.2s |

Some ao sair da tela ou ao colidir com inimigo/chão. A mesma cena de projétil é reaproveitada para o chumbo dos caçadores, em versão hostil (cor diferente, colide com o jogador).

---

## 7. Sistema de Ondas

| Onda | Meta |
|---|---|
| 1 | 5 inimigos |
| 2 | 6 inimigos |
| 3 | 7 inimigos |
| 4 | 8 inimigos |
| 5 | 9 inimigos |
| 6 | 1 (boss) |

- **Ondas 1–3:** apenas inimigos de fogo,os Chamas vivas.
"imagem do chama viva"
- **Onda 4+:** Chama viva + caçadores (45% de chance de spawn de caçador).
"Imagem do caçador"
- **Onda 6:** apenas o boss, precedido de um som de aviso.
"imagem do boss"
 
## 8. Inimigos

### Chama viva
| Atributo | Valor |
|---|---|
| Velocidade | 100.0 |
| Vida | 2 |
| Dano de contato | 1 |
| Propagação | a cada 4–8s, raio 140px |

Persegue o jogador no eixo horizontal, ataca por contato e se propaga criando clones (respeitando o limite de 12 incêndios).

"Imagem do chama viva"

### Caçador (a partir da onda 4)
| Atributo | Valor |
|---|---|
| Velocidade | 90.0 |
| Vida | 3 |
| Distância preferida | 420.0 |
| Cooldown de disparo | 2.2s |
| Chumbos por disparo | 1 (dispersão 14°) |

Mantém distância ideal do jogador e atira com escopeta, sincronizado ao frame 10 da animação de ataque. Só atira se estiver na tela.
"Imagem do caçador"


### Boss (Fase 1)
"Um homem gigantesco e forte."

| Atributo | Valor |
|---|---|
| Vida | 60 |
| Velocidade | 60.0 |
| Alcance do stomp | 220.0 |
| Dano do stomp | 2 |
| Dano da granada | 2 |
| Duração da queimadura (molotov) | 3.0s |

"Imagem do boss"

**Ataques (sorteados quando fora do alcance de stomp; dentro do alcance sempre usa stomp):**
- **Stomp:** dano em área ao redor do boss.
- **Granada:** arco até o jogador, explode em 1.4s, dano em área (raio 130).
- **Molotov:** arco até o jogador, explode em 1.1s, cria poça de fogo (raio 90, dura 4s) que incendeia o jogador.

Solta uma fala de voz única em algum momento aleatório da luta (entre 4–18s). Derrotá-lo conclui a Fase 1.

---

## 9. HUD

- Barra de vida do jogador.
- Indicador de onda ("ONDA X" / "ONDA 6 - CHEFE!") e barra de progresso da onda.
  "Ilustração de exemplo"
- Pausa com opções de volume (Master/Música/SFX) — música fica "abafada" durante a pausa.
"imagem de exemplo"
- Tela de fim de jogo (vitória/derrota) com opção de reiniciar ou voltar ao menu.
- "Imagem de exemplo"
- Controles de toque só em dispositivos móveis.

---

## 10. Áudio

- Buses: Music e SFX.
- Músicas: menu, fase, boss (troca automática ao spawnar o chefe).
- SFX: pulo, tiro de água, tiro de escopeta, dano/morte de inimigos, dano/morte do boss, dano/morte do jogador, vitória, derrota, aviso de boss.
- Volumes salvos em disco entre sessões.

---

## 11. Menu Principal

Música própria em loop. Botão **Jogar** (inicia a fase e reseta o estado) e botão **Sair**.
Tem um fundo com partículas de chamas animadas.

"imagem ilustrativa"

---

## 12. Estrutura de Cenas

Onda 1-3 :Fundo em chamas,árvores secas.
Onda 4-5 :Chamas apagadas,porém fundo mais escuro,árvores secas.
Onda 6(Chefe):Folhagem viva,fundo de uma floresta,árvores vivas.

---

## 13. Assets Confirmados

- **Bombeiro:** idle, run, jump, shoot, shooted, burning, defend, smashed, dying.
- **Boss:** idle;stomp, throw, die, hit.
- **Caçador:** idle, run, attack.
- **Fogo:** idle.
- **Ambiente:** nuvens, corvos, árvores, background de fase, jato de água, botões de toque.
- **UI:** botões de menu/pausa/opções, fontes, spritesheet de fogo animado do título.
