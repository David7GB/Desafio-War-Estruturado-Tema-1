# Desafio-War-Estruturado-Tema-1
trabalho

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

/* ===========================================================
   ESTRUTURAS
   =========================================================== */

typedef struct {
    char nome[40];
    int dono;      // ID do jogador dono do território
    int tropas;
} Territorio;

typedef struct {
    char descricao[120];
    int concluida;
} Missao;


/* ===========================================================
   FUNÇÕES DE ALOCAÇÃO
   =========================================================== */

// Aloca dinamicamente um vetor de territórios
Territorio* criarTerritorios(int qtd) {
    Territorio* t = malloc(qtd * sizeof(Territorio));
    if (!t) {
        printf("Erro ao alocar memoria para territorios.\n");
        exit(1);
    }
    return t;
}

// Aloca dinamicamente um vetor de missões
Missao* criarMissoes(int qtd) {
    Missao* m = malloc(qtd * sizeof(Missao));
    if (!m) {
        printf("Erro ao alocar memoria para missoes.\n");
        exit(1);
    }
    return m;
}


/* ===========================================================
   FUNÇÕES DE LÓGICA DO JOGO
   =========================================================== */

// Valida se o jogador pode atacar o território de destino
int podeAtacar(Territorio* territorios, int jogador, int origem, int destino) {

    // O território de origem deve pertencer ao jogador atacante
    if (territorios[origem].dono != jogador) {
        return 0;
    }

    // O território de destino deve pertencer a outro jogador
    if (territorios[destino].dono == jogador) {
        return 0;
    }

    return 1;
}

// Executa um ataque (exemplo simplificado)
void atacar(Territorio* territorios, int jogador, int origem, int destino) {

    if (!podeAtacar(territorios, jogador, origem, destino)) {
        printf("\nAtaque invalido!\n");
        return;
    }

    int ataque = rand() % 6 + 1;   // dado de 1 a 6
    int defesa = rand() % 6 + 1;

    printf("\nAtaque: %d | Defesa: %d\n", ataque, defesa);

    if (ataque > defesa) {
        printf("Territorio conquistado!\n");
        territorios[destino].dono = jogador;
    } else {
        printf("Ataque falhou.\n");
    }
}


/* ===========================================================
   FUNÇÃO PARA LIBERAR MEMÓRIA
   =========================================================== */

void liberarMemoria(Territorio* territorios, Missao* missoes) {
    free(territorios);
    free(missoes);
}


/* ===========================================================
   PROGRAMA PRINCIPAL
   =========================================================== */

int main() {

    srand(time(NULL));  // inicializa gerador de números aleatórios

    int qtdTerritorios = 5;
    int qtdMissoes = 3;

    // Criando elementos do jogo
    Territorio* territorios = criarTerritorios(qtdTerritorios);
    Missao* missoes = criarMissoes(qtdMissoes);

    // Exemplo de preenchimento
    for (int i = 0; i < qtdTerritorios; i++) {
        sprintf(territorios[i].nome, "Territorio %d", i + 1);
        territorios[i].dono = i % 2;  // alterna jogador 0 e 1
        territorios[i].tropas = 5;
    }

    strcpy(missoes[0].descricao, "Conquistar 2 territorios inimigos.");
    strcpy(missoes[1].descricao, "Ocupar 3 territorios.");
    strcpy(missoes[2].descricao, "Eliminar o jogador adversario.");

    // Exemplo de ataque: jogador 0 tenta atacar o território 1 a partir do 0
    atacar(territorios, 0, 0, 1);

    // Finaliza o jogo e libera tudo
    liberarMemoria(territorios, missoes);

    return 0;
}
