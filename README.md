# atividade-11
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ALUNOS 5
#define TAMANHO_NOME 50
#define NUM_NOTAS 4

typedef struct {
    int matricula;
    char nome[TAMANHO_NOME];
    int idade;
    float notas[NUM_NOTAS];
    float nota_total;
    float media;
    char resultado[15];
} Aluno;
Aluno alunos[MAX_ALUNOS];
int quantidade_alunos = 0;
void calcular_notas(Aluno *aluno) {
    aluno->nota_total = 0;
    for (int i = 0; i < NUM_NOTAS; i++) {
        aluno->nota_total += aluno->notas[i];
    }
    aluno->media = aluno->nota_total / NUM_NOTAS;

    if (aluno->media < 4) {
        strcpy(aluno->resultado, "Reprovado");
    } else if (aluno->media >= 4 && aluno->media <= 6) {
        strcpy(aluno->resultado, "Recuperacao");
    } else {
        strcpy(aluno->resultado, "Aprovado");
    }
}
void adicionar_aluno() {
    if (quantidade_alunos >= MAX_ALUNOS) {
        printf("Limite de alunos atingido!\n");
        return;
    }
    Aluno novo_aluno;
    printf("Digite a matrícula do aluno: ");
    scanf("%d", &novo_aluno.matricula);
    printf("Digite o nome do aluno: ");
    scanf(" %[^\n]", novo_aluno.nome); // lê o nome com espaços
    printf("Digite a idade do aluno: ");
    scanf("%d", &novo_aluno.idade);

    for (int i = 0; i < NUM_NOTAS; i++) {
        do {
            printf("Digite a nota %d do aluno (0 a 10): ", i + 1);
            scanf("%f", &novo_aluno.notas[i]);
            if (novo_aluno.notas[i] < 0 || novo_aluno.notas[i] > 10) {
                printf("Nota inválida! Insira um valor entre 0 e 10.\n");
            }
        } while (novo_aluno.notas[i] < 0 || novo_aluno.notas[i] > 10);
    }

    calcular_notas(&novo_aluno);
    alunos[quantidade_alunos++] = novo_aluno;
    printf("Aluno adicionado com sucesso!\n");
}
void listar_alunos() {
    if (quantidade_alunos == 0) {
        printf("Nenhum aluno cadastrado!\n");
        return;
    }
    printf("Lista de alunos cadastrados:\n");
    for (int i = 0; i < quantidade_alunos; i++) {
        printf("Matrícula: %d\n", alunos[i].matricula);
        printf("Nome: %s\n", alunos[i].nome);
        printf("Idade: %d\n", alunos[i].idade);
        for (int j = 0; j < NUM_NOTAS; j++) {
            printf("Nota %d: %.2f\n", j + 1, alunos[i].notas[j]);
        }
        printf("Nota Total: %.2f\n", alunos[i].nota_total);
        printf("Média: %.2f\n", alunos[i].media);
        printf("Resultado: %s\n", alunos[i].resultado);
        printf("------------------------\n");
    }
}
void editar_aluno() {
    if (quantidade_alunos == 0) {
        printf("Nenhum aluno cadastrado!\n");
        return;
    }
    int matricula;
    printf("Digite a matrícula do aluno a ser editado: ");
    scanf("%d", &matricula);
    int encontrado = 0;
    for (int i = 0; i < quantidade_alunos; i++) {
        if (alunos[i].matricula == matricula) {
            encontrado = 1;
            printf("Editando aluno: %s\n", alunos[i].nome);
            for (int j = 0; j < NUM_NOTAS; j++) {
                do {
                    printf("Digite a nova nota %d do aluno (0 a 10): ", j + 1);
                    scanf("%f", &alunos[i].notas[j]);
                    if (alunos[i].notas[j] < 0 || alunos[i].notas[j] > 10) {
                        printf("Nota inválida! Insira um valor entre 0 e 10.\n");
                    }
                } while (alunos[i].notas[j] < 0 || alunos[i].notas[j] > 10);
            }
            calcular_notas(&alunos[i]);
            printf("Aluno editado com sucesso!\n");
            break;
        }
    }
    if (!encontrado) {
        printf("Aluno não encontrado!\n");
    }
}
int main() {
    int opcao;
    do {
        printf("\nSistema de Cadastro de Alunos\n");
        printf("1. Adicionar Aluno\n");
        printf("2. Listar Alunos\n");
        printf("3. Editar Aluno\n");
        printf("4. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                adicionar_aluno();
                break;
            case 2:
                listar_alunos();
                break;
            case 3:
                editar_aluno();
                break;
            case 4:
                printf("Saindo...\n");
                break;
            default:
                printf("Opcao invalida!\n");
                break;
        }
    } while (opcao != 4);

    return 0;
}
