// PIM
// Usuarios e Senhas Cadastradas: (funcionario1, senha1), (funcionario2, senha2), (funcionario3, senha3)
#include <stdio.h>
#include <string.h>

#define NUM_PRODUTOS 100
#define NUM_FUNCIONARIOS 10

typedef struct {
    int codigo;
    char nome[30];
    double preco;
} Produto;

typedef struct {
    char usuario[30];
    char senha[20]; 
} Funcionario;

void exibirProdutos(Produto produtos[], int numProdutos) {
    printf("\nLista de Produtos Hortifruti:\n");
    for (int i = 0; i < numProdutos; i++) {
        printf("Codigo: %d - %s  R$ %.2f\n", produtos[i].codigo, produtos[i].nome, produtos[i].preco);
    }
}

void adicionarProduto(Produto produtos[], int *numProdutos) {
    if (*numProdutos < NUM_PRODUTOS) {
        Produto novoProduto;
        printf("Digite o codigo do novo produto (ou 0 para voltar ao menu): ");
        scanf("%d", &novoProduto.codigo);
        if (novoProduto.codigo == 0) {
            printf("Voltando ao menu...\n");
            return;
        }
        printf("Digite o nome do novo produto: ");
        getchar();
        fgets(novoProduto.nome, sizeof(novoProduto.nome), stdin);
        novoProduto.nome[strcspn(novoProduto.nome, "\n")] = 0;

        printf("Digite o preco do novo produto: ");
        scanf("%lf", &novoProduto.preco);
        
        produtos[*numProdutos] = novoProduto;
        (*numProdutos)++;
        printf("Produto adicionado com sucesso!\n");
    } else {
        printf("Limite de produtos atingido. Nao foi possivel adicionar.\n");
    }
}

void removerProduto(Produto produtos[], int *numProdutos) {
    if (*numProdutos == 0) {
        printf("Nao ha produtos cadastrados para remover.\n");
        return;
    }

    int codigoProduto;
    printf("Digite o codigo do produto que deseja remover (ou 0 para voltar ao menu): ");
    scanf("%d", &codigoProduto);
    if (codigoProduto == 0) {
        printf("Voltando ao menu...\n");
        return;
    }

    int encontrado = 0;
    for (int i = 0; i < *numProdutos; i++) {
        if (produtos[i].codigo == codigoProduto) {
            encontrado = 1;
            for (int j = i; j < *numProdutos - 1; j++) {
                produtos[j] = produtos[j + 1];
            }
            (*numProdutos)--;
            printf("Produto removido com sucesso!\n");
            break;
        }
    }

    if (!encontrado) {
        printf("Codigo do produto nao encontrado.\n");
    }
}

void corrigirProduto(Produto produtos[], int numProdutos) {
    int codigoProduto;
    printf("Digite o codigo do produto que deseja corrigir (ou 0 para voltar ao menu): ");
    scanf("%d", &codigoProduto);

    if (codigoProduto == 0) {
        printf("Voltando ao menu...\n");
        return;
    }

    int encontrado = 0;
    for (int i = 0; i < numProdutos; i++) {
        if (produtos[i].codigo == codigoProduto) {
            encontrado = 1;

            printf("\nProduto encontrado: %s - R$ %.2f\n", produtos[i].nome, produtos[i].preco);

            char novoNome[30];
            double novoPreco;

            printf("Digite o novo nome do produto (ou mantenha o nome atual para nao alterar): ");
            getchar();
            fgets(novoNome, sizeof(novoNome), stdin);
            novoNome[strcspn(novoNome, "\n")] = 0;

            if (strlen(novoNome) > 0) {
                strcpy(produtos[i].nome, novoNome);
            }

            printf("Digite o novo preco do produto (ou mantenha o preco atual para nao alterar): ");
            scanf("%lf", &novoPreco);
            
            if (novoPreco >= 0) {
                produtos[i].preco = novoPreco;
            }

            printf("Produto corrigido com sucesso!\n");
            break;
        }
    }

    if (!encontrado) {
        printf("Codigo do produto nao encontrado.\n");
    }
}

int autenticarFuncionario(Funcionario funcionarios[], int numFuncionarios) {
    char usuario[20];
    char senha[20];

    printf("Digite o usuario: ");
    scanf("%s", usuario);
    printf("Digite a senha: ");
    scanf("%s", senha);

    for (int i = 0; i < numFuncionarios; i++) {
        if (strcmp(funcionarios[i].usuario, usuario) == 0 && strcmp(funcionarios[i].senha, senha) == 0) {
            return 1; 
        }
    }
    printf("Usuario ou senha incorretos.\n");
    return 0; 
}

