![Header](https://capsule-render.vercel.app/api?type=waving&height=121&color=A9A9A9&text=🚑%20Gerenciador%20de%20atendimento%20médico%20🚑&fontSize=35&fontColor=D3D3D3&section=header&fontAlignY=65)

<p align="center">
  <a href="#-objetivo-">✨ Objetivo</a> |
  <a href="#-tecnologias-utilizadas-">Tecnologias</a> |
  <a href="#%EF%B8%8F-código-%EF%B8%8F">Código</a> |
  <a href="#-detalhes-do-projeto-">Detalhes</a> |
  <a href="#-autores-do-projeto-">Autores ✨</a>
</p>

## 📈 Objetivo 📈

A ideia do projeto é criarmos um gerenciador de atendimento para um `hospital em C`.

Dentro do projeto temos alguns critérios a serem analisados, fora que cada item do menu utiliza alguma parte matéria `Estrutura de Dados`.

São elas:
- Lista Dinâmica Encadeada;
- Fila;
- Árvore Binária de Busca;

## 👾 Tecnologias Utilizadas 👾
<div style="display: inline_block" ><br>
  <img align="center" alt="C" src="https://img.shields.io/badge/C-A8B9CC.svg?style=for-the-badge&logo=C&logoColor=black">  
  <img align="center" alt="Replit" src="https://img.shields.io/badge/Replit-F26207.svg?style=for-the-badge&logo=Replit&logoColor=white">  
</div>
<br>

## 🖥️ Código 🖥️

Optamos por incluir o código utilizado no projeto neste bloco para uma apresentação visual mais atraente, mas ele também está disponível no repositório.
<details>
  <summary>Código em C</summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
  int dia;
  int mes;
  int ano;
} Data;

typedef struct {
  char nome[100];
  int idade;
  char rg[10];
  Data entrada;
} Paciente;

typedef struct ELista {
  Paciente dados;
  struct ELista *proximo;
} ELista;

typedef struct {
  ELista *inicio;
  int qtde;
} Lista;

typedef struct EFila {
  Paciente dados;
  struct EFila *proximo;
} EFila;

typedef struct {
  EFila *head;
  EFila *tail;
  int qtde;
} Fila;

typedef struct EArvore {
  Paciente dados;
  struct EArvore *filhoEsq;
  struct EArvore *filhoDir;
} EArvore;

typedef struct {
  EArvore *raiz;
  int qtde;
} Arvore;

//***CADASTRO***
//***CADASTRO***
//***CADASTRO***
//***CADASTRO***
//***CADASTRO***

// Como funciona a lista?
// A lista é uma estrutura de dados que armazena uma sequência de elementos.
// Cada elemento é armazenado em uma posição na lista. Caca elemento ou nó,
// aponta junto com um dado para o próximo nó na sequência. Essa estrutura
// permite um redimensionamento dinâmico muito bom e a inserçao e remoção
// eficiente de elementos de qualquer lugar da lista.

// Nessa funçao nós inicializamos a lista, recebendo um ponteiro da struct Lista
void inicializarLista(Lista *lista) {
  lista->inicio = NULL;
  lista->qtde = 0;
}

// Nessa funçao nós cadastramos um paciente na lista.
// O fgets() guarda o valor da string e inclui um caractere nulo '\0' no final.
// O strcspn procura a aparição de um caractere em uma primeira string. Assim,
// ele ajuda na diferenciação de um nome para outro, ou de um rg para o outro.
void cadastrarPaciente(Lista *lista) {
  Paciente p;
  printf("\n");
  printf("Nome: ");
  fgets(p.nome, sizeof(p.nome), stdin);
  p.nome[strcspn(p.nome, "\n")] = '\0';
  printf("Idade: ");
  scanf("%d", &p.idade);
  getchar();
  printf("RG (sem . e sem espaços): ");
  fgets(p.rg, sizeof(p.rg), stdin);
  p.rg[strcspn(p.rg, "\n")] = '\0';
  printf("Data de entrada (DD MM AAAA): ");
  scanf("%d %d %d", &p.entrada.dia, &p.entrada.mes, &p.entrada.ano);
  getchar();

  ELista *novo = (ELista *)malloc(sizeof(ELista));
  if (novo == NULL) {
    printf("Erro ao alocar memória.\n");
    return;
  }
  novo->dados = p;
  novo->proximo = lista->inicio;
  lista->inicio = novo;
  lista->qtde++;

  printf("Paciente cadastrado com sucesso!\n");
}

