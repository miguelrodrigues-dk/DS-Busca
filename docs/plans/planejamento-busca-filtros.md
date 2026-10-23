# Planejamento de Implementação — Feature: Busca de Títulos (Fase 1)

**Plataforma:** MOLTEN — Streaming & Avaliador de Filmes  
**Tecnologia:** ASP.NET Core MVC, Entity Framework Core e SQLite  
**Histórico:** Documento de Planejamento referente à 8ª Entrega Oficial do Projeto  
**Status:** Busca Textual Concluída / Filtros Avançados Mapeados  

---

## 1. Escopo, Contexto e Objetivos
O objetivo desta 8ª entrega foi planejar e implementar o sistema de localização rápida de títulos no catálogo da plataforma de streaming **MOLTEN**. A feature permite que os usuários realizem pesquisas dinâmicas de filmes diretamente no banco de dados SQLite por meio de termos de texto, otimizando a navegação na interface de listagem.

### Componentes Atualizados nesta Entrega:
- **`Controllers/FilmesController.cs`**: Inclusão do parâmetro de string e lógica LINQ para busca por texto.
- **`Views/Filmes/Index.cshtml`**: Integração da barra de pesquisa com envio de dados para o servidor.

---

## 2. Abordagem Técnico-Operacional e Sequência de Implementação

### Passo 1: Captura do Termo de Busca
A Action `Index` do `FilmesController` foi desenhada para receber o parâmetro opcional de texto diretamente da Query String da URL (ex: `/Filmes?busca=Star+Wars`):
- `string? busca`: Termo enviado pelo usuário para varredura de dados na plataforma.

### Passo 2: Construção da Query de Busca por Texto
A consulta utiliza `IQueryable<Filme>` combinada com cláusulas condicionais do Entity Framework Core. Se a string `busca` contiver caracteres, o sistema aplica uma varredura acumulativa:
- O filtro valida se o termo está contido no **Título** OU na **Descrição (Sinopse)** do filme.
- A tradução do `.Contains()` para o banco de dados SQLite opera de forma case-insensitive nativamente, garantindo que buscas por letras maiúsculas ou minúsculas tragam o mesmo resultado.

### Passo 3: Situação Atual da Interface e Próximos Passos
Nesta etapa de desenvolvimento da plataforma **MOLTEN**, o mecanismo principal de **Barra de Busca** encontra-se totalmente funcional e integrado ao banco de dados. Os elementos visuais do componente `<select>` de Status/Categoria foram posicionados no layout HTML da View para fins de estruturação e design da interface. 

O mapeamento da lógica de persistência e o acoplamento do filtro do componente `Select` à query LINQ do controller ficaram definidos no cronograma técnico como prioridade inicial para a próxima sprint de refinamento do catálogo.

---

## 3. Roteiro de Validação, Dependências e Critérios de Conclusão
- **Cenário 1 (Busca por Título):** Digitar uma palavra contida no título de um filme e validar se a listagem isola o registro correto.
- **Cenário 2 (Busca por Sinopse):** Pesquisar termos específicos contidos apenas na descrição/sinopse do filme para checar a abrangência do filtro textual.
- **Cenário 3 (Retorno Vazio):** Certificar que o sistema responde de maneira estável quando termos sem correspondência são submetidos.
- **Garantia de Regressão:** O fluxo de criação de filmes (`Create`) e exibição de detalhes (`Detalhes`) permanece inalterado e seguro contra falhas.
