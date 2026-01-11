# batalha-naval
criação do jogo batalha naval
#include <stdio.h>

#define TAM_TAB 10
#define TAM_HAB 5

// Função para imprimir o tabuleiro
void imprimirTabuleiro(int tabuleiro[TAM_TAB][TAM_TAB]) {
    printf("\n=== TABULEIRO ===\n\n");
    for (int i = 0; i < TAM_TAB; i++) {
        for (int j = 0; j < TAM_TAB; j++) {
            if (tabuleiro[i][j] == 0)
                printf("~ "); // Água
            else if (tabuleiro[i][j] == 3)
                printf("N "); // Navio
            else if (tabuleiro[i][j] == 5)
                printf("* "); // Área da habilidade
        }
        printf("\n");
    }
}

// Criação da matriz de habilidade em cone
void criarCone(int cone[TAM_HAB][TAM_HAB]) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (j >= (TAM_HAB / 2) - i && j <= (TAM_HAB / 2) + i)
                cone[i][j] = 1;
            else
                cone[i][j] = 0;
        }
    }
}

// Criação da matriz de habilidade em cruz
void criarCruz(int cruz[TAM_HAB][TAM_HAB]) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (i == TAM_HAB / 2 || j == TAM_HAB / 2)
                cruz[i][j] = 1;
            else
                cruz[i][j] = 0;
        }
    }
}

// Criação da matriz de habilidade em octaedro (losango)
void criarOctaedro(int octaedro[TAM_HAB][TAM_HAB]) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (abs(i - TAM_HAB / 2) + abs(j - TAM_HAB / 2) <= TAM_HAB / 2)
                octaedro[i][j] = 1;
            else
                octaedro[i][j] = 0;
        }
    }
}

// Função para sobrepor habilidade no tabuleiro
void aplicarHabilidade(
    int tabuleiro[TAM_TAB][TAM_TAB],
    int habilidade[TAM_HAB][TAM_HAB],
    int origemLinha,
    int origemColuna
) {
    for (int i = 0; i < TAM_HAB; i++) {
        for (int j = 0; j < TAM_HAB; j++) {
            if (habilidade[i][j] == 1) {
                int linhaTab = origemLinha + i - TAM_HAB / 2;
                int colunaTab = origemColuna + j - TAM_HAB / 2;

                // Verifica limites do tabuleiro
                if (linhaTab >= 0 && linhaTab < TAM_TAB &&
                    colunaTab >= 0 && colunaTab < TAM_TAB) {

                    // Não sobrescreve navios
                    if (tabuleiro[linhaTab][colunaTab] == 0)
                        tabuleiro[linhaTab][colunaTab] = 5;
                }
            }
        }
    }
}

int main() {
    int tabuleiro[TAM_TAB][TAM_TAB] = {0};

    // Colocando alguns navios manualmente
    tabuleiro[2][2] = 3;
    tabuleiro[2][3] = 3;
    tabuleiro[5][5] = 3;
    tabuleiro[7][1] = 3;

    // Matrizes de habilidades
    int cone[TAM_HAB][TAM_HAB];
    int cruz[TAM_HAB][TAM_HAB];
    int octaedro[TAM_HAB][TAM_HAB];

    // Criar habilidades
    criarCone(cone);
    criarCruz(cruz);
    criarOctaedro(octaedro);

    // Aplicar habilidades em posições fixas
    aplicarHabilidade(tabuleiro, cone, 1, 5);       // Cone
    aplicarHabilidade(tabuleiro, cruz, 5, 5);       // Cruz
    aplicarHabilidade(tabuleiro, octaedro, 7, 7);   // Octaedro

    // Exibir tabuleiro final
    imprimirTabuleiro(tabuleiro);

    return 0;
}