// Nessa funçao consultamos o paciente na lista por meio do RG.
// A função strcmp() compara duas strings e retorna 0 se forem iguais. Assim
// ajudando na busca do paciente. Se o rg fornecido for igual ao que está
// armazenado no "database", o código retorna 0 para identificar que foi
// encontrado o paciente procurado!
void consultarPaciente(Lista *lista) {
  char rg[10];
  printf("\n");
  printf("Digite o RG do paciente que deseja consultar: ");
  fgets(rg, sizeof(rg), stdin);
  rg[strcspn(rg, "\n")] = '\0';

  ELista *atual = lista->inicio;
  while (atual != NULL) {
    if (strcmp(atual->dados.rg, rg) == 0) { //É aqui que ocorre a comparação do
                                            // RG inputado para o RG armazenado.
      printf("\n");
      printf("Paciente encontrado:\n");
      printf("Nome: %s\n", atual->dados.nome);
      printf("Idade: %d\n", atual->dados.idade);
      printf("RG: %s\n", atual->dados.rg);
      printf("Data de Entrada: %02d/%02d/%04d\n", atual->dados.entrada.dia,
             atual->dados.entrada.mes, atual->dados.entrada.ano);
      return;
    }
    atual = atual->proximo; // O loop corre até encontrar o paciente procurado.
                            // Caso o paciente não seja encontrado, o loop
                            // terminar e é impresso a mensagem abaixo!
  }
  printf("Paciente não encontrado.\n");
}

// Essa função exibe os pacientes cadastrados na lista.
void exibirPacientes(Lista *lista) {
  if (lista->inicio == NULL) {
    printf("Nenhum paciente cadastrado.\n");
    return;
  }

  ELista *atual = lista->inicio; //É aqui que a lista é percorrida
  while (atual !=
         NULL) { // Percorrendo a lista e imprimindo os dados dos pacientes
    printf("\n");
    printf("Nome: %s\n", atual->dados.nome);
    printf("Idade: %d\n", atual->dados.idade);
    printf("RG: %s\n", atual->dados.rg);
    printf("Data de Entrada: %02d/%02d/%04d\n", atual->dados.entrada.dia,
           atual->dados.entrada.mes, atual->dados.entrada.ano);
    printf("--------------------\n");
    atual = atual->proximo; // Avança para o próximo paciente da lista
  }
}

// Nessa função nós atualizamos os dados de um paciente em específico que é
// encontrado pelo RG.
void atualizarPaciente(Lista *lista) {
  char rg[10];
  printf("\n");
  printf("Digite o RG do paciente que deseja atualizar: ");
  fgets(rg, sizeof(rg), stdin);
  rg[strcspn(rg, "\n")] = '\0';

  ELista *atual = lista->inicio;
  while (atual != NULL) {
    if (strcmp(atual->dados.rg, rg) == 0) {
      printf("\n");
      printf("Atualizando dados do paciente %s:\n", atual->dados.nome);
      printf("Nova Idade: ");
      scanf("%d", &atual->dados.idade);
      getchar();
      printf("Nova Data de Entrada (DD MM AAAA): ");
      scanf("%d %d %d", &atual->dados.entrada.dia, &atual->dados.entrada.mes,
            &atual->dados.entrada.ano);
      getchar();

      printf("Dados atualizados com sucesso!\n");
      return;
    }
    atual = atual->proximo;
  }
  printf("Paciente não encontrado.\n");
}

// Nessa função nós removemos o paciente, solicitando ao atendente hospitalar
// que o RG do paciente a ser excluído seja fornecido.
void removerPaciente(Lista *lista) {
  char rg[10];
  printf("\n");
  printf("Digite o RG do paciente que deseja remover: ");
  fgets(rg, sizeof(rg), stdin);
  rg[strcspn(rg, "\n")] = '\0';

  ELista *anterior = NULL;
  ELista *atual = lista->inicio;
  while (atual != NULL) {
    if (strcmp(atual->dados.rg, rg) == 0) {
      if (anterior == NULL) {
        lista->inicio = atual->proximo;
      } else {
        anterior->proximo = atual->proximo;
      }
      free(atual);
      lista->qtde--;
      printf("Paciente removido com sucesso!\n");
      return;
    }
    anterior = atual;
    atual = atual->proximo;
  }
  printf("Paciente não encontrado.\n");
}

