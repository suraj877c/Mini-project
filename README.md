#include<stdio.h>
#include<conio.h>
#include<stdlib.h>
void menu();
 void create();
 void open();
 void rewrite();
 void copy();
 void edit();

    int main (){
        menu();
        return 0;
    }

    void menu(){
    int choice;
    while(choice){
    printf("\n---- Notepad Mini ----\n");
    printf("\n1. new file ");
    printf("\n2. open file ");
    printf("\n3. rewrite file (write/append)");
    printf("\n4. copy file (/content/)");
    printf("\n5. edit file(/content/)");
    printf("\n0. exit ");
    printf("\nnow enter your choice ");
        scanf("%d",&choice);
        getchar(); //remove newline;
    switch (choice){
        case 1: {
        system("cls");
         create();
        getch();
        break;
        }
        case 2:{
        system("cls");
         open();
        getch();
        break;
        }
        case 3:{
            system("cls");
            rewrite();
            getch();
            break;
        }
        case 4:{
            system("cls");
            copy();
            getch();
            break;
        }
        case 5:{
            system("cls");
            edit();
            getch();
            break;
        }
        case 0:{
            system("cls");
          printf("\nexit");
          printf("\nT H A N K Y O U");
           getch();
            break;
        }
    default:{
        printf("\nOppS....you enterd wrong choice!! ");
        break;
    }
    }
}
  }

void create(){
    char filename[100], ch;
    FILE *fp = NULL;

    printf("enter new file name: ");
    scanf("%s", filename);
    getchar(); //remove newline;
    fp = fopen(filename, "w"); // write mode
    if(fp == NULL) {
        printf("error file opening.\n");
        return;
    }
    printf("\nwrite something \n");
     printf("if you do stop Ctrl+D ya Ctrl+Z press this keys:\n");

    while((ch=getchar())!=EOF) {
        fputc(ch,fp); 
    }  

    fclose(fp);
    printf("\nsuccessfully created !!.\n");
}

void open(){
    char filename[100],ch;
    FILE *fp = NULL;

    printf("enter file name you want to open: ");
    scanf("%s",filename);
    fp=fopen(filename ,"r"); //read mode;
    if(fp==NULL){
        printf("cound't find !! \n");
        return;
    }
    printf("\nfile content is\n\n");
    while((ch=fgetc(fp))!=EOF){
    putchar(ch);
    }
    fclose(fp);
}

void rewrite(){
    char filename[100],ch;
    FILE *fp = NULL;
    
    printf("enter file name you want to edit: ");
    scanf("%s",filename);
    getchar(); //remove newline;
    fp=fopen(filename,"a");
    if(fp==NULL){
        printf("your file is missing !!\n");
        return;
    }

     printf("\nwrite something \n");
     printf("if you do stop Ctrl+D ya Ctrl+Z press this keys:\n");

    while((ch=getchar())!=EOF) {
        fputc(ch,fp); 
    }  
 fclose(fp);
    printf("\nsuccessfully updated !!.\n");
}

void copy(){
    char filename[100],ch;
    FILE *fp1=NULL,*fp2=NULL;

    printf("enter your file you want to copy: ");
    scanf("%s",filename);
    getchar();
    fp1=fopen(filename,"r");
    if(fp1==NULL){
        printf("coundn't find\n");
        return;
    }
    fp2=fopen("temp.txt","w");//this file copy content of filename;
    if(fp2==NULL){
        printf("no file avilable\n");
        return;
    }
    while((ch=fgetc(fp1))!=EOF){
        fputc(ch,fp2);
    }
    fclose(fp1);
    fclose(fp2);
    printf("sucessfully copied\n");
}

void edit(){
    char filename[100];
    char tempFilename[] = "temp.txt";
    FILE *org, *temp;
    char line[1000], newLine[1000];
    int lineNumber = 1, targetLine;

    printf("enter file name you want to edit: ");
    scanf("%s", filename);

    printf("write a line want you replace ");
    scanf("%d", &targetLine);
    getchar();

    printf("write somthing ");
    fgets(newLine, sizeof(newLine), stdin);

    org = fopen(filename, "r");
    temp = fopen(tempFilename, "w");

    if (!org || !temp) {
        printf("error file opening.\n");
        return;
    }

    //  Line-by-line copy/replace
    while (fgets(line, sizeof(line), org)) {
        if (lineNumber == targetLine) {
            fputs(newLine, temp); // replace line
        } else {
            fputs(line, temp);    // copy original line
        }
        lineNumber++;
    }

    fclose(org);
    fclose(temp);

    // Replace original file
    remove(filename);
    rename(tempFilename, filename);

    printf("File successfully edited!\n");
  
}
