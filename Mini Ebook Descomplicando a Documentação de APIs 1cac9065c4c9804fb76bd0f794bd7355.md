# Mini Ebook: Descomplicando a Documentação de APIs com Swagger (Foco em Maven)

Criado em: 3 de abril de 2025 02:23

## Mini Ebook: Descomplicando a Documentação de APIs com Swagger (Foco em Maven)

**Bem-vindo!**

Se você é um desenvolvedor de APIs Java utilizando o Maven como ferramenta de gerenciamento de projetos, sabe a importância de uma documentação clara e concisa. Ela não apenas facilita o uso da sua API por outros desenvolvedores, mas também agiliza a integração e reduz a margem para erros. É aí que o Swagger (agora conhecido como OpenAPI Specification) entra em cena como uma ferramenta poderosa para descrever e visualizar suas APIs de forma padronizada e interativa, e o Maven pode simplificar a integração dessas ferramentas no seu fluxo de trabalho.

Este mini ebook foi criado para te guiar pelos principais conceitos e passos para começar a documentar suas APIs Java de forma eficaz com Swagger, integrando-o ao seu projeto Maven. Prepare-se para elevar o nível da sua documentação!

**O Que Você Vai Aprender:**

- **O que é o Swagger/OpenAPI Specification?** Entenda o conceito e os benefícios de usar essa especificação.
- **Integração do Swagger com Projetos Maven:** Descubra como adicionar as dependências necessárias.
- **Principais Componentes da Especificação OpenAPI:** Explore a estrutura fundamental de um documento Swagger.
- **Como Descrever seus Endpoints (Anotações Java):** Detalhe caminhos, métodos HTTP, parâmetros e respostas diretamente no seu código Java.
- **Definindo Modelos de Dados (Schemas) em Java:** Estruture os dados de requisição e resposta utilizando suas classes Java.
- **Adicionando Informações Adicionais (Anotações):** Inclua segurança, tags e descrições utilizando anotações específicas.
- **Gerando a Documentação Swagger com Maven:** Configure plugins Maven para gerar o arquivo de especificação OpenAPI e a interface Swagger UI.
- **Práticas Recomendadas em um Contexto Maven:** Dicas para criar uma documentação de API de alta qualidade integrada ao seu projeto Maven.

**Capítulo 1: Entendendo o Swagger/OpenAPI Specification**

Originalmente criado como o "Swagger Specification", o formato evoluiu e agora é mantido como um projeto aberto chamado **OpenAPI Specification**. No entanto, o termo "Swagger" ainda é amplamente utilizado para se referir à especificação e ao ecossistema de ferramentas que a cercam.

**Em essência, a OpenAPI Specification é uma linguagem de descrição para APIs RESTful.** Ela permite que você defina a estrutura da sua API de uma forma legível tanto para humanos quanto para máquinas. Isso significa que, com um arquivo de especificação OpenAPI, é possível:

- **Gerar automaticamente documentação interativa:** A famosa interface do Swagger UI.
- **Gerar código cliente (SDKs) em diversas linguagens:** Facilitando a integração com sua API.
- **Validar requisições e respostas:** Garantindo a conformidade com a especificação.
- **Criar mocks de API:** Para testes e desenvolvimento em paralelo.

**Benefícios de Usar OpenAPI:**

- **Clareza e Consistência:** Padroniza a forma como sua API é descrita.
- **Melhora a Colaboração:** Facilita a comunicação entre equipes de desenvolvimento, testes e documentação.
- **Acelera a Integração:** Desenvolvedores externos podem entender e usar sua API mais rapidamente.
- **Reduz Erros:** A documentação precisa diminui a probabilidade de erros de integração.
- **Melhora a Experiência do Desenvolvedor:** Uma boa documentação torna sua API mais atraente e fácil de usar.
- **Integração Simplificada com Maven:** O Maven facilita a adição e o gerenciamento das bibliotecas Swagger.

**Capítulo 2: Integração do Swagger com Projetos Maven**

Para utilizar o Swagger em seu projeto Java com Maven, você precisará adicionar as dependências apropriadas ao seu arquivo `pom.xml`. Uma biblioteca popular para isso é o **Springfox**, que oferece integração facilitada com aplicações Spring (Spring MVC e Spring Boot).