//***ATENDIMENTO***
//***ATENDIMENTO***
//***ATENDIMENTO***
//***ATENDIMENTO***
//***ATENDIMENTO***

// A Fila é tranquila, basicamente como conhecemos no mundo fora das máquinas. O
// Primeiro a entrar é o primeiro a sair.

// Nessa funçao nós inicializamos a fila, recebendo um ponteiro da struct Fila
void inicializarFila(Fila *fila) {
  fila->head = NULL;
  fila->tail = NULL;
  fila->qtde = 0;
}

// Nessa funçao enfileiramos os nossos pacientes na fila.
void enqueue(Fila *fila, Paciente paciente) {
  EFila *novo = (EFila *)malloc(sizeof(EFila));
  if (novo == NULL) {
    printf("Erro ao alocar memória.\n");
    return;
  }
  novo->dados = paciente;
  novo->proximo = NULL;

  if (fila->tail == NULL) {
    fila->head = novo;
  } else {
    fila->tail->proximo = novo;
  }
  fila->tail = novo;
  fila->qtde++;

  printf("Paciente enfileirado com sucesso!\n");
}

// Nessa funçao desenfileiramos os pacientes da fila. Só que da maneira correta,
// sempre pelo primeiro da fila.
void dequeue(Fila *fila) {
  if (fila->head == NULL) {
    printf("Fila vazia.\n");
    return;
  }

  EFila *removido = fila->head;
  fila->head = fila->head->proximo;
  if (fila->head == NULL) {
    fila->tail = NULL;
  }
  free(removido);
  fila->qtde--;

  printf("Paciente desenfileirado com sucesso!\n");
}

// Essa função nos mostra a atual situação da fila, ou seja, sua ordem.
void mostrarQueue(Fila *fila) {
  if (fila->head == NULL) {
    printf("Fila vazia.\n");
    return;
  }

  EFila *atual = fila->head;
  while (atual != NULL) {
    printf("\n");
    printf("Nome: %s\n", atual->dados.nome);
    printf("Idade: %d\n", atual->dados.idade);
    printf("RG: %s\n", atual->dados.rg);
    printf("Data de Entrada: %02d/%02d/%04d\n", atual->dados.entrada.dia,
           atual->dados.entrada.mes, atual->dados.entrada.ano);
    printf("--------------------\n");
    atual = atual->proximo;
  }
}

//***PESQUISAR***
//***PESQUISAR***
//***PESQUISAR***
//***PESQUISAR***
//***PESQUISAR***

// A ABB, ou Árvore Binária de Busca é uma estrutura de dados que organiza seus
// elementos em uma estrutura de árvore binária, na qual cada nó possui no
// máximo dois filhos: um filho à esquerda e outro à direita.

// Aqui nós começamos a falar sobre a parte que foi mais "pegada" para nós dois.
// Essa função insere um paciente na árvore binária de busca, mas alterando
// diretamente os nós da árvore
EArvore *inserirPaciente(EArvore *raiz, Paciente paciente, int criterio) {
  // Se a raiz for igual a NULL, significa que a árvore está vazia. E assim
  // criamos um nó.
  if (raiz == NULL) {
    EArvore *novo = (EArvore *)malloc(sizeof(EArvore));
    if (novo == NULL) {
      printf("Erro ao alocar memória.\n");
      return NULL;
    }
    novo->dados = paciente;
    novo->filhoEsq = NULL;
    novo->filhoDir = NULL;
    return novo;
  }
  // Se o critério do paciente for menor que o critério da raiz, o paciente é
  // inserido como filho na esquerda, se o critério for maior, o paciente é
  // inserido como filho na direita.

  if (criterio == 1) { // Ano de registro
    if (paciente.entrada.ano < raiz->dados.entrada.ano)
      raiz->filhoEsq = inserirPaciente(raiz->filhoEsq, paciente, criterio);
    else
      raiz->filhoDir = inserirPaciente(raiz->filhoDir, paciente, criterio);
  } else if (criterio == 2) { // Mês de registro
    if (paciente.entrada.mes < raiz->dados.entrada.mes)
      raiz->filhoEsq = inserirPaciente(raiz->filhoEsq, paciente, criterio);
    else
      raiz->filhoDir = inserirPaciente(raiz->filhoDir, paciente, criterio);
  } else if (criterio == 3) { // Dia de registro
    if (paciente.entrada.dia < raiz->dados.entrada.dia)
      raiz->filhoEsq = inserirPaciente(raiz->filhoEsq, paciente, criterio);
    else
      raiz->filhoDir = inserirPaciente(raiz->filhoDir, paciente, criterio);
  } else if (criterio == 4) { // Idade
    if (paciente.idade < raiz->dados.idade)
      raiz->filhoEsq = inserirPaciente(raiz->filhoEsq, paciente, criterio);
    else
      raiz->filhoDir = inserirPaciente(raiz->filhoDir, paciente, criterio);
  }

  return raiz;
}

