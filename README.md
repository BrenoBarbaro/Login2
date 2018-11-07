#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <string.h>
#include <windows.h>
#include <conio.h>

typedef struct produtos
{
    char codProduto[5];
    char nomeProduto[21];
    double valorProduto;
    struct Lista *proximo;
} Lista;

typedef struct compras
{
    char codProduto[5];
    char nomeProduto[21];
    double valorProduto;
    struct ListaCompras *proximo;
} ListaCompras;

typedef struct usuarios
{
    char usuarioCadastrado[11];
    char senhaCadastrado[11];
    struct Login *proximo;
}Login;

Lista *inicio = NULL;
ListaCompras *cprodutos = NULL;
Login *ListaUsers = NULL;

void gotoxy(int x, int y){
     SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),(COORD){x-1,y-1});
}

void menu();
void cadastroUsuario();
void listaFuncionario();
void PesquisaFuncionario();
void ProdutosVendidos();
void MostraLogin();
void sair();

//-------------------------------------------------------------------------------

int main(void)
{
    FILE *inic;
    inic = fopen("Usuarios.txt","rb");

    if(inic == NULL)
    {
        inic = fopen("Usuarios.txt","ab");
        cadastroUsuario();
    }
    else
    {
        MostraLogin();  // --> Funções seguintes em MostraLogin
    }
    fclose(inic);

    return 0;
}

void menu()
{
    char op;

    do
    {
        fflush(stdin);
        system("cls");
        printf("******************************************************");
        printf("\n*                 NewHorizon Market                  *");
        printf("\n*                                                    *");
        printf("\n*      1 - Cadastrar Funcionario                     *");
        printf("\n*      2 - Listar Funcionarios                       *");
        printf("\n*      3 - Pesquisar Funcionario                     *");
        printf("\n*      4 - Nomes Iguais                              *");
        printf("\n*                                                    *");
        printf("\n*      5 - Sair                                      *");
        printf("\n*                                                    *");
        printf("\n******************************************************");
        printf("\n*      Opcao:                                        *");
        printf("\n******************************************************");
        op = getch();
        switch(op)
        {

            case '1': cadastroUsuario(); break;

            case '2': listaFuncionario(); break;

            case '3': PesquisaFuncionario(); break;

            case '4': ProdutosVendidos(); break;

            case '5': sair(); exit(0); break;

            default: printf("Opcao Invalida\n\nPressione Enter para continuar!"); break;
        }
    }while(op != 5);
}

void cadastraUsuario()
{
	Login cUsers;
	
	FILE *fp;
	fp = fopen("Usuarios.txt","wb");
	fclose(fp);
	
	if((fp = fopen("Usuarios.txt","ab"))==NULL)
	{
		printf("Problema na abertura do arquivo.\n\n");
		exit(1);
	}
	
	printf("---> Cadastro de usuarios <---\n\n");
	fflush(stdin);
	printf("Usuario: ");
	gets(cUsers.usuarioCadastrado);
	fflush(stdin);
	printf("Senha: ");
	gets(cUsers.senhaCadastrada);
	fflush(stdin);
	
	fwrite(&cUsers,sizeof(Login),1,fp);
	fclose(fp);
	printf("\n\n");
	
	printf("---> %s <---",cUsers.usuarioCadastrado);
	printf("---> %s <---",cUsers.senhaCadastrada);
	
}


void MostraLogin()
{
	Login cUsers;
	char usuario[12],usuarioTemp[12];
    char senha[8],senhaTemp[8];
	
	FILE *fp;
	
	if((fp = fopen("Usuarios.txt","rb"))==NULL)
	{
		printf("Problema na abertura do arquivo.\n\n");
	}
	printf("\n\n");
	while(fread(&cUsers,sizeof(Login),1,fp)==1)
	{
		
	}
    fclose(fp);
	printf("%s\t%s",cUsers.usuarioCadastrado,cUsers.senhaCadastrada);

	printf("\n---> Login <---\n\n");
	fflush(stdin);
	printf("Usuario: ");
	fgets(usuario,sizeof(usuario),stdin);
	fflush(stdin);
	printf("Senha: ");
	fgets(senha,sizeof(senha),stdin);
	
	usuario[strcspn(usuario,"\r\n")] = '\0';
	senha[strcspn(senha,"\r\n")] = '\0';
	
	if((strcmp(cUsers.usuarioCadastrado,usuario)==0) && (strcmp(cUsers.usuarioCadastrado,usuario)==0))
	{
		printf("\n\nOkay");
	}else{
		printf("\n\nUsuario ou Senha Invalido!\n\n");
		exit(1);
	}
	getchar();
}

void listaFuncionario()
{
    FILE *pUser;
    Login ctt;

    pUser = fopen("Usuarios.txt","rb");

    if(pUser == NULL)
    {
        printf("Problema na abertura do arquivo!\n");
    }
    else
    {printf("\n\n");
        while( fread(&ctt, sizeof(Login),1,pUser) == 1)
        {
            printf("------------------------------------------------------\n");
            printf("Usuario: %s\tSenha: %s\n",ctt.usuarioCadastrado,ctt.senhaCadastrado);
            printf("------------------------------------------------------\n\n");
        }
    }
    fclose(pUser);
    fflush(stdin);
    getch();
}

void PesquisaFuncionario()
{
    FILE *pUser;
    Login ctt;
    char usuario[30];

    pUser = fopen("Usuarios.txt","rb");

    if(pUser == NULL)
    {
        printf("Problemas na abertura do arquivo!\n");
    }
    else
    {
        fflush(stdin);
        printf("\n\nDigite o nome a pesquisar: ");
        gets(usuario);

        while( fread(&ctt,sizeof(Login),1,pUser)==1)
        {
            if(strcmp(usuario, ctt.usuarioCadastrado)==0)
            {
                printf("------------------------------------------------------\n");
                printf("Usuario: %s\tSenha: %s\n",ctt.usuarioCadastrado,ctt.senhaCadastrado);
                printf("------------------------------------------------------\n\n");
            }
        }
    }
    fclose(pUser);
    getch();
}

void ProdutosVendidos()
{
    FILE *pUser;
    Login ctt;

    pUser = fopen("Usuarios.txt","rb");

    if(pUser == NULL)
    {
        printf("Problemas na abertura do arquivo!\n");
    }
    else
    {
        char selecionaProduto[30];

        printf("Digite o Produto: ");
        scanf("%s",selecionaProduto);

        while(fread(&ctt,sizeof(Login),1,pUser)==1)
        {
            if(selecionaProduto[0] == ctt.usuarioCadastrado[0])
            {
                printf("------------------------------------------------------\n");
                printf("Usuario: %s\tSenha: %s\n",ctt.usuarioCadastrado,ctt.senhaCadastrado);
                printf("------------------------------------------------------\n\n");
            }
        }
    }
    fclose(pUser);
    getch();
}

void sair()
{
    system("CLS");
    printf("*************************************************");
    printf("\n*                                               *");
    printf("\n*     Obrigado , NewHorizon Market agradece     *");
    printf("\n*         sua presenca, volte sempre.           *");
    printf("\n*                                               *");
    printf("\n*************************************************\n");
}
