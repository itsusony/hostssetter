#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char* argv[]){
    if(argc != 3 || !argv[1] || !argv[2] || strlen(argv[1])==0 || strlen(argv[2])==0){
        fprintf(stderr,"usage:\n\nAdd domain into hosts:\t./hostssetter www.google.co.jp 127.0.0.1\n"
            "Del domain in hosts:\t./hostssetter www.google.co.jp DELETE\n\n");
        return 1;
    }   
    char* domain = strdup(argv[1]);
    char* ip = strdup(argv[2]);
    int domain_len = strlen(domain);
    int is_del = strstr(ip,"DELETE")!=NULL ? 1 : 0;

    FILE *fp = fopen("/etc/hosts","r");
    if(fp!=NULL){
        char *newhosts = calloc(1024*1024,sizeof(char));
        char line[1024];
        int exist = 0;
        while(fgets(line,1024,fp)!=NULL){
            int line_len = strlen(line);
            if(line_len == 1 && strchr(line,'\n')!=NULL)continue;
            char* target = line_len>domain_len+1 ? strstr(line,domain) : NULL;
            if(target!=NULL && (target[domain_len]=='\n' || !target[domain_len])){
                exist = 1;
                if(!is_del){
                    char newline[1024]="";
                    sprintf(newline,"%s %s\n",ip,domain);
                    strcat(newhosts,newline);
                }   
            }else{
                strcat(newhosts,line);
            }   
        }   
        fclose(fp);
        if(newhosts && strlen(newhosts)>0){
            FILE *fp2 = fopen("/etc/hosts","w");
            if(exist || is_del){
                fprintf(fp2,"%s",newhosts);
                printf(exist ? (is_del?"deleted\n":"updated\n") : "none\n");
            }else{
                fprintf(fp2,"%s\n%s %s",newhosts,ip,domain);
                printf("added");
            }   
            fclose(fp2);
        }else{
            fprintf(stderr,"hosts write error\n");
        }   
    }else{
        fprintf(stderr,"file open error\n");
    }   
    return 0;
}