// Essa funçao serve para mostrar os pacientes, numa sequência de acordo com o
// critério que for escolhido.
void in_ordem(EArvore *raiz) {
  if (raiz != NULL) {
    in_ordem(raiz->filhoEsq);
    printf("\n");
    printf("Nome: %s\n", raiz->dados.nome);
    printf("Idade: %d\n", raiz->dados.idade);
    printf("RG: %s\n", raiz->dados.rg);
    printf("Data de Entrada: %02d/%02d/%04d\n", raiz->dados.entrada.dia,
           raiz->dados.entrada.mes, raiz->dados.entrada.ano);
    printf("--------------------\n");
    in_ordem(raiz->filhoDir);
  }
}

// Essa funçao inicializa a árvore binária de busca.
void inicializarArvore(Arvore *arvore) {
  arvore->raiz = NULL;
  arvore->qtde = 0;
}

// Essa função serve para inserir um paciente na árvore binária de busca, mas
// mexendo com sua estrutura, ou seja, raiz e o contador de elementos
void inserirPacienteEstrutura(Arvore *arvore, Paciente paciente, int criterio) {
  arvore->raiz = inserirPaciente(arvore->raiz, paciente, criterio);
  arvore->qtde++;
}

// Essa função serve para construir uma ABB a partir de uma lista de pacientes.
void construirArvore(Arvore *arvore, Lista *lista, int criterio) {
  inicializarArvore(arvore);
  ELista *atual = lista->inicio;
  while (atual != NULL) {
    inserirPacienteEstrutura(arvore, atual->dados, criterio);
    atual = atual->proximo;
  }
}

// Aqui temos as funçoes que mostram os registros relacionados à algum critério
// sendo eles Ano, Mês, Dia e Idade.
void mostrarRegistrosPorAno(Lista *lista) {
  Arvore arvore;
  construirArvore(&arvore, lista, 1);
  in_ordem(arvore.raiz);
}

void mostrarRegistrosPorMes(Lista *lista) {
  Arvore arvore;
  construirArvore(&arvore, lista, 2);
  in_ordem(arvore.raiz);
}

void mostrarRegistrosPorDia(Lista *lista) {
  Arvore arvore;
  construirArvore(&arvore, lista, 3);
  in_ordem(arvore.raiz);
}

void mostrarRegistrosPorIdade(Lista *lista) {
  Arvore arvore;
  construirArvore(&arvore, lista, 4);
  in_ordem(arvore.raiz);
}

//***CARREGAR/SALVAR***
//***CARREGAR/SALVAR***
//***CARREGAR/SALVAR***
//***CARREGAR/SALVAR***
//***CARREGAR/SALVAR***

// Aqui começamos a ver como salvaremos nossos dados em algum lugar!
// Função para carregar um arquivo que foi salvo anteriormente.
void carregarDados(Lista *lista) {
  FILE *arquivo = fopen("pacientes.txt", "r");
  if (arquivo == NULL) {
    printf("Erro ao abrir o arquivo.\n");
    return;
  }

  while (!feof(arquivo)) {
    Paciente p;
    // Aqui temos a leitura dos dados do paciente, que estam no arquivo.
    printf("\n");
    fscanf(arquivo, "%[^,],%d,%[^,],%d/%d/%d\n", p.nome, &p.idade, p.rg,
           &p.entrada.dia, &p.entrada.mes, &p.entrada.ano);

    // Aloca espaço para um novo nó na lista
    ELista *novo = (ELista *)malloc(sizeof(ELista));
    if (novo == NULL) {
      printf("Erro ao alocar memória.\n");
      fclose(arquivo);
      return;
    }
    novo->dados = p;
    novo->proximo = lista->inicio;
    lista->inicio = novo;
    lista->qtde++;
  }

  fclose(arquivo);
  printf("Dados carregados com sucesso!\n");
}

