@startuml
skinparam classAttributeIconSize 0
skinparam monochrome true

package "model" {
    abstract class ItemAcervo {
        - id : int
        - titulo : String
        - autor : String
        - anoPublicacao : int
        - disponivel : boolean
        + ItemAcervo(id : int, titulo : String, autor : String, anoPublicacao : int)
        + {abstract} calcularPrazoDevolucao() : int
        + getId() : int
        + getTitulo() : String
        + setTitulo(titulo : String) : void
        + getAutor() : String
        + setAutor(autor : String) : void
        + getAnoPublicacao() : int
        + setAnoPublicacao(anoPublicacao : int) : void
        + isDisponivel() : boolean
        + setDisponivel(disponivel : boolean) : void
    }

    class Livro {
        - isbn : String
        - numeroPaginas : int
        + Livro(id : int, titulo : String, autor : String, anoPublicacao : int, isbn : String, numeroPaginas : int)
        + calcularPrazoDevolucao() : int
        + getIsbn() : String
        + setIsbn(isbn : String) : void
        + getNumeroPaginas() : int
        + setNumeroPaginas(numeroPaginas : int) : void
    }

    class MangaHQs {
        - volume : int
        - editora : String
        - tipo : String
        + MangaHQs(id : int, titulo : String, autor : String, anoPublicacao : int, volume : int, editora : String, tipo : String)
        + calcularPrazoDevolucao() : int
        + getVolume() : int
        + setVolume(volume : int) : void
        + getEditora() : String
        + setEditora(editora : String) : void
        + getTipo() : String
        + setTipo(tipo : String) : void
    }

    class Acervo {
        - itens : ArrayList<ItemAcervo>
        + Acervo()
        + adicionarItem(item : ItemAcervo) : void
        + listarTodos() : ArrayList<ItemAcervo>
        + buscarPorTitulo(titulo : String) : ArrayList<ItemAcervo>
        + buscarPorId(id : int) : ItemAcervo
        + atualizarItem(item : ItemAcervo) : boolean
        + removerItem(id : int) : boolean
    }
}

package "controller" {
    class BibliotecaController {
        - acervo : Acervo
        + BibliotecaController()
        + cadastrarLivro(id : int, titulo : String, autor : String, ano : int, isbn : String, paginas : int) : void
        + cadastrarMangaHQ(id : int, titulo : String, autor : String, ano : int, volume : int, editora : String, tipo : String) : void
        + consultarAcervo() : ArrayList<ItemAcervo>
        + pesquisarMaterial(termo : String) : ArrayList<ItemAcervo>
        + atualizarMaterial(item : ItemAcervo) : boolean
        + removerMaterial(id : int) : boolean
        + calcularPrazo(id : int) : int
    }
}

package "view" {
    class BibliotecaView {
        - controller : BibliotecaController
        + BibliotecaView(controller : BibliotecaController)
        + iniciarTela(stage : Stage) : void
    }
}

class Main {
    + {static} main(args : String[]) : void
}

' Relacionamentos
ItemAcervo <|-- Livro : herda
ItemAcervo <|-- MangaHQs : herda
Acervo "1" o-- "0..*" ItemAcervo : armazena >
BibliotecaController "1" --> "1" Acervo : gerencia >
BibliotecaView "1" --> "1" BibliotecaController : solicita operações >
Main ..> BibliotecaController : instancia
Main ..> BibliotecaView : inicia

@enduml
