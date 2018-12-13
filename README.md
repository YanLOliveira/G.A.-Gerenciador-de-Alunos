#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <string.h>


typedef struct alunos ALUNO;
struct alunos{
char nome[30];
float nota[3];
int faltas;

};
void Cabecalho();
void Cadastrar();
void Listar (int tipo);
void Pesquisar();
void Relatorio();
void RemoveAluno();
void MudaNota();
void MudaFalta();
void AlteraCadastro();

// listar alunos lista reprovaodos

int main (){
    setlocale(LC_ALL, "Portuguese");
 int opcao;

 do{
     Cabecalho();
     printf("\n1 - Cadastrar Aluno");
     printf("\n2 - Alterar Cadastro");
     printf("\n3 - Pesquisar Alunos");
     printf("\n4 - Listar Alunos");
     printf("\n5 - Relatorio");
     printf("\n0 - Sair");
     printf("\nOpcao: ");
     scanf("%d", &opcao);
     switch(opcao){
       case 1:

            Cadastrar();
        break;

       case 2:
             AlteraCadastro();
        break;

       case 3:

             Pesquisar();
        break;

       case 4:

            Listar(1);
        break;

       case 5:

           Relatorio();

        break;

       case 0:

            printf("Obrigado por sua visita\n");
            getch();

        break;
       default:

            printf("Opcao invalida");
            getch();

       break;

     }




    }while(opcao != 0);


return 0;
}





Cabecalho(){
    system("cls");
    printf("-------------------------------------------\n");
    printf("           Gestão de Alunos\n");
    printf("-------------------------------------------\n");

}

Cadastrar(){
    FILE *arq;
    ALUNO al;

    arq = fopen("alunos.txt","ab");

    if(arq == NULL){
        printf("Arquivo não encontrado!\n");

    }else{
         do{
            Cabecalho();
            fflush(stdin);
            printf("\nDigite o nome : ");
            gets(al.nome);
            for(int i = 0;i < 3; i++){
                printf("\nDigite a nota da n%d : ",i+1);
                scanf("%f",&al.nota[i]);

            }
            printf("\nDigite a Quantidade de faltas do Aluno : ");
            scanf("%d",&al.faltas);

            fwrite( &al , sizeof(ALUNO) , 1 , arq );

            printf("\nDeseja Continuar (s/n)?");

         }while(getche() == 's');
    fclose(arq);

    }

}

Listar(int tipo){
        FILE *arq;
        ALUNO al;

        arq = fopen("alunos.txt","rb");

        Cabecalho();
        if(arq == NULL){
            printf("Arquivo não encontrado!\n");

        }else{
            int indice =1;
            while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){
                printf("\nCod: %d",indice);
                printf("\n\tNome %s\n",al.nome);
                for(int i = 0;i < 3; i++){
                printf(" \tNota n%d : %.2f ",i+1,al.nota[i]);
                }
                printf("\n\tFaltas : %d\n",al.faltas);
                printf("-------------------------------");

                indice++;
            }
    }
    fclose(arq);
    if(tipo == 1){
        printf("\n");
        getch();
    }
}




Pesquisar(){
        FILE *arq;
        ALUNO al;
        char nome [30];


        arq = fopen("alunos.txt","rb");

        Cabecalho();
        if(arq == NULL){
            printf("Arquivo não encontrado!\n");

        }else{
              fflush(stdin);
              printf("\nDigite o nome : ");
              gets(nome);
              int indice =1;
              while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){
                if(strcasecmp(nome,al.nome)== 0){
                    printf("\nCod: %d",indice);
                    printf("\n\tNome %s\n",al.nome);
                    for(int i = 0;i < 3; i++){
                    printf(" \tNota n%d : %.2f ",i+1,al.nota[i]);
                    }
                    printf("\n\tFaltas : %d\n",al.faltas);
                    printf("-------------------------------");


                }

               indice++;
              }


        }
        fclose(arq);

        getch();

}

Relatorio(){
    FILE *arq,*arq2;
    ALUNO al;
    char nome[30];
    float nota[3];
    int faltas;


    arq = fopen("alunos.txt","rb");
    arq2 = fopen("Relatorio.txt","w");

    if(arq == NULL||arq2==NULL){
            printf("Arquivo fonte não encontrado!\n");

        }else{

              //fprintf(arq2,"\tNome\tFaltas\tNota 1\tNota 2\tNota 3\n");


             while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){

                strcpy(nome,al.nome);
                for(int i = 0;i < 3; i++){

                    nota[i]=al.nota[i];

                }

                faltas=al.faltas;

               fprintf(arq2,"%s\t%d\t%.2f\t%.2f\t%.2f\n",nome,faltas,nota[0],nota[1],nota[2]);


             }

        }
        fclose(arq);
        fclose(arq2);


        printf("\nRelatorio gerado");
        getch();


}