**Exemplo de Dependências Springfox no `pom.xml`:**

XML

`<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version> </dependency>`

**Observações Importantes:**

- **Versão:** Verifique a versão mais recente do `springfox-boot-starter` (ou outra biblioteca Swagger que você escolher) no Maven Repository e utilize-a.
- **Spring Boot:** O `springfox-boot-starter` é ideal para projetos Spring Boot. Se você estiver usando apenas Spring MVC, pode precisar de dependências ligeiramente diferentes (como `springfox-swagger2` e `springfox-swagger-ui`).
- **Outras Bibliotecas:** Existem outras bibliotecas para integrar Swagger com Java e Maven, como o Swagger Core diretamente ou frameworks como o Jakarta EE com suas próprias extensões. A escolha dependerá do seu ambiente de desenvolvimento.

Após adicionar a dependência, o Maven irá baixar e gerenciar as bibliotecas necessárias para o seu projeto.

**Capítulo 3: Principais Componentes da Especificação OpenAPI**

Assim como mencionado anteriormente, um documento OpenAPI possui componentes chave como `openapi`, `info`, `servers`, `paths` e `components`. Ao usar bibliotecas como o Springfox, essas informações são geralmente geradas a partir das suas configurações e anotações no código Java.

**Capítulo 4: Como Descrever seus Endpoints (Anotações Java)**

Com bibliotecas como o Springfox, a documentação da sua API é gerada a partir de anotações diretamente no seu código Java (controladores REST).

**Exemplo de Anotações Springfox:**

Java

`import io.swagger.annotations.*;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/usuarios")
@Api(tags = "Usuários", description = "Operações relacionadas a usuários")
public class UsuarioController {

    @ApiOperation(value = "Lista todos os usuários", response = Usuario.class, responseContainer = "List")
    @GetMapping
    public List<Usuario> listarUsuarios() {
        // ... lógica para listar usuários
        return new ArrayList<>();
    }

    @ApiOperation(value = "Cria um novo usuário", response = Usuario.class)
    @ApiResponses(value = {
            @ApiResponse(code = 201, message = "Usuário criado com sucesso"),
            @ApiResponse(code = 400, message = "Requisição inválida")
    })
    @PostMapping
    public ResponseEntity<Usuario> criarUsuario(@ApiParam(value = "Dados do novo usuário", required = true) @RequestBody NovoUsuario novoUsuario) {
        // ... lógica para criar usuário
        return new ResponseEntity<>(HttpStatus.CREATED);
    }

    @ApiModel(description = "Representa um usuário")
    public static class Usuario {
        @ApiModelProperty(notes = "Identificador único do usuário", readOnly = true)
        private Long id;
        @ApiModelProperty(notes = "Nome completo do usuário")
        private String nome;
        @ApiModelProperty(notes = "Endereço de email do usuário")
        private String email;

        // Getters e Setters
    }

    @ApiModel(description = "Dados para criar um novo usuário")
    public static class NovoUsuario {
        @ApiModelProperty(notes = "Nome completo do usuário", required = true)
        private String nome;
        @ApiModelProperty(notes = "Endereço de email do usuário", required = true)
        private String email;

        // Getters e Setters
    }
}`

**Principais Anotações Springfox:**

- **`@Api`:** Nível de classe, define metadados para o controlador (tags, descrição).
- **`@ApiOperation`:** Nível de método, descreve a operação do endpoint (valor, resposta, etc.).
- **`@ApiParam`:** Nível de parâmetro, descreve um parâmetro de requisição (`@PathVariable`, `@RequestParam`, `@RequestBody`).
- **`@ApiResponses` e `@ApiResponse`:** Nível de método, define as possíveis respostas HTTP do endpoint.
- **`@ApiModel`:** Nível de classe (geralmente DTOs/Entidades), descreve um modelo de dados (schema).
- **`@ApiModelProperty`:** Nível de campo dentro de um `@ApiModel`, descreve uma propriedade do modelo de dados.

**Capítulo 5: Definindo Modelos de Dados (Schemas) em Java**

Como visto no exemplo acima, seus modelos de dados (as classes Java utilizadas para requisição e resposta) são automaticamente convertidos em schemas OpenAPI através das anotações Springfox (`@ApiModel` e `@ApiModelProperty`). Isso simplifica a definição da estrutura dos dados.