// Função para salvar os dados dos pacientes num arquivo
void salvarDados(Lista *lista) {
  FILE *arquivo = fopen("pacientes.txt", "w");
  if (arquivo == NULL) {
    printf("Erro ao abrir o arquivo.\n");
    return;
  }

  ELista *atual = lista->inicio;
  while (atual != NULL) {
    // Escreve os dados dos pacientes no arquivo num formato preparado
    printf("\n");
    fprintf(arquivo, "%s,%d,%s,%02d/%02d/%04d\n", atual->dados.nome,
            atual->dados.idade, atual->dados.rg, atual->dados.entrada.dia,
            atual->dados.entrada.mes, atual->dados.entrada.ano);
    atual = atual->proximo;
  }

  fclose(arquivo);
  printf("Dados salvos com sucesso!\n");
}

// Função para exibir os dados sobre os alunos que criaram o código.
void exibirInformacoesAlunos() {
  printf("\n");
  printf("PROJETO DESENVOLVIDO POR:\n");
  printf("--------------------------------\n");
  printf("Aluno: Anna Carolina R. P. Zomer\n");
  printf("Ciclo: 4º Semestre\n");
  printf("Curso: Ciência da Computação\n");
  printf("Disciplina: Estrutura de Dados\n");
  printf("Data: 24/05/2024\n");
  printf("--------------------------------\n");
  printf("Aluno: Humberto de O. Pellegrini\n");
  printf("Ciclo: 4º Semestre\n");
  printf("Curso: Ciência da Computação\n");
  printf("Disciplina: Estrutura de Dados\n");
  printf("Data: 24/05/2024\n");
  printf("--------------------------------\n");
}

//***MENUS***
//***MENUS***
//***MENUS***
//***MENUS***
//***MENUS***

// Menu para cadastrar e outras funções relacionadas
void menuCadastro(Lista *lista) {
  int opcao;
  do {
    printf("\nMenu de Cadastro:\n");
    printf("1. Cadastrar novo paciente\n");
    printf("2. Consultar paciente cadastrado\n");
    printf("3. Mostrar lista completa de pacientes\n");
    printf("4. Atualizar dados de paciente\n");
    printf("5. Remover paciente\n");
    printf("0. Voltar\n");
    printf("Escolha uma opcao: ");
    scanf("%d", &opcao);
    getchar();

    switch (opcao) {
    case 1:
      cadastrarPaciente(lista);
      break;
    case 2:
      consultarPaciente(lista);
      break;
    case 3:
      exibirPacientes(lista);
      break;
    case 4:
      atualizarPaciente(lista);
      break;
    case 5:
      removerPaciente(lista);
      break;
    case 0:
      printf("Voltando ao menu principal...\n");
      break;
    default:
      printf("Opcao invalida!\n");
      break;
    }
  } while (opcao != 0);
}

// Menu do atendimento dos pacientes
void menuAtendimento(Fila *fila, Lista *lista) {
  int opcao;
  do {
    printf("\nMenu de Atendimento:\n");
    printf("1. Enfileirar paciente\n");
    printf("2. Desenfileirar paciente\n");
    printf("3. Mostrar fila\n");
    printf("0. Voltar\n");
    printf("Escolha uma opcao: ");
    scanf("%d", &opcao);
    getchar();

    switch (opcao) {
    case 1: {
      char rg[10];
      printf("Digite o RG do paciente que quer colocar na fila (sem . e sem "
             "espaços): ");
      fgets(rg, sizeof(rg), stdin);
      rg[strcspn(rg, "\n")] = '\0';

      ELista *atual = lista->inicio;
      while (atual != NULL) {
        if (strcmp(atual->dados.rg, rg) == 0) {
          enqueue(fila, atual->dados);
          break;
        }
        atual = atual->proximo;
      }
      if (atual == NULL) {
        printf("Paciente nao encontrado.\n");
      }
      break;
    }
    case 2:
      dequeue(fila);
      break;
    case 3:
      mostrarQueue(fila);
      break;
    case 0:
      printf("Voltando ao menu principal...\n");
      break;
    default:
      printf("Opcao invalida!\n");
      break;
    }
  } while (opcao != 0);
}

