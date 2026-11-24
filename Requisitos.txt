# Documento de Requisitos — Biblioteca Cultural

## 1. Objetivo do Sistema
O sistema **Biblioteca Cultural** é uma aplicação desktop desenvolvida para gerenciar o acervo de materiais de leitura da instituição (livros, mangás e histórias em quadrinhos - HQs). O objetivo principal é permitir o controle organizado dos itens cadastrados, facilitar a busca de materiais e calcular automaticamente os prazos de devolução conforme o tipo de obra.

## 2. Público-Alvo (Usuários do Sistema)
* **Atendentes / Bibliotecários:** Responsáveis por cadastrar novos materiais, atualizar informações, remover itens danificados ou extraviados, consultar o acervo e verificar prazos de devolução.

## 3. Funcionalidades Principais
O sistema contempla exclusivamente as seguintes operações:
* Cadastro de novos materiais no acervo (Livros, Mangás e HQs);
* Consulta geral (listagem) de todos os materiais cadastrados;
* Pesquisa de materiais específicos no acervo;
* Atualização de dados de materiais já existentes;
* Remoção de materiais do acervo;
* Cálculo do prazo de devolução com base nas regras de cada tipo de material.

## 4. Tecnologias Utilizadas
* **Linguagem de Programação:** Java;
* **Interface Gráfica:** JavaFX;
* **Ambiente de Desenvolvimento (IDE):** BlueJ;
* **Padrão Arquitetural:** MVC (Model-View-Controller);
* **Armazenamento de Dados:** Em memória, utilizando a estrutura `ArrayList`.

## 5. Limitações do Sistema
Por se tratar de um projeto acadêmico focado nos conceitos de Programação Orientada a Objetos, o sistema possui as seguintes restrições de escopo:
* **Persistência em memória:** Os dados são armazenados apenas em `ArrayList` durante a execução do programa. Não há integração com banco de dados relacional ou salvamento automático em arquivos externos (os dados são perdidos ao fechar o sistema);
* **Sem controle de usuários/login:** Não existe tela de autenticação ou perfis de acesso distintos;
* **Sem histórico completo de empréstimos:** O sistema realiza o cálculo do prazo de devolução para os itens, mas não gerencia cadastro de leitores, multas financeiras ou reservas online;
* **Execução local:** Aplicação desktop monoposto, sem comunicação em rede ou acesso via navegador.

---

## 6. Requisitos Funcionais (RF)

| Código | Requisito | Descrição |
| :--- | :--- | :--- |
| **RF01** | Cadastrar Material | O sistema deve permitir o cadastro de **Livros**, **Mangás** e **HQs**, armazenando seus atributos básicos e específicos. |
| **RF02** | Consultar Acervo | O sistema deve listar todos os itens atualmente armazenados no acervo. |
| **RF03** | Pesquisar Material | O sistema deve permitir buscar materiais no acervo (por exemplo, por título ou código identificador). |
| **RF04** | Atualizar Material | O sistema deve permitir editar as informações de um material previamente cadastrado. |
| **RF05** | Remover Material | O sistema deve permitir excluir um material do acervo. |
| **RF06** | Calcular Prazo de Devolução | O sistema deve calcular a data ou quantidade de dias de devolução de acordo com a categoria do item (`Livro` ou `MangaHQs`). |

---

## 7. Requisitos Não Funcionais (RNF)

| Código | Requisito | Descrição |
| :--- | :--- | :--- |
| **RNF01** | Linguagem e Plataforma | O sistema deve ser implementado em **Java** e executado como aplicação desktop. |
| **RNF02** | Interface Gráfica | A interface visual deve ser construída utilizando **JavaFX**. |
| **RNF03** | Compatibilidade de IDE | A estrutura do projeto deve ser compatível com abertura, compilação e execução no **BlueJ**. |
| **RNF04** | Arquitetura MVC | O código deve separar responsabilidades entre Model (`ItemAcervo`, `Livro`, `MangaHQs`, `Acervo`), View (`BibliotecaView`) e Controller (`BibliotecaController`). |
| **RNF05** | Estrutura de Dados | O gerenciamento dinâmico dos itens em memória deve utilizar exclusivamente `java.util.ArrayList`. |
| **RNF06** | Aplicação de Conceitos POO | O modelo deve aplicar encapsulamento (atributos privados com getters/setters), herança e polimorfismo no cálculo de prazos. |
