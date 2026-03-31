# 🏥 Voll.med - API REST com Spring Boot 3

## 📋 Índice
- [Sobre o Projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Arquitetura](#arquitetura)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Pré-requisitos](#pré-requisitos)
- [Instalação e Configuração](#instalação-e-configuração)
- [Banco de Dados](#banco-de-dados)
- [Endpoints da API](#endpoints-da-api)
- [Boas Práticas Implementadas](#boas-práticas-implementadas)
- [Roadmap](#roadmap)
- [Referências](#referências)

---

## 📖 Sobre o Projeto

A **Voll.med REST API** é uma aplicação backend desenvolvida com **Spring Boot 3** para gerenciamento de cadastros médicos de uma clínica fictícia. Este é um projeto **introdutório** que foca nos fundamentos de desenvolvimento de APIs REST com Spring Framework.

Este projeto foi desenvolvido durante o **Curso de Spring Boot 3: documente, teste e prepare uma API para o deploy** da **DevMedia**, abordando os conceitos essenciais:

- 🌐 **REST API** com Spring Web
- 💾 **Persistência** com JPA/Hibernate
- 🗃️ **Migrations** com Flyway
- ✅ **Validações** com Bean Validation
- 📝 **DTOs** usando Records do Java 17
- 📄 **Paginação** de resultados
- 🗑️ **Soft Delete** (exclusão lógica)

### 🎯 Objetivo

Desenvolver uma API REST simples e funcional para:
- **Cadastrar** médicos com especialidade e endereço completo
- **Listar** médicos de forma paginada
- **Atualizar** informações cadastrais
- **Inativar** médicos (soft delete)

> 💡 **Nota**: Este é um projeto **fundamental** focado em CRUD básico. Funcionalidades avançadas como autenticação JWT, agendamento de consultas e validações complexas serão implementadas em versões futuras do curso.

---

## ⚙️ Funcionalidades

### ✅ Implementadas

#### CRUD de Médicos
- ✅ **Cadastrar** médico com dados completos
  - Nome, email, telefone, CRM
  - Especialidade médica (enum)
  - Endereço completo (logradouro, bairro, CEP, cidade, UF, número, complemento)
- ✅ **Listar** médicos ativos com paginação
  - Ordenação padrão por nome
  - Tamanho de página configurável (padrão: 10 registros)
  - Filtro automático de médicos ativos
- ✅ **Atualizar** informações cadastrais
  - Atualização parcial (apenas campos informados)
  - Nome, telefone e endereço podem ser atualizados
- ✅ **Excluir** médico (soft delete)
  - Inativação lógica mantendo dados no banco
  - Médicos inativos não aparecem em listagens

### 🚧 Planejadas para Próximas Versões

- ⏳ CRUD de pacientes
- ⏳ Sistema de autenticação JWT
- ⏳ Agendamento de consultas
- ⏳ Cancelamento de consultas
- ⏳ Documentação Swagger/OpenAPI
- ⏳ Testes automatizados

---

## 🏗️ Arquitetura

O projeto segue uma **arquitetura em camadas simplificada** (MVC pattern adaptado):

```
┌──────────────────────────────────────────────────────┐
│               CONTROLLER LAYER                       │
│          (Apresentação/Requisições HTTP)             │
│                                                      │
│  MedicoController                                    │
│    └── Endpoints REST (GET, POST, PUT, DELETE)       │
└──────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────┐
│                 DOMAIN LAYER                         │
│           (Entidades e Lógica de Domínio)            │
│                                                      │
│  Medico (Entidade JPA)                               │
│  Endereco (Embeddable)                               │
│  Especialidade (Enum)                                │
│  DTOs (Records): Cadastro, Atualização, Listagem     │
└──────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────┐
│             REPOSITORY LAYER                         │
│           (Acesso a Dados/Persistência)              │
│                                                      │
│  MedicoRepository (Spring Data JPA)                  │
│    └── Queries derivadas: findAllByAtivoTrue()       │
└──────────────────────────────────────────────────────┘
                        ↓
┌──────────────────────────────────────────────────────┐
│                 DATABASE LAYER                       │
│              (MySQL + Flyway Migrations)             │
│                                                      │
│  MySQL Database: vollmed_api                         │
│  Flyway: Versionamento automático                    │
└──────────────────────────────────────────────────────┘
```

### Padrões de Design

- ✅ **MVC Pattern** - Separação clara entre camadas
- ✅ **Repository Pattern** - Abstração de persistência com Spring Data JPA
- ✅ **DTO Pattern** - Records para transferência de dados
- ✅ **Embedded Pattern** - Endereco como @Embeddable
- ✅ **Soft Delete Pattern** - Flag `ativo` para exclusão lógica

---

## 🛠 Tecnologias Utilizadas

### Framework e Linguagem
- **[Java 17](https://www.oracle.com/java)** 
  - Records para DTOs
  - Text Blocks
  - Pattern Matching
  - Inferência de tipos com `var`
- **[Spring Boot 3.0.0-M5](https://spring.io/projects/spring-boot)** (Milestone)
- **[Maven](https://maven.apache.org)** - Gerenciamento de build e dependências

### Módulos Spring
- **[Spring Web](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html)** - REST API
- **[Spring Data JPA](https://spring.io/projects/spring-data-jpa)** - Abstração de persistência
- **[Spring Boot DevTools](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.devtools)** - Hot reload

### Banco de Dados
- **[MySQL 8.0+](https://www.mysql.com)** - Banco de dados relacional
- **[Hibernate](https://hibernate.org)** - Implementação JPA/ORM
- **[Flyway](https://flywaydb.org)** - Versionamento e migração de schema

### Validação e Produtividade
- **[Bean Validation (Jakarta)](https://beanvalidation.org)** - Validação declarativa
- **[Lombok](https://projectlombok.org)** - Redução de boilerplate
  - `@Getter`, `@NoArgsConstructor`, `@AllArgsConstructor`
  - `@EqualsAndHashCode`

### Testes
- **[Spring Boot Test](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing)** - Starter para testes
- **[JUnit 5](https://junit.org/junit5/)** - Framework de testes

---

## 📁 Estrutura do Projeto

```
spring-boot-3-rest-api-main/
│
├── src/
│   ├── main/
│   │   ├── java/med/voll/api/
│   │   │   │
│   │   │   ├── ApiApplication.java              # Classe principal
│   │   │   │
│   │   │   ├── controller/                      # Controllers REST
│   │   │   │   ├── HelloController.java         # Endpoint de teste
│   │   │   │   └── MedicoController.java        # CRUD de médicos
│   │   │   │
│   │   │   ├── medico/                          # Domínio de Médicos
│   │   │   │   ├── Medico.java                  # Entidade JPA
│   │   │   │   ├── MedicoRepository.java        # Repository interface
│   │   │   │   ├── Especialidade.java           # Enum de especialidades
│   │   │   │   ├── DadosCadastroMedico.java     # DTO Record - Cadastro
│   │   │   │   ├── DadosAtualizacaoMedico.java  # DTO Record - Atualização
│   │   │   │   └── DadosListagemMedico.java     # DTO Record - Listagem
│   │   │   │
│   │   │   └── endereco/                        # Domínio de Endereço
│   │   │       ├── Endereco.java                # Embeddable
│   │   │       └── DadosEndereco.java           # DTO Record
│   │   │
│   │   └── resources/
│   │       ├── application.properties           # Configurações da aplicação
│   │       └── db/migration/                    # Scripts Flyway
│   │           ├── V1__create-table-medicos.sql
│   │           ├── V2__alter-table-medicos-add-column-telefone.sql
│   │           └── V3__alter-table-medicos-add-column-ativo.sql
│   │
│   └── test/
│       └── java/med/voll/api/
│           └── ApiApplicationTests.java          # Testes básicos
│
├── .mvn/                                         # Maven wrapper
├── .gitignore
├── mvnw                                          # Maven wrapper Unix/Mac
├── mvnw.cmd                                      # Maven wrapper Windows
├── pom.xml                                       # Configuração Maven
└── README.md
```

### Organização por Domínios

O projeto organiza código por **domínios de negócio**:

- **`medico/`** - Tudo relacionado a médicos
- **`endereco/`** - Componente reutilizável de endereço
- **`controller/`** - Controladores REST

Essa estrutura facilita:
- 📦 Localização de código relacionado
- 🔧 Manutenção e evolução
- 🧩 Adição de novos domínios (pacientes, consultas, etc)

---

## 📦 Pré-requisitos

### Software Necessário

| Software | Versão Mínima | Link |
|----------|---------------|------|
| ☕ Java JDK | 17+ | [Download](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html) |
| 🔧 Maven | 3.8+ | [Download](https://maven.apache.org/download.cgi) |
| 🗄️ MySQL | 8.0+ | [Download](https://dev.mysql.com/downloads/mysql/) |
| 💻 IDE | - | IntelliJ IDEA, Eclipse ou VS Code |

### Verificar Instalação

```bash
# Verificar Java
java -version
# Esperado: java version "17.x.x" ou superior

# Verificar Maven (opcional, pode usar mvnw)
mvn -version
# Esperado: Apache Maven 3.8.x ou superior

# Verificar MySQL
mysql --version
# Esperado: mysql Ver 8.0.x ou superior
```

---

## 🚀 Instalação e Configuração

### Passo 1: Clonar o Repositório

```bash
git clone <url-do-repositorio>
cd spring-boot-3-rest-api-main
```

### Passo 2: Configurar o MySQL

```sql
-- Conecte ao MySQL e execute:
CREATE DATABASE vollmed_api;
```

### Passo 3: Configurar Credenciais

Edite `src/main/resources/application.properties`:

```properties
# Configuração do banco de dados
spring.datasource.url=jdbc:mysql://localhost/vollmed_api
spring.datasource.username=root
spring.datasource.password=root

# Configuração Hibernate/JPA
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

> 💡 **Dica**: Ajuste `username` e `password` conforme suas credenciais MySQL.

### Passo 4: Instalar Dependências

```bash
mvn clean install
```

Ou usando o Maven Wrapper (não precisa ter Maven instalado):

```bash
# Linux/Mac
./mvnw clean install

# Windows
mvnw.cmd clean install
```

### Passo 5: Executar a Aplicação

```bash
# Com Maven
mvn spring-boot:run

# Com Maven Wrapper (recomendado)
./mvnw spring-boot:run         # Linux/Mac
mvnw.cmd spring-boot:run       # Windows
```

### Passo 6: Verificar se está Funcionando

Acesse no navegador ou use curl:

```bash
# Endpoint de teste
curl http://localhost:8080/hello

# Deve retornar: "Hello World!"
```

🎉 **Aplicação rodando em**: `http://localhost:8080`

---

## 🗄️ Banco de Dados

### Gerenciamento Automático com Flyway

O projeto utiliza **Flyway** para gerenciar o versionamento do banco de dados automaticamente. Na primeira execução, as migrations serão aplicadas.

### Migrations (Scripts SQL Versionados)

| Versão | Script | Descrição |
|--------|--------|-----------|
| **V1** | `V1__create-table-medicos.sql` | Cria tabela `medicos` inicial |
| **V2** | `V2__alter-table-medicos-add-column-telefone.sql` | Adiciona coluna `telefone` |
| **V3** | `V3__alter-table-medicos-add-column-ativo.sql` | Adiciona coluna `ativo` e atualiza registros |

### Modelo de Dados

#### Tabela: medicos

```sql
CREATE TABLE medicos (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    crm VARCHAR(6) NOT NULL UNIQUE,
    especialidade VARCHAR(100) NOT NULL,
    telefone VARCHAR(20) NOT NULL,
    ativo TINYINT NOT NULL DEFAULT 1,
    
    -- Endereço (embedded)
    logradouro VARCHAR(100) NOT NULL,
    bairro VARCHAR(100) NOT NULL,
    cep VARCHAR(9) NOT NULL,
    complemento VARCHAR(100),
    numero VARCHAR(20),
    uf CHAR(2) NOT NULL,
    cidade VARCHAR(100) NOT NULL
);
```

### Estrutura de Dados

```
┌─────────────────────────────────────────┐
│              MEDICO                     │
├─────────────────────────────────────────┤
│ id (PK)              BIGINT             │
│ nome                 VARCHAR(100)       │
│ email (UNIQUE)       VARCHAR(100)       │
│ crm (UNIQUE)         VARCHAR(6)         │
│ especialidade        VARCHAR(100)       │
│ telefone             VARCHAR(20)        │
│ ativo                TINYINT            │
│                                         │
│ ┌─────────────────────────────────────┐ │
│ │   ENDERECO (Embedded)               │ │
│ ├─────────────────────────────────────┤ │
│ │ logradouro      VARCHAR(100)        │ │
│ │ bairro          VARCHAR(100)        │ │
│ │ cep             VARCHAR(9)          │ │
│ │ complemento     VARCHAR(100)        │ │
│ │ numero          VARCHAR(20)         │ │
│ │ uf              CHAR(2)             │ │
│ │ cidade          VARCHAR(100)        │ │
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

### Especialidades Médicas (Enum)

```java
public enum Especialidade {
    ORTOPEDIA,
    CARDIOLOGIA,
    GINECOLOGIA,
    DERMATOLOGIA
}
```

### Evolução do Schema

O Flyway mantém histórico de todas as alterações:

```
Versão 1 (V1)          Versão 2 (V2)          Versão 3 (V3)
┌──────────┐           ┌──────────┐           ┌──────────┐
│ medicos  │   +Tel    │ medicos  │   +Ativo  │ medicos  │
├──────────┤  ──────>  ├──────────┤  ──────>  ├──────────┤
│ id       │           │ id       │           │ id       │
│ nome     │           │ nome     │           │ nome     │
│ email    │           │ email    │           │ email    │
│ crm      │           │ crm      │           │ crm      │
│ espec... │           │ espec... │           │ espec... │
│ endereço │           │ telefone │◄── novo   │ telefone │
│          │           │ endereço │           │ ativo    │◄── novo
└──────────┘           └──────────┘           └──────────┘
```

---

## 🌐 Endpoints da API

### Base URL
```
http://localhost:8080
```

### Recursos Disponíveis

#### 🏠 Hello World (Teste)

```http
GET /hello

Response: 200 OK
"Hello World!"
```

#### 👨‍⚕️ Médicos

##### 1. Cadastrar Médico

```http
POST /medicos
Content-Type: application/json

{
  "nome": "Dr. Carlos Oliveira",
  "email": "carlos.oliveira@voll.med",
  "telefone": "11987654321",
  "crm": "123456",
  "especialidade": "ORTOPEDIA",
  "endereco": {
    "logradouro": "Rua das Acácias",
    "bairro": "Jardim Paulista",
    "cep": "01234-567",
    "cidade": "São Paulo",
    "uf": "SP",
    "numero": "200",
    "complemento": "Bloco A - Sala 15"
  }
}

Response: 200 OK
```

##### 2. Listar Médicos (Paginado)

```http
GET /medicos
GET /medicos?page=0&size=5
GET /medicos?page=0&size=10&sort=nome
GET /medicos?page=1&size=20&sort=especialidade

Response: 200 OK
{
  "content": [
    {
      "id": 1,
      "nome": "Dr. Carlos Oliveira",
      "email": "carlos.oliveira@voll.med",
      "crm": "123456",
      "especialidade": "ORTOPEDIA"
    }
  ],
  "pageable": {
    "pageNumber": 0,
    "pageSize": 10,
    "sort": {
      "sorted": true,
      "unsorted": false
    }
  },
  "totalPages": 1,
  "totalElements": 1,
  "last": true,
  "first": true,
  "numberOfElements": 1
}
```

**Parâmetros de Query:**
- `page` - Número da página (inicia em 0)
- `size` - Quantidade de registros por página (padrão: 10)
- `sort` - Campo para ordenação (padrão: nome)

##### 3. Atualizar Médico

```http
PUT /medicos
Content-Type: application/json

{
  "id": 1,
  "nome": "Dr. Carlos Oliveira Silva",
  "telefone": "11999887766",
  "endereco": {
    "logradouro": "Avenida Paulista",
    "bairro": "Bela Vista",
    "cep": "01310-100",
    "cidade": "São Paulo",
    "uf": "SP",
    "numero": "1000"
  }
}

Response: 200 OK
```

> 💡 **Atualização Parcial**: Apenas os campos enviados serão atualizados. Email, CRM e especialidade não podem ser alterados.

##### 4. Excluir Médico (Inativar)

```http
DELETE /medicos/1

Response: 200 OK
```

> ⚠️ **Soft Delete**: O médico não é excluído do banco, apenas marcado como inativo (`ativo = false`). Médicos inativos não aparecem na listagem.

---

## 🎯 Boas Práticas Implementadas

### 1️⃣ Records do Java 17

DTOs imutáveis e concisos:

```java
public record DadosCadastroMedico(
    @NotBlank String nome,
    @NotBlank @Email String email,
    @NotBlank String telefone,
    @NotBlank @Pattern(regexp = "\\d{4,6}") String crm,
    @NotNull Especialidade especialidade,
    @NotNull @Valid DadosEndereco endereco
) {}
```

**Vantagens:**
- ✅ Imutabilidade automática
- ✅ `equals()`, `hashCode()` e `toString()` gerados
- ✅ Código mais limpo e legível

### 2️⃣ Bean Validation

Validações declarativas com annotations:

```java
@NotBlank                          // Não pode ser nulo, vazio ou só espaços
@NotNull                           // Não pode ser nulo
@Email                             // Formato válido de email
@Pattern(regexp = "\\d{4,6}")     // CRM: 4 a 6 dígitos
@Valid                             // Validação em cascata (nested objects)
```

### 3️⃣ Paginação com Spring Data

```java
@GetMapping
public Page<DadosListagemMedico> listar(
    @PageableDefault(size = 10, sort = {"nome"}) Pageable paginacao
) {
    return repository.findAllByAtivoTrue(paginacao)
        .map(DadosListagemMedico::new);
}
```

**Benefícios:**
- 🚀 Performance melhorada (não carrega todos os registros)
- 📄 Resposta inclui metadados de paginação
- 🔧 Configurável via query params

### 4️⃣ Soft Delete

```java
@DeleteMapping("/{id}")
@Transactional
public void excluir(@PathVariable Long id) {
    var medico = repository.getReferenceById(id);
    medico.excluir();  // Apenas marca ativo = false
}
```

**Vantagens:**
- ✅ Preserva histórico de dados
- ✅ Possibilita recuperação futura
- ✅ Mantém integridade referencial

### 5️⃣ Derived Queries

Spring Data JPA gera queries automaticamente:

```java
public interface MedicoRepository extends JpaRepository<Medico, Long> {
    Page<Medico> findAllByAtivoTrue(Pageable paginacao);
}
```

Traduz para SQL:
```sql
SELECT * FROM medicos WHERE ativo = 1 ORDER BY nome LIMIT 10 OFFSET 0;
```

### 6️⃣ Embedded Entities

Endereço como componente embarcado:

```java
@Embeddable
public class Endereco {
    private String logradouro;
    private String bairro;
    // ... outros campos
}

@Entity
public class Medico {
    @Embedded
    private Endereco endereco;
}
```

**Benefício**: Reutilização de código para múltiplas entidades (médico, paciente, etc).

### 7️⃣ Flyway Migrations

Versionamento de banco de dados como código:

```
V1__create-table-medicos.sql       ← Cria estrutura inicial
V2__alter-table-add-telefone.sql   ← Adiciona campo
V3__alter-table-add-ativo.sql      ← Adiciona soft delete
```

**Vantagens:**
- ✅ Rastreabilidade de mudanças
- ✅ Deploy automatizado
- ✅ Sincronização entre ambientes

### 8️⃣ Separação de Responsabilidades

```
Controller  → Recebe requisições HTTP, valida entrada
Repository  → Acesso a dados
Entity      → Lógica de domínio
DTO         → Transferência de dados
```

---

## 📚 Exemplos Práticos

### Exemplo 1: Cadastro Completo

```bash
curl -X POST http://localhost:8080/medicos \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Dra. Ana Costa",
    "email": "ana.costa@voll.med",
    "telefone": "21987654321",
    "crm": "654321",
    "especialidade": "GINECOLOGIA",
    "endereco": {
      "logradouro": "Avenida Brasil",
      "bairro": "Centro",
      "cep": "20000-000",
      "cidade": "Rio de Janeiro",
      "uf": "RJ",
      "numero": "1500",
      "complemento": "Consultório 301"
    }
  }'
```

### Exemplo 2: Listagem Paginada

```bash
# Primeira página (10 registros)
curl http://localhost:8080/medicos

# Segunda página
curl http://localhost:8080/medicos?page=1

# 20 registros por página, ordenado por especialidade
curl "http://localhost:8080/medicos?size=20&sort=especialidade"
```

### Exemplo 3: Atualização Parcial

```bash
# Atualiza apenas telefone e endereço (nome e email permanecem iguais)
curl -X PUT http://localhost:8080/medicos \
  -H "Content-Type: application/json" \
  -d '{
    "id": 1,
    "telefone": "11999998888",
    "endereco": {
      "logradouro": "Nova Rua",
      "numero": "500"
    }
  }'
```

### Exemplo 4: Inativação

```bash
# Marca médico como inativo
curl -X DELETE http://localhost:8080/medicos/1
```

---

## 🎓 Conceitos Aprendidos no Curso

### Módulo 1: Fundamentos REST
- ✅ Princípios REST e recursos
- ✅ Verbos HTTP (GET, POST, PUT, DELETE)
- ✅ Status codes HTTP apropriados
- ✅ Spring Web e @RestController

### Módulo 2: Persistência de Dados
- ✅ JPA e Hibernate
- ✅ Spring Data JPA
- ✅ Entidades e relacionamentos
- ✅ Repositories e queries derivadas

### Módulo 3: Validações
- ✅ Bean Validation
- ✅ Annotations de validação
- ✅ Validação em cascata (@Valid)
- ✅ Custom validators

### Módulo 4: Migrations
- ✅ Flyway migrations
- ✅ Versionamento de schema
- ✅ Scripts SQL organizados

### Módulo 5: Boas Práticas
- ✅ DTOs com Records
- ✅ Paginação de resultados
- ✅ Soft delete
- ✅ Separação de responsabilidades

---

## 🗺️ Roadmap

### 📍 Estado Atual
Este projeto é a **versão básica** focada em:
- ✅ CRUD de médicos
- ✅ Persistência com MySQL
- ✅ Validações básicas
- ✅ Paginação

### 🚀 Próximas Versões (Evolução do Curso)

#### Versão 2.0 - Expansão de Recursos
- [ ] CRUD de pacientes
- [ ] Validações mais complexas
- [ ] Tratamento de erros personalizado
- [ ] DTO de detalhamento completo

#### Versão 3.0 - Segurança
- [ ] Spring Security
- [ ] Autenticação JWT
- [ ] Endpoints protegidos
- [ ] Autorização por roles

#### Versão 4.0 - Consultas
- [ ] Agendamento de consultas
- [ ] Cancelamento de consultas
- [ ] Regras de negócio complexas
- [ ] Validadores customizados

#### Versão 5.0 - Documentação e Testes
- [ ] SpringDoc OpenAPI
- [ ] Swagger UI
- [ ] Testes unitários
- [ ] Testes de integração

#### Versão 6.0 - Deploy
- [ ] Profile de produção
- [ ] Docker e Docker Compose
- [ ] CI/CD
- [ ] Monitoramento

---

## 📖 Guia de Estudo

### Para Iniciantes em Spring Boot

1. **Comece aqui** 👈 Você está no projeto certo!
   - Entenda controllers e endpoints REST
   - Aprenda sobre entidades JPA
   - Pratique validações com Bean Validation

2. **Próximo passo**: `spring-boot-3-best-practices-main`
   - Adiciona autenticação JWT
   - Implementa mais domínios (pacientes, usuários)
   - Tratamento global de exceções

3. **Projeto avançado**: `spring-boot-api-deploy-main`
   - Agendamento de consultas
   - Validações complexas de negócio
   - Documentação Swagger
   - Testes automatizados

### Fluxo de Aprendizado

```
┌─────────────────────┐
│  REST API Básica    │ ← VOCÊ ESTÁ AQUI
│  (Este projeto)     │
│  - CRUD             │
│  - JPA              │
│  - Validações       │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│  Best Practices     │
│  - JWT Auth         │
│  - Multi-domain     │
│  - Exception Handle │
└──────────┬──────────┘
           │
           ↓
┌─────────────────────┐
│  Production Ready   │
│  - Business Rules   │
│  - Swagger Docs     │
│  - Tests            │
│  - Deploy           │
└─────────────────────┘
```

---

## 🧪 Testando a API

### Com cURL

```bash
# 1. Cadastrar médico
curl -X POST http://localhost:8080/medicos \
  -H "Content-Type: application/json" \
  -d '{"nome":"Dr. Pedro","email":"pedro@email.com","telefone":"11999999999","crm":"111111","especialidade":"CARDIOLOGIA","endereco":{"logradouro":"Rua A","bairro":"Centro","cep":"12345-678","cidade":"São Paulo","uf":"SP","numero":"10"}}'

# 2. Listar médicos
curl http://localhost:8080/medicos

# 3. Atualizar médico
curl -X PUT http://localhost:8080/medicos \
  -H "Content-Type: application/json" \
  -d '{"id":1,"nome":"Dr. Pedro Silva","telefone":"11888888888"}'

# 4. Inativar médico
curl -X DELETE http://localhost:8080/medicos/1
```

### Com Postman ou Insomnia

1. **Importe a collection** (se disponível)
2. **Configure Base URL**: `http://localhost:8080`
3. **Teste os endpoints** sequencialmente

### Validando o Banco de Dados

```sql
-- Ver todos os médicos
SELECT * FROM medicos;

-- Ver apenas ativos
SELECT * FROM medicos WHERE ativo = 1;

-- Ver especialidades
SELECT DISTINCT especialidade FROM medicos;

-- Ver histórico Flyway
SELECT * FROM flyway_schema_history;
```

---

## 🎓 Referências

### Curso Base

**Curso**: Spring Boot 3: documente, teste e prepare uma API para o deploy  
**Plataforma**: [DevMedia](https://www.devmedia.com.br)  
**Instrutor**: Rodrigo Ferreira  
**Escola**: [Alura](https://www.alura.com.br)

### Documentação Oficial

- 📘 [Spring Boot 3 Reference](https://docs.spring.io/spring-boot/docs/3.0.x/reference/htmlsingle/)
- 🌐 [Spring Web MVC](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html)
- 💾 [Spring Data JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
- 🗃️ [Flyway Documentation](https://flywaydb.org/documentation/)
- ☕ [Java 17 Documentation](https://docs.oracle.com/en/java/javase/17/)

### Tutoriais Recomendados

- 🔰 [Spring Quickstart Guide](https://spring.io/quickstart)
- 📚 [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
- 🎯 [Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/)
- ✅ [Validation Guide](https://spring.io/guides/gs/validating-form-input/)

### Links do Projeto

- 🎨 [Layout Figma](https://www.figma.com/file/N4CgpJqsg7gjbKuDmra3EV/Voll.med)
- 📋 [Trello - Funcionalidades](https://trello.com/b/O0lGCsKb/api-voll-med)

---

## 🔧 Troubleshooting

### Problema: Erro ao conectar no MySQL

```
com.mysql.cj.jdbc.exceptions.CommunicationsException: Communications link failure
```

**Soluções:**
1. Verifique se o MySQL está rodando: `sudo service mysql status`
2. Confirme usuário e senha no `application.properties`
3. Verifique se o banco `vollmed_api` foi criado

### Problema: Flyway migration falha

```
FlywayException: Validate failed: Migrations have failed validation
```

**Soluções:**
1. Limpe o histórico Flyway: `DELETE FROM flyway_schema_history;`
2. Ou recrie o banco: `DROP DATABASE vollmed_api; CREATE DATABASE vollmed_api;`

### Problema: Porta 8080 já em uso

```
Port 8080 was already in use
```

**Soluções:**
1. Adicione no `application.properties`: `server.port=8081`
2. Ou mate o processo na porta 8080

### Problema: Lombok não funciona na IDE

**Soluções:**
1. **IntelliJ**: Instale plugin "Lombok" e habilite annotation processing
2. **Eclipse**: Baixe `lombok.jar` e execute o instalador
3. **VS Code**: Instale extensão "Lombok Annotations Support"

---

## 📄 Licença

Projeto desenvolvido pela [Alura](https://www.alura.com.br) para fins educacionais.

---

## 👨‍💻 Sobre

Projeto educacional baseado no curso da **DevMedia**, focado em fundamentos de desenvolvimento de APIs REST com Spring Boot 3.

### Evolução do Projeto

Este é o **projeto inicial** de uma série:

1. **spring-boot-3-rest-api-main** ← Você está aqui
   - REST API básica
   - CRUD simples
   - Fundamentos

2. **spring-boot-3-best-practices-main**
   - Adiciona autenticação
   - Múltiplos domínios
   - Boas práticas avançadas

3. **spring-boot-api-deploy-main**
   - Regras de negócio complexas
   - Documentação completa
   - Preparado para produção

---

## 🤝 Contribuindo

Este é um projeto educacional. Para contribuir:

1. Fork o projeto
2. Crie uma branch: `git checkout -b feature/minha-feature`
3. Commit: `git commit -m 'Adiciona minha feature'`
4. Push: `git push origin feature/minha-feature`
5. Abra um Pull Request

---

## 💬 Suporte

- 💡 **Dúvidas sobre Spring Boot**: [Stack Overflow](https://stackoverflow.com/questions/tagged/spring-boot)
- 🎓 **Dúvidas sobre o curso**: [DevMedia](https://www.devmedia.com.br) ou [Alura](https://www.alura.com.br)
- 🐛 **Issues**: Abra uma issue neste repositório

---

## ⭐ Próximos Passos

Depois de dominar este projeto, você estará pronto para:

1. ✅ Adicionar novos domínios (pacientes)
2. ✅ Implementar autenticação
3. ✅ Criar regras de negócio complexas
4. ✅ Escrever testes automatizados
5. ✅ Documentar com Swagger
6. ✅ Fazer deploy em produção

---

**Desenvolvido com ❤️ para aprender Spring Boot 3**