// Menu da Função Pesquisa
void menuPesquisa(Lista *lista) {
  int opcao;
  do {
    printf("\nMenu de Pesquisa:\n");
    printf("1. Mostrar registros ordenados por ano de registro\n");
    printf("2. Mostrar registros ordenados por mes de registro\n");
    printf("3. Mostrar registros ordenados por dia de registro\n");
    printf("4. Mostrar registros ordenados por idade do paciente\n");
    printf("0. Voltar\n");
    printf("Escolha uma opcao: ");
    scanf("%d", &opcao);
    getchar();

    switch (opcao) {
    case 1:
      mostrarRegistrosPorAno(lista);
      break;
    case 2:
      mostrarRegistrosPorMes(lista);
      break;
    case 3:
      mostrarRegistrosPorDia(lista);
      break;
    case 4:
      mostrarRegistrosPorIdade(lista);
      break;
    case 0:
      printf("Voltando ao menu principal...\n");
      break;
    default:
      printf("Opcao invalida!\n");
      break;
    }
  } while (opcao != 0);
}

// Menu do carregamento e salvamentos dos arquivos.
void menuCarregarSalvar(Lista *lista) {
  int opcao;
  do {
    printf("\nMenu de Carregar/Salvar:\n");
    printf("1. Carregar dados de um arquivo\n");
    printf("2. Salvar dados em um arquivo\n");
    printf("0. Voltar\n");
    printf("Escolha uma opcao: ");
    scanf("%d", &opcao);
    getchar();

    switch (opcao) {
    case 1:
      carregarDados(lista);
      break;
    case 2:
      salvarDados(lista);
      break;
    case 0:
      printf("Voltando ao menu principal...\n");
      break;
    default:
      printf("Opcao invalida!\n");
      break;
    }
  } while (opcao != 0);
}

int main() {
  Lista lista;
  Fila fila;
  int opcao;

  inicializarLista(&lista);
  inicializarFila(&fila);

  do {
    printf("\nMenu Principal:\n");
    printf("1. Cadastro\n");
    printf("2. Atendimento\n");
    printf("3. Pesquisa\n");
    printf("4. Carregar/Salvar\n");
    printf("5. Sobre os Alunos\n");
    printf("0. Sair\n");
    printf("Escolha uma opcao: ");
    scanf("%d", &opcao);
    getchar();

    switch (opcao) {
    case 1:
      menuCadastro(&lista);
      break;
    case 2:
      menuAtendimento(&fila, &lista);
      break;
    case 3:
      menuPesquisa(&lista);
      break;
    case 4:
      menuCarregarSalvar(&lista);
      break;
    case 5:
      exibirInformacoesAlunos(&lista);
      break;
    case 0:
      printf("Saindo...\n");
      break;
    default:
      printf("Opcao invalida!\n");
      break;
    }
  } while (opcao != 0);

  return 0;
}

