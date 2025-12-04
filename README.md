# Ponderada HASH - Semana 08 (Em Sala)

## Exercício 2 — Versão de Firmware do CBTC

**Dados:**
* Função Hash: $H(x,y) = (x \oplus y) \oplus (3x \pmod{16})$
* $x = 1010$ (valor decimal 10)
* $y = 0110$ (valor decimal 6)

### 1. Calcule H(x,y)

**Cálculo:**

1.  Primeiro, calculamos o XOR entre $x$ e $y$:
    $$1010 \oplus 0110 = 1100$$
2.  Depois, calculamos o termo da direita $(3 \times 10 \pmod{16})$:
    $$3 \times 10 = 30$$
    $$30 = 16 + 14 \implies 30 \pmod{16} = 14$$
    Em binário, $14 = 1110$.
3.  Por fim, fazemos o XOR dos dois resultados:
    $$1100 \oplus 1110 = 0010$$

> **[INSERIR AQUI A FOTO 20251204_152139.jpg (Parte esquerda - item a)]**

**Resposta:** O valor de $H(x,y)$ é **0010**.

---

### 2. Se um ataque troca apenas o segundo nibble para y’ = 0111, o valor de H muda?

**Cálculo com $y' = 0111$:**

1.  Novo XOR entre $x$ e $y'$:
    $$1010 \oplus 0111 = 1101$$
2.  O termo da direita permanece o mesmo (pois depende apenas de $x$):
    $$3x \pmod{16} = 1110$$
3.  Novo cálculo final:
    $$1101 \oplus 1110 = 0011$$

> **[INSERIR AQUI A FOTO 20251204_152139.jpg (Parte direita - item b)]**

**Resposta:** Sim, o valor de $H$ muda (de `0010` para `0011`).

---

## Exercício 3 — Detecção de Fraude em Bilhetagem Eletrônica

**Dados:**
* Função Hash: $H(ID, t) = (ID \oplus t) \pmod{256}$
* $ID = 11001010$
* $Tempo (t) = 00110101$

### 1. Calcule o hash

**Cálculo:**

Realizando a operação XOR bit a bit:
$$11001010 \oplus 00110101 = 11111111$$

Convertendo para decimal:
$$11111111_2 = 255_{10}$$
$$(2^8 - 1) \pmod{256} = 255$$

> **[INSERIR AQUI A FOTO 20251204_152145.jpg (Parte I)]**

**Resposta:** O hash é **11111111** (ou 255 em decimal).

---

### 2 e 3. O atacante tenta alterar Tempo para 00110100. O hash permanece igual?

**Cálculo com novo tempo $t' = 00110100$:**

Realizando o novo XOR:
$$11001010 \oplus 00110100 = 11111110$$

Convertendo para decimal:
$$11111110_2 = 254_{10}$$
$$(2^8 - 2) \pmod{256} = 254$$

Comparação:
* Hash original: **255** (`11111111`)
* Novo Hash: **254** (`11111110`)

> **[INSERIR AQUI A FOTO 20251204_152123.jpg (Parte III)]**

**Resposta:** Não, o hash não permanece igual. Ele altera de 255 para 254.

---

### 4. Explique: por que esse tipo de "hash fraco" é inseguro?

**Resposta:**
Este hash é considerado inseguro e fraco por três motivos principais:

1.  **Linearidade e Falta de Difusão (Efeito Avalanche):** Em um bom hash criptográfico, mudar apenas 1 bit na entrada deveria alterar drasticamente o resultado (cerca de 50% dos bits de saída). Neste caso, vimos que alterar 1 bit no Tempo resultou na alteração de apenas 1 bit no Hash final. Isso torna o comportamento previsível.
2.  **Reversibilidade (Preimage Attack):** Como a operação principal é um XOR simples, é trivial para um atacante falsificar um ID. Se o atacante quer gerar um Hash específico num determinado Tempo, ele pode calcular facilmente o ID necessário fazendo: $ID = Hash \oplus Tempo$.
3.  **Facilidade de Colisão:** É muito fácil encontrar dois pares diferentes de $(ID, Tempo)$ que gerem o mesmo resultado, o que quebra a integridade que o sistema tenta garantir.