**Capítulo 6: Adicionando Informações Adicionais (Anotações)**

Você pode adicionar informações adicionais como segurança e tags utilizando mais anotações Springfox ou configurando beans específicos.

**Exemplo de Configuração de Segurança (Springfox):**

Java

`import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.*;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spi.service.contexts.SecurityContext;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

@Configuration
@EnableSwagger2
public class SwaggerConfig {

    private ApiKey apiKey() {
        return new ApiKey("Bearer", "Authorization", "header");
    }

    private SecurityContext securityContext() {
        return SecurityContext.builder().securityReferences(defaultAuth()).build();
    }

    private List<SecurityReference> defaultAuth() {
        AuthorizationScope authorizationScope = new AuthorizationScope("global", "accessEverything");
        AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
        authorizationScopes[0] = authorizationScope;
        return Arrays.asList(new SecurityReference("Bearer", authorizationScopes));
    }

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .securityContexts(Arrays.asList(securityContext()))
                .securitySchemes(Arrays.asList(apiKey()))
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.exemplo.seuprojeto.controller")) // Substitua pelo seu pacote de controllers
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfo(
                "Minha Incrível API",
                "Uma API simples para demonstração.",
                "v1.0.0",
                "Termos de serviço",
                new Contact("Equipe de Desenvolvimento", "www.exemplo.com", "suporte@exemplo.com"),
                "Licença da API",
                "URL da licença",
                Collections.emptyList()
        );
    }
}`

Esta configuração define um esquema de segurança "Bearer" (JWT) e o aplica a todos os endpoints da API.

**Capítulo 7: Gerando a Documentação Swagger com Maven**

Com o Springfox (ou outra biblioteca similar), a documentação OpenAPI é geralmente gerada dinamicamente quando sua aplicação Spring Boot é executada. O Springfox expõe um endpoint (por padrão `/v2/api-docs`) que retorna o arquivo de especificação OpenAPI no formato JSON.

Para visualizar essa documentação de forma interativa, você pode adicionar o Swagger UI ao seu projeto Maven. O `springfox-boot-starter` já inclui o Swagger UI por padrão, acessível geralmente em `/swagger-ui/`.

**Configuração Adicional (Opcional):**

Você pode personalizar a URL do Swagger UI ou outras configurações no seu arquivo `application.properties` ou `application.yml`.

**Exemplo em `application.properties`:**

Properties

`springfox.documentation.swagger-ui.path=/minha-documentacao`

Com essa configuração, o Swagger UI estaria acessível em `/minha-documentacao/`.

**Geração Estática (Menos Comum com Springfox):**

Em alguns cenários, você pode querer gerar o arquivo de especificação OpenAPI estaticamente durante o processo de build do Maven. Isso geralmente envolve a configuração de plugins Maven específicos para a biblioteca Swagger que você está utilizando. Consulte a documentação da biblioteca escolhida para obter detalhes sobre a geração estática.

**Capítulo 8: Práticas Recomendadas em um Contexto Maven**

Para criar uma documentação de API de alta qualidade com Swagger integrado ao seu projeto Maven, considere as seguintes práticas:

- **Mantenha as Anotações Atualizadas:** Certifique-se de que as anotações no seu código Java reflitam com precisão o comportamento da sua API.
- **Seja Detalhado e Claro nas Anotações:** Forneça descrições concisas e informativas nas anotações `@Api`, `@ApiOperation`, `@ApiParam` e `@ApiModelProperty`.
- **Use Nomes Significativos em Classes e Métodos:** Nomes claros facilitam a compreensão da documentação gerada.
- **Forneça Exemplos (Através de Anotações ou Configuração):** Algumas bibliotecas permitem fornecer exemplos de requisição e resposta através de anotações ou configurações adicionais.
- **Organize seus Controllers com `@Api(tags = ...)`:** Utilize tags para agrupar endpoints logicamente na documentação.
- **Utilize `@ApiModel` e `@ApiModelProperty` Corretamente:** Descreva seus modelos de dados de forma clara e completa.
- **Configure a Segurança Adequadamente:** Utilize as anotações ou configurações apropriadas para descrever os mecanismos de segurança da sua API.
- **Verifique a Documentação Gerada Regularmente:** Acesse o endpoint do Swagger UI para garantir que a documentação esteja correta e atualizada.
- **Integre a Verificação da Documentação ao seu Processo de Build (Opcional):** Existem ferramentas que podem validar seu arquivo de especificação OpenAPI durante o build do Maven.

**Conclusão**

Integrar o Swagger ao seu projeto Java com Maven simplifica significativamente o processo de documentação de APIs. Ao utilizar bibliotecas como o Springfox e suas anotações, você pode gerar uma documentação rica e interativa diretamente do seu código. O Maven facilita o gerenciamento das dependências necessárias e a configuração do ambiente.

Esperamos que este mini ebook tenha te dado uma base sólida para começar a documentar suas APIs Java com Swagger e Maven com confiança. Explore as anotações, configure o Swagger UI e eleve o nível da sua documentação!

**Próximos Passos:**

- Adicione a dependência Springfox ao seu projeto Maven.
- Anote seus controllers e modelos de dados com as anotações Springfox.
- Execute sua aplicação e acesse o Swagger UI (geralmente em `/swagger-ui/`).
- Explore as opções de configuração e personalização do Springfox.
- Consulte a documentação oficial do Springfox para recursos mais avançados.

**Boa documentação!**

[](https://lh3.googleusercontent.com/a/ACg8ocJeHpDuAx2RF2NwkG0rHIQ37cOknZhfcxPXQOE5CR04gshmxwNzyQ=s64-c)

Criar Visão Geral em Áudio

Deep Research

- • **O que é o Swagger/OpenAPI Specification?** Entenda o conceito e os benefícios de usar essa especificação.
- • **Principais Componentes da Especificação OpenAPI:** Explore a estrutura fundamental de um documento Swagger.
- • **Como Descrever seus Endpoints:** Detalhe caminhos, métodos HTTP, parâmetros e respostas.
- • **Definindo Modelos de Dados (Schemas):** Estruture os dados de requisição e resposta de forma clara.
- • **Adicionando Informações Adicionais:** Inclua segurança, tags e descrições para enriquecer sua documentação.
- • **Ferramentas Swagger:** Conheça as principais ferramentas para criar, visualizar e interagir com sua documentação.
- • **Práticas Recomendadas:** Dicas para criar uma documentação de API de alta qualidade.
- • **Gerar automaticamente documentação interativa:** A famosa interface do Swagger UI.
- • **Gerar código cliente (SDKs) em diversas linguagens:** Facilitando a integração com sua API.
- • **Validar requisições e respostas:** Garantindo a conformidade com a especificação.
- • **Criar mocks de API:** Para testes e desenvolvimento em paralelo.
- • **Clareza e Consistência:** Padroniza a forma como sua API é descrita.
- • **Melhora a Colaboração:** Facilita a comunicação entre equipes de desenvolvimento, testes e documentação.
- • **Acelera a Integração:** Desenvolvedores externos podem entender e usar sua API mais rapidamente.
- • **Reduz Erros:** A documentação precisa diminui a probabilidade de erros de integração.
- • **Melhora a Experiência do Desenvolvedor:** Uma boa documentação torna sua API mais atraente e fácil de usar.
- • **`openapi`:** Especifica a versão da OpenAPI Specification utilizada (ex: `3.0.0`).
- • **`info`:** Contém informações gerais sobre a API, como título, descrição, versão e informações de contato.
- • **`servers`:** Uma lista de URLs dos servidores onde a API está hospedada.
- • **`paths`:** Define os endpoints da sua API, incluindo os métodos HTTP suportados (GET, POST, PUT, DELETE, etc.) e seus detalhes.
- • **`components`:** Contém definições reutilizáveis para schemas (modelos de dados), responses, parameters, security schemes, etc.
- • **`summary`:** Uma breve descrição do que o endpoint faz.
- • **`description`:** Uma descrição mais detalhada do endpoint.
- • **`tags`:** Categorias para agrupar endpoints relacionados.
- • **`parameters`:** Define os parâmetros que podem ser enviados na requisição (path, query, header, cookie).
- • **`requestBody`:** Descreve o corpo da requisição (para métodos como POST e PUT).
- • **`responses`:** Define as possíveis respostas do endpoint, incluindo códigos de status HTTP, descrições e schemas para o corpo da resposta.
- • **`type`:** O tipo de dado (object, string, integer, number, boolean, array).
- • **`properties`:** Para objetos, define os campos e seus respectivos schemas.
- • **`format`:** Formatos específicos para tipos de dados (ex: int64, email, date, date-time).
- • **`description`:** Uma descrição do schema ou da propriedade.
- • **`required`:** Uma lista de propriedades obrigatórias para um objeto.
- • **`readOnly` / `writeOnly`:** Indica se uma propriedade é apenas para leitura ou apenas para escrita.
- • **`$ref`:** Permite referenciar schemas definidos em `components/schemas`, promovendo a reutilização.
- • **`security`:** Define os esquemas de segurança utilizados pela sua API (ex: Basic Auth, API Keys, OAuth 2.0).
- • **`tags`:** Permite agrupar seus endpoints logicamente, facilitando a navegação na documentação.
- • **`externalDocs`:** Um link para documentação externa relacionada.
- • **Swagger Editor:** Uma ferramenta online e offline para criar e editar arquivos de especificação OpenAPI no formato YAML ou JSON. Possui validação em tempo real.
- • **Swagger UI:** Uma interface web interativa que renderiza arquivos de especificação OpenAPI como documentação HTML visualmente atraente. Permite testar os endpoints diretamente.
- • **Swagger Codegen:** Uma ferramenta para gerar automaticamente código cliente (SDKs) em várias linguagens de programação e stubs de servidor a partir de um arquivo de especificação OpenAPI.
- • **Mantenha a Documentação Atualizada:** A documentação deve sempre refletir o estado atual da sua API.
- • **Seja Detalhado e Claro:** Forneça descrições concisas e informativas para todos os endpoints, parâmetros e schemas.
- • **Use Nomes Significativos:** Escolha nomes claros e descritivos para seus endpoints, parâmetros e schemas.
- • **Forneça Exemplos:** Inclua exemplos de requisição e resposta para facilitar o entendimento.
- • **Organize sua Documentação:** Use tags para agrupar endpoints relacionados e facilite a navegação.
- • **Utilize Referências (`$ref`):** Reutilize definições de schemas e outros componentes para manter a consistência e reduzir a redundância.
- • **Valide sua Especificação:** Utilize o Swagger Editor ou outras ferramentas para garantir que seu arquivo de especificação esteja correto e bem formatado.
- • **Integre a Geração da Documentação ao seu Workflow:** Considere automatizar a geração da documentação a partir do seu código ou de arquivos de configuração.
- • Experimente o Swagger Editor online.
- • Integre o Swagger UI na sua próxima API.
- • Considere usar o Swagger Codegen para gerar SDKs.
- • Explore a documentação oficial da OpenAPI Specification para se aprofundar nos detalhes.
- • **O que é o Swagger/OpenAPI Specification?** Entenda o conceito e os benefícios de usar essa especificação.
- • **Integração do Swagger com Projetos Maven:** Descubra como adicionar as dependências necessárias.
- • **Principais Componentes da Especificação OpenAPI:** Explore a estrutura fundamental de um documento Swagger.
- • **Como Descrever seus Endpoints (Anotações Java):** Detalhe caminhos, métodos HTTP, parâmetros e respostas diretamente no seu código Java.
- • **Definindo Modelos de Dados (Schemas) em Java:** Estruture os dados de requisição e resposta utilizando suas classes Java.
- • **Adicionando Informações Adicionais (Anotações):** Inclua segurança, tags e descrições utilizando anotações específicas.
- • **Gerando a Documentação Swagger com Maven:** Configure plugins Maven para gerar o arquivo de especificação OpenAPI e a interface Swagger UI.
- • **Práticas Recomendadas em um Contexto Maven:** Dicas para criar uma documentação de API de alta qualidade integrada ao seu projeto Maven.
- • **Gerar automaticamente documentação interativa:** A famosa interface do Swagger UI.
- • **Gerar código cliente (SDKs) em diversas linguagens:** Facilitando a integração com sua API.
- • **Validar requisições e respostas:** Garantindo a conformidade com a especificação.
- • **Criar mocks de API:** Para testes e desenvolvimento em paralelo.
- • **Clareza e Consistência:** Padroniza a forma como sua API é descrita.
- • **Melhora a Colaboração:** Facilita a comunicação entre equipes de desenvolvimento, testes e documentação.
- • **Acelera a Integração:** Desenvolvedores externos podem entender e usar sua API mais rapidamente.
- • **Reduz Erros:** A documentação precisa diminui a probabilidade de erros de integração.
- • **Melhora a Experiência do Desenvolvedor:** Uma boa documentação torna sua API mais atraente e fácil de usar.
- • **Integração Simplificada com Maven:** O Maven facilita a adição e o gerenciamento das bibliotecas Swagger.
- • **Versão:** Verifique a versão mais recente do `springfox-boot-starter` (ou outra biblioteca Swagger que você escolher) no Maven Repository e utilize-a.
- • **Spring Boot:** O `springfox-boot-starter` é ideal para projetos Spring Boot. Se você estiver usando apenas Spring MVC, pode precisar de dependências ligeiramente diferentes (como `springfox-swagger2` e `springfox-swagger-ui`).
- • **Outras Bibliotecas:** Existem outras bibliotecas para integrar Swagger com Java e Maven, como o Swagger Core diretamente ou frameworks como o Jakarta EE com suas próprias extensões. A escolha dependerá do seu ambiente de desenvolvimento.
- • **`@Api`:** Nível de classe, define metadados para o controlador (tags, descrição).
- • **`@ApiOperation`:** Nível de método, descreve a operação do endpoint (valor, resposta, etc.).
- • **`@ApiParam`:** Nível de parâmetro, descreve um parâmetro de requisição (`@PathVariable`, `@RequestParam`, `@RequestBody`).
- • **`@ApiResponses` e `@ApiResponse`:** Nível de método, define as possíveis respostas HTTP do endpoint.
- • **`@ApiModel`:** Nível de classe (geralmente DTOs/Entidades), descreve um modelo de dados (schema).
- • **`@ApiModelProperty`:** Nível de campo dentro de um `@ApiModel`, descreve uma propriedade do modelo de dados.
- • **Mantenha as Anotações Atualizadas:** Certifique-se de que as anotações no seu código Java reflitam com precisão o comportamento da sua API.
- • **Seja Detalhado e Claro nas Anotações:** Forneça descrições concisas e informativas nas anotações `@Api`, `@ApiOperation`, `@ApiParam` e `@ApiModelProperty`.
- • **Use Nomes Significativos em Classes e Métodos:** Nomes claros facilitam a compreensão da documentação gerada.
- • **Forneça Exemplos (Através de Anotações ou Configuração):** Algumas bibliotecas permitem fornecer exemplos de requisição e resposta através de anotações ou configurações adicionais.
- • **Organize seus Controllers com `@Api(tags = ...)`:** Utilize tags para agrupar endpoints logicamente na documentação.
- • **Utilize `@ApiModel` e `@ApiModelProperty` Corretamente:** Descreva seus modelos de dados de forma clara e completa.
- • **Configure a Segurança Adequadamente:** Utilize as anotações ou configurações apropriadas para descrever os mecanismos de segurança da sua API.
- • **Verifique a Documentação Gerada Regularmente:** Acesse o endpoint do Swagger UI para garantir que a documentação esteja correta e atualizada.
- • **Integre a Verificação da Documentação ao seu Processo de Build (Opcional):** Existem ferramentas que podem validar seu arquivo de especificação OpenAPI durante o build do Maven.
- • Adicione a dependência Springfox ao seu projeto Maven.
- • Anote seus controllers e modelos de dados com as anotações Springfox.
- • Execute sua aplicação e acesse o Swagger UI (geralmente em `/swagger-ui/`).
- • Explore as opções de configuração e personalização do Springfox.
- • Consulte a documentação oficial do Springfox para recursos mais avançados.

[](https://lh3.googleusercontent.com/a/ACg8ocJeHpDuAx2RF2NwkG0rHIQ37cOknZhfcxPXQOE5CR04gshmxwNzyQ=s64-c)

Google Apps

Conta do Google

Jefferson Da Matta

jeffersondlmatta@gmail.com