```
</details>

## 🍃 Detalhes do Projeto 🍃

### 📍MENU PRINCIPAL
- No `Menu Principal` temos as opções a serem escolhidas:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MENU_PRINCIPAL.png" />

### 📍MENU CADASTRAR
- Assim que entramos na opção `Cadastro` temos as seguintes opções:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MENU_CADASTRO.png" />

#### CADASTRAR ⬇️
- Cadastramos um novo paciente. - `Opção 01`
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/CADASTRAR_PACIENTE.png" />

#### CONSULTAR ⬇️
- Consultamos um paciente pelo número de RG. - `Opção 02`

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/CONSULTAR_PACIENTE.png" />

#### MOSTRAR ⬇️
- Mostramos todos os pacientes cadastrados. - `Opção 03`

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MOSTRAR_PACIENTES.png" />

#### ATUALIZAR ⬇️
- Atualizamos os dados `Idade` e `Data de Entrada` do paciente que foi localizado pelo número de RG. - `Opção 04`

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/ATUALIZAR_PACIENTE.png" />

  - Assim que fica o `Antes x Depois` do arquivo de salvamento dos pacientes:
  <div style="display: flex; justify-content: center; align-items: center; max-width: 50px;">
      <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/SALVOS_ANTES.png" style="margin-right: 5px;" />
      <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/SALVOS_DEPOIS.png" style="margin-left: 5px;" />
  </div>

#### REMOVER ⬇️
- Atualizamos os dados `Idade` e `Data de Entrada` do paciente que foi localizado pelo número de RG. - `Opção 05`

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/REMOVER_PACIENTE.png" />

  - Assim que fica o `Antes x Depois` do arquivo de salvamento dos pacientes:
  <div style="display: flex; justify-content: center; align-items: center; max-width: 50px;">
      <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/ANTES_SALVAR_REMOVER.png" style="margin-right: 10px;" />
      <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/DEPOIS_SALVAR_REMOVER.png" style="margin-left: 10px;" />
  </div>

### 📍MENU ATENDIMENTO
- Assim que entramos na opção `Atendimento` temos as seguintes opções:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MENU_ATENDIMENTO.png" />

#### ENFILEIRAR ⬇️
- Os pacientes são enfileirados por ordem de urgência, ou seja, o 1º fica no topo e por ai vai indo. - `Opção 01` 
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/ENFILEIRAR.png" />

- Os enfileiramentos são mostrados dessa maneira na `Opção 03`: 
  <p align = "middle">
  <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MOSTRAR_ENFILEIRAR.png" />

#### DESENFILEIRAR ⬇️
- Os pacientes são `desenfileirados` por ordem também, ou seja, do 1º ao último. - `Opção 02`

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/DESENFILEIRAR.png" />

- Os desenfileiramentos são mostrados dessa maneira na `Opção 03` (a primeira saiu da lista): 
  <p align = "middle">
  <img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MOSTRAR_DESENFILEIRAR.png" />

### 📍MENU PESQUISA
- Assim que entramos na opção `Pesquisa` temos as seguintes opções:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MENU_PESQUISAR.png" />

#### ANO ⬇️
- Assim que escolhemos a `Opção 01`, os pacientes são mostrados em ordem crescente pelos anos, ou seja, do menor ano ao maior.
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/PESQUISA_ANO.png" />

#### MÊS ⬇️
- Assim que escolhemos a `Opção 02`, os pacientes são mostrados em ordem crescente pelos meses, ou seja, do menor mês ao maior.
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/PESQUISA_MES.png" />

#### DIA ⬇️
- Assim que escolhemos a `Opção 03`, os pacientes são mostrados em ordem crescente pelos dias, ou seja, do menor dia ao maior.
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/PESQUISA_DIA.png" />

#### IDADE ⬇️
- Assim que escolhemos a `Opção 04`, os pacientes são mostrados em ordem crescente pelas idades, ou seja, da menor idade para a maior.
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/PESQUISA_IDADE.png" />

### 📍MENU CARREGAR/SALVAR
- Assim que entramos na opção `Caregar/Salvar` temos as seguintes opções:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/MENU_CARREGAR.png" />

⚠️ **IMPORTANTE:** Fazer a `Opção 01` assim que iniciar o código.

#### CARREGAR ⬇️
- A `Opção 01` serve para carregar o arquivo txt, que possui pacientes cadastrados, para que, ao cadastrar um paciente novo, não se percam os dados dos anteriores, e para que o código, principalmente as opções de pesquisa, atendimento e consulta, funcionem perfeitamente.
 

#### SALVAR ⬇️
- A `Opção 02` serve para salvar os pacientes novos e todas as atualizações que foram feitas nos cadastros. 
 
<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/SALVAR.png" />

### 📍MENU SOBRE
- Por fim, ao entrarmos na opção `Sobre os Alunos`, é mostrado as informações dos alunos que desenvolveram o código:

<p align = "middle">
<img src="https://github.com/z0mer/PJ.HOSP_C/blob/main/img/SOBRE.png" />

## 🧑🏻‍💻 Autores do Projeto 🧑🏻‍💻

#### Anna Carolina Zomer ⬇️
[![E-mail](https://img.shields.io/badge/GitHub-181717.svg?style=for-the-badge&logo=GitHub&logoColor=white)](https://github.com/z0mer)

#### Humberto Pellegrini ⬇️
[![E-mail](https://img.shields.io/badge/GitHub-181717.svg?style=for-the-badge&logo=GitHub&logoColor=white)](https://github.com/Humbertin07)

![Footer](https://capsule-render.vercel.app/api?type=waving&height=121&color=A9A9A9&text=👋🏻%20Esperamos%20que%20tenham%20gostado!%20👋🏻&fontSize=35&fontColor=D3D3D3&section=footer&fontAlignY=45)