RemoveAluno(){
          FILE *arq,*arq3;
          ALUNO al;
          int cod,indice;

          arq = fopen("alunos.txt","rb");
          arq3 = fopen("copiaaluno.txt","wb");

          if(arq == NULL||arq3==NULL){

               printf("Arquivo fonte não encontrado!\n");

         }else{
                Listar(0);
                printf("\nDigite o codigo do aluno que deseja remover: ");
                scanf("%d",&cod);
                indice= 1;
                while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){

                if(cod!=indice){

                  fwrite( &al , sizeof(ALUNO) , 1 , arq3 );
                  printf("%d",indice);
                }
                indice++;



         }
    }
         fclose(arq);
         fclose(arq3);

         remove("alunos.txt");
         rename("copiaaluno.txt","alunos.txt");


}


MudaFalta(){
          FILE *arq,*arq3;
          ALUNO al;
          int cod,indice,novaFalta;

          arq = fopen("alunos.txt","rb");
          arq3 = fopen("copiaaluno.txt","wb");

          if(arq == NULL||arq3==NULL){

               printf("Arquivo fonte não encontrado!\n");

         }else{
                Listar(0);
                printf("\nDigite o codigo do aluno que deseja alterar a falta: ");
                scanf("%d",&cod);
                indice= 1;
                while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){

                if(cod!=indice){

                  fwrite( &al , sizeof(ALUNO) , 1 , arq3 );
                  printf("%d",indice);
                }else{
                    printf("\nCod: %d",indice);
                    printf("\n\tNome %s Faltas:%d\n",al.nome,al.faltas);
                    printf("Faltas a adicionar (valores negativos removem faltas): ");
                    scanf("%d",&novaFalta);
                    al.faltas+=novaFalta;

                    fwrite( &al , sizeof(ALUNO) , 1 , arq3 );


                }
                indice++;



         }
    }
         fclose(arq);
         fclose(arq3);

         remove("alunos.txt");
         rename("copiaaluno.txt","alunos.txt");



}
MudaNota(){

          FILE *arq,*arq3;
          ALUNO al;
          int cod,indice,mudaNota;
          float novaNota;

          arq = fopen("alunos.txt","rb");
          arq3 = fopen("copiaaluno.txt","wb");

          if(arq == NULL||arq3==NULL){

               printf("Arquivo fonte não encontrado!\n");

         }else{
                Listar(0);
                printf("\nDigite o codigo do aluno que deseja alterar a nota: ");
                scanf("%d",&cod);
                indice= 1;
                while(fread ( &al , sizeof(ALUNO) , 1 ,arq ) == 1 ){

                if(cod!=indice){

                  fwrite( &al , sizeof(ALUNO) , 1 , arq3 );
                  printf("%d",indice);
                }else{
                    printf("\nCod: %d",indice);
                    printf("\n\tNome %s nota 1:%.1f nota 2:%.1f nota 3:%.1f\n",al.nome,al.nota[0],al.nota[1],al.nota[2]);
                    printf("Digite o numero da nota que deseja mudar: ");
                    scanf("%d",&mudaNota);
                    printf("\nDigite a nova nota: ");
                    scanf("%f",&novaNota);

                    mudaNota--;
                    al.nota[mudaNota]=novaNota;

                    fwrite( &al , sizeof(ALUNO) , 1 , arq3 );


                }
                indice++;



         }
    }
         fclose(arq);
         fclose(arq3);

         remove("alunos.txt");
         rename("copiaaluno.txt","alunos.txt");



}

AlteraCadastro(){

    int opcao;

     do{
         Cabecalho();
         printf("\n1 - Remover Aluno");
         printf("\n2 - Alterar Faltas");
         printf("\n3 - Alterar nota");
         printf("\n0 - Sair");
         printf("\nOpcao: ");
         scanf("%d", &opcao);
         switch(opcao){
           case 1:
                  RemoveAluno();
            break;

           case 2:

                  MudaFalta();

            break;

           case 3:
                  MudaNota();
            break;


           case 0:


            break;
           default:

                printf("Opcao invalida");
                getch();

           break;

         }




        }while(opcao != 0);




}




