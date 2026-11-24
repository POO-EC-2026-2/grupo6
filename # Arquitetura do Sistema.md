# Arquitetura do Sistema — Biblioteca Cultural

## 1. Visão Geral do Padrão MVC
A **Biblioteca Cultural** segue o padrão arquitetural **MVC (Model-View-Controller)** para separar a representação dos dados e regras de negócio, a interface gráfica em JavaFX e o controle das ações do usuário.

* **Model (`ItemAcervo`, `Livro`, `MangaHQs`, `Acervo`):** Representa os dados do domínio e as regras de negócio (cadastro/consulta/atualização/remoção em memória e cálculo do prazo de devolução). Não depende de JavaFX nem conhece a `BibliotecaView`.
* **View (`BibliotecaView`):** Camada de apresentação construída com **JavaFX**. Exibe formulários, listas/tabelas e mensagens, delegando as ações do usuário ao `BibliotecaController`.
* **Controller (`BibliotecaController`):** Camada intermediária que recebe os pedidos da `BibliotecaView`, valida entradas básicas quando necessário, aciona o `Acervo` e devolve os resultados para a interface exibir.
* **Ponto de Entrada (`Main`):** Inicializa a aplicação JavaFX, instancia `BibliotecaController` e `BibliotecaView` e abre a janela principal.

---

## 2. Camada Model (Classes e Regras de Negócio)

### 2.1. `ItemAcervo` (Classe Abstrata)
* **Responsabilidade:** Define os atributos e comportamentos comuns a qualquer material do acervo, servindo como superclasse para `Livro` e `MangaHQs`.
* **Atributos principais:** `id`, `titulo`, `autor`, `anoPublicacao`, `disponivel` (encapsulados como `private`, acessados via getters/setters).
* **Regras de negócio:**
  * Declara o método abstrato `calcularPrazoDevolucao()`, obrigando cada subclasse concreta a definir sua própria regra de prazo.

### 2.2. `Livro` e `MangaHQs` (Subclasses Concretas)
* **Responsabilidade:** Especializam `ItemAcervo` com dados específicos de cada tipo de material e implementam `calcularPrazoDevolucao()` (**polimorfismo**).
* **`Livro`:** Adiciona atributos como `isbn` e `numeroPaginas`.
* **`MangaHQs`:** Adiciona atributos como `volume`, `editora` e `tipo` (mangá ou HQ).
* **Regra de prazo de devolução:** Cada classe sobrescreve `calcularPrazoDevolucao()` retornando a quantidade de dias definida pela equipe (a ser confirmada com Uriel e Marcos).

### 2.3. `Acervo`
* **Responsabilidade:** Gerencia a coleção de materiais em memória usando `ArrayList<ItemAcervo>`.
* **Regras de negócio:**
  * Armazena `Livro` e `MangaHQs` na mesma lista por meio de polimorfismo (`ItemAcervo`).
  * Executa as operações de adicionar, listar todos, buscar (por título ou ID), atualizar dados de um item existente e remover por ID.
  * Evita duplicidade de ID (caso o ID seja único no acervo).

---

## 3. Camada View (`BibliotecaView` — JavaFX)

* **Responsabilidade:** Construir e gerenciar a interface gráfica em JavaFX (`Stage`, `Scene`, layouts, campos de texto, botões e visualização dos itens).
* **Comunicação com o Controller:**
  * Mantém uma referência para `BibliotecaController`.
  * Nos eventos de clique dos botões (cadastrar, pesquisar, atualizar, remover, calcular prazo), lê os valores dos campos da tela e chama os métodos correspondentes no `BibliotecaController`.
  * Recebe o retorno do `BibliotecaController` (lista de itens, sucesso/falha ou número de dias do prazo) e atualiza a tela ou exibe alertas (`Alert`) ao usuário.
* **Restrição:** A `BibliotecaView` **não** manipula diretamente o `ArrayList` do `Acervo` nem contém regras de cálculo de prazo.

---

## 4. Camada Controller (`BibliotecaController`)

* **Responsabilidade:** Coordenar o fluxo entre a `BibliotecaView` e o `Acervo`.
* **Comunicação com a View e o Model:**
  * Recebe dados primitivos ou valores enviados pela `BibliotecaView` (ex.: título, autor, ano, campos específicos de Livro ou Mangá/HQ, ID ou termo de busca).
  * Instancia o objeto concreto (`Livro` ou `MangaHQs`) e solicita ao `Acervo` que o armazene, consulte, atualize ou remova.
  * Localiza o item pelo ID e invoca `calcularPrazoDevolucao()` para retornar o prazo à View.
  * Trata exceções simples (como formato numérico inválido ou item não encontrado) para que a View mostre mensagens claras sem quebrar a aplicação.

---

## 5. Fluxo de Comunicação (Visão Geral)

```text
[Usuário] --> [BibliotecaView (JavaFX)] --> [BibliotecaController] --> [Acervo (ArrayList<ItemAcervo>)]
                    ^                           |                             |
                    |                           +---- retorna dados/status ---+
                    +--- atualiza tela / alerta <---+
