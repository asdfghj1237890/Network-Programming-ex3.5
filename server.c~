#include <stdio.h>
#include <netinet/in.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <pthread.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>
#include <sys/wait.h>

void str_echo(int s){
	char buf[20] = {0};
	int n;
	
	while (n = recv(s,buf,20,0)) {
		puts("Message from client...");
		fputs(buf,stdout);
		send(s,buf,strlen(buf),0);
		memset(buf, 0, sizeof(buf));
	}
	if(n == 0){
		puts("Client disconnetd...");
		fflush(stdout);
	}
}

static void *
	doit(void *arg)
{
	int connfd;
	connfd = *((int *)arg);
	free(arg); //free memory

	pthread_detach(pthread_self());
	str_echo(connfd);
	close(connfd);
	return NULL;
}

int main(){
	int ls,cs,len,*iptr;

	struct sockaddr_in serv,cli;
	pid_t pid;
	int stat;
	pthread_t th;
	
	puts("I am server...");
	
	//creating socket
	ls = socket(AF_INET,SOCK_STREAM,0);
	puts("Socket created succressfully...");
	
	//socket address structure
	serv.sin_family = AF_INET;
	serv.sin_addr.s_addr = INADDR_ANY;
	serv.sin_port = htons(2016);

	bind(ls,(struct sockaddr*)&serv,sizeof(serv));
	puts("Binding Done...");
	
	listen(ls,3);
	puts("listening for client...");
	
	for( ; ; ){
		len = sizeof(cli);
		iptr = malloc(sizeof(int));
		*iptr = accept(ls,(struct sockaddr*)&cli,&len);
		puts("Connected to Client...");
		//creating thread
		pthread_create(&th,NULL,&doit,iptr);
	}
	return 0;


}