void adicionarFuncionario(Funcionario funcionarios[], int *numFuncionarios) {
    if (*numFuncionarios < NUM_FUNCIONARIOS) {
        Funcionario novoFuncionario;
        printf("Digite o novo usuario: ");
        scanf("%s", novoFuncionario.usuario);
        printf("Digite a nova senha: ");
        scanf("%s", novoFuncionario.senha);

        funcionarios[*numFuncionarios] = novoFuncionario;
        (*numFuncionarios)++;
        printf("Funcionario adicionado com sucesso!\n");
    } else {
        printf("Limite de funcionarios atingido. Nao foi possivel adicionar.\n");
    }
}

int main() {
    Produto produtos[NUM_PRODUTOS] = {
        {1, "Maca Fuji (1 Kg)", 7.50},
        {2, "Banana Prata (1 Kg)", 5.20},
        {3, "Tomate Italiano (1 Kg)", 8.30},
        {4, "Cenoura (1 Kg)", 4.80},
        {5, "Batata Inglesa (1 Kg)", 3.90},
        {6, "Abacaxi (unidade)", 6.50},
        {7, "Manga Palmer (1 Kg)", 7.20},
        {8, "Laranja Pera (1 Kg)", 3.50},
        {9, "Alface Americana (unidade)", 3.00},
        {10, "Brocolis (1 Kg)", 9.50},
        {11, "Pimentao Verde (1 Kg)", 5.80},
        {12, "Abobrinha Italiana (1 Kg)", 6.90},
        {13, "Cebola (1 Kg)", 4.20},
        {14, "Pepino (1 Kg)", 3.80},
        {15, "Berinjela (1 Kg)", 5.10},
        {16, "Abacate (1 Kg)", 6.90},
        {17, "Limao Tahiti (1 Kg)", 4.00},
        {18, "Melancia (1 Kg)", 2.50},
        {19, "Maracuja (1 Kg)", 10.90},
        {20, "Morango (1 Kg)", 14.90},
        {21, "Pessego (1 Kg)", 9.80},
        {22, "Nectarina (1 Kg)", 8.50},
        {23, "Goiaba (1 Kg)", 7.20},
        {24, "Mamao Papaya (1 Kg)", 6.80},
        {25, "Kiwi (1 Kg)", 13.50},
        {26, "Uva Italia (1 Kg)", 9.50},
        {27, "Beterraba (1 Kg)", 3.80},
        {28, "Couve Flor (unidade)", 5.90},
        {29, "Espinafre (maco)", 4.50},
        {30, "Repolho Verde (1 Kg)", 4.00},
        {31, "Repolho Roxo (1 Kg)", 5.00},
        {32, "Alho (1 Kg)", 25.00},
        {33, "Cogumelo Champignon (1 Kg)", 18.90},
        {34, "Cebolinha (maco)", 2.00},
        {35, "Salsinha (maco)", 2.50},
        {36, "Coentro (maco)", 2.30},
        {37, "Rucula (maco)", 3.80},
        {38, "Batata Doce (1 Kg)", 4.50},
        {39, "Mandioquinha (1 Kg)", 7.80},
        {40, "Inhame (1 Kg)", 8.50},
        {41, "Jilo (1 Kg)", 4.10},
        {42, "Almeirao (maco)", 3.70},
        {43, "Acelga (1 Kg)", 4.30},
        {44, "Agriao (maco)", 3.50},
        {45, "Chicoria (maco)", 4.20},
        {46, "Caju (1 Kg)", 6.00},
        {47, "Acerola (1 Kg)", 12.50},
        {48, "Jabuticaba (1 Kg)", 14.00},
        {49, "Figo (1 Kg)", 13.90},
        {50, "Pitanga (1 Kg)", 11.20},
        {51, "Roma (1 Kg)", 15.50},
        {52, "Fruta do Conde (1 Kg)", 13.80},
        {53, "Carambola (1 Kg)", 9.50},
        {54, "Seriguela (1 Kg)", 11.00},
        {55, "Lichia (1 Kg)", 25.00},
        {56, "Graviola (1 Kg)", 18.90},
        {57, "Ameixa (1 Kg)", 10.80},
        {58, "Tamarindo (1 Kg)", 12.50},
        {59, "Acai (1 Kg)", 15.00},
        {60, "Castanha do Para (1 Kg)", 32.00},
        {61, "Castanha de Caju (1 Kg)", 45.00},
        {62, "Nozes (1 Kg)", 48.00},
        {63, "Amendoas (1 Kg)", 39.00},
        {64, "Pistache (1 Kg)", 55.00},
        {65, "Damasco Seco (1 Kg)", 22.00},
        {66, "Uva Passa (1 Kg)", 15.00},
        {67, "Tamara (1 Kg)", 20.00},
        {68, "Amora (1 Kg)", 19.00},
        {69, "Framboesa (1 Kg)", 25.00},
        {70, "Mirtilo (Blueberry) (1 Kg)", 35.00},
        {71, "Gengibre (1 Kg)", 18.50},
        {72, "Cacau (1 Kg)", 25.90},
        {73, "Caqui (1 Kg)", 6.90},
        {74, "Pera Portuguesa (1 Kg)", 7.50},
        {75, "Pera Williams (1 Kg)", 9.80},
        {76, "Kiwi Verde (1 Kg)", 12.00},
        {77, "Melao Amarelo (1 Kg)", 4.80},
        {78, "Melao Cantaloupe (1 Kg)", 5.80},
        {79, "Coco Seco (unidade)", 6.00},
        {80, "Coco Verde (unidade)", 5.00},
        {81, "Hortela (maco)", 2.50},
        {82, "Couve Manteiga (maco)", 3.50},
        {83, "Nabo (1 Kg)", 4.20},
        {84, "Chuchu (1 Kg)", 3.30},
        {85, "Maxixe (1 Kg)", 5.00},
        {86, "Quiabo (1 Kg)", 6.10},
        {87, "Mandioca (1 Kg)", 3.80},
        {88, "Aipim (1 Kg)", 4.00},
        {89, "Milho Verde (unidade)", 2.50},
        {90, "Ervilha Torta (1 Kg)", 10.50},
        {91, "Aspargo (1 Kg)", 30.00},
        {92, "Alcaparras (1 Kg)", 50.00},
        {93, "Amendoim (1 Kg)", 18.00},
        {94, "Pinhao (1 Kg)", 12.00},
        {95, "Soja (1 Kg)", 6.00},
        {96, "Lentilha (1 Kg)", 8.90},
        {97, "Grao de Bico (1 Kg)", 9.50},
        {98, "Feijao Carioca (1 Kg)", 7.80},
        {99, "Feijao Preto (1 Kg)", 6.50},
        {100, "Ervilha Seca (1 Kg)", 5.80}
    };

    Funcionario funcionarios[NUM_FUNCIONARIOS] = {
        {"funcionario1", "senha1"},
        {"funcionario2", "senha2"},
        {"funcionario3", "senha3"}
    };

    int numProdutos = 100; 
    int numFuncionarios = 3; 
    int continuar = 1;

    
    if (!autenticarFuncionario(funcionarios, numFuncionarios)) {
        return 0; 
    }

    while (continuar) {
        int opcao;
        printf("\n--- Menu Principal ---\n");
        printf("1. Realizar compra\n");
        printf("2. Adicionar novo produto\n");
        printf("3. Remover produto\n");
        printf("4. Mostrar tabela de produtos\n");
        printf("5. Corrigir produto\n");
        printf("6. Adicionar novo funcionario\n");
        printf("0. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        if (opcao == 1) {
            int codigoProduto, quantidade;
            double total = 0.0;
            int comprando = 1;

            exibirProdutos(produtos, numProdutos);

            while (comprando) {
                printf("\nDigite o codigo do produto (0 para finalizar compra e voltar ao menu): ");
                scanf("%d", &codigoProduto);

                if (codigoProduto == 0) {
                    printf("Finalizando compra e voltando ao menu...\n");
                    break;
                }

                int encontrado = 0;
                for (int i = 0; i < numProdutos; i++) {
                    if (produtos[i].codigo == codigoProduto) {
                        encontrado = 1;
                        printf("Digite a quantidade de %s: ", produtos[i].nome);
                        scanf("%d", &quantidade);

                        if (quantidade < 0) {
                            printf("Quantidade invalida. Tente novamente.\n");
                            continue;
                        }

                        total += produtos[i].preco * quantidade;
                        printf("Adicionado: %s x%d - Total parcial: R$ %.2f\n", produtos[i].nome, quantidade, total);
                        break;
                    }
                }

                if (!encontrado) {
                    printf("Codigo do produto nao encontrado. Tente novamente.\n");
                }
            }

            printf("\nTotal a ser pago: R$ %.2f\n", total);
        } else if (opcao == 2) {
            int adicionarNovoProduto = 1;

            while (adicionarNovoProduto) {
                exibirProdutos(produtos, numProdutos);
                adicionarProduto(produtos, &numProdutos);
                printf("Deseja adicionar outro produto? (1 - Sim, 0 - Voltar ao menu): ");
                scanf("%d", &adicionarNovoProduto);
            }
        } else if (opcao == 3) {
            exibirProdutos(produtos, numProdutos);
            removerProduto(produtos, &numProdutos);
        } else if (opcao == 4) {
            exibirProdutos(produtos, numProdutos);
        } else if (opcao == 5) {
            exibirProdutos(produtos, numProdutos);
            corrigirProduto(produtos, numProdutos);
        } else if (opcao == 6) {
            adicionarFuncionario(funcionarios, &numFuncionarios);
        } else if (opcao == 0) {
            continuar = 0;
        } else {
            printf("Opcao invalida. Tente novamente.\n");
        }
    }

    printf("Obrigado por usar o sistema!\n");
    return 0;
}
