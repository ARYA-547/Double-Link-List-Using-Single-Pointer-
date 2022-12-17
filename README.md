# Double-Link-List-Using-Single-Pointer-
All the the operations of a double link list is performed using single pointer,by using the concept of XOR link list
//Implementing double link list using a single pointer.
#include<stdio.h>
#include<stdlib.h>
#include<inttypes.h>
typedef struct node
{
	int data;
	struct node* npx;
}node;
node* head=NULL;
node* tail=NULL;
void create();//function to add new node at the end.
void print_forward();
void print_backward();
void delete_end();
void delete_begin();
int main()
{
	int ch,c=0;//counter to show the instruction only once during execution
	while(1)
	{
		if(c==0)
		{
			for(int i=0;i<=40;i++)
		      printf("*");
		    printf("\n");
		    printf("Press 1 to insert.\n");
		    printf("Press 2 to travel forward.\n");
	     	printf("Press 3 to travel backward.\n");
		    printf("Press 4 to Delete at end.\n");
	    	printf("Press 5 to Delete at beginning.\n");
		    printf("press 6 to exit.\n");
		    for(int i=0;i<=40;i++)
		       printf("*");
	    	printf("\n");
		    for(int i=0;i<=40;i++)
		      printf("-");
		    printf("\n");
		    printf("Enter your choice:");
		    scanf("%d",&ch);
		    printf("\n");
		    for(int i=0;i<=40;i++)
		      printf("-");
		    printf("\n");
		}
		else
		{
			printf("\n");
		    for(int i=0;i<=40;i++)
		       printf("-");
		    printf("\n");
		    printf("Enter your choice:");
		    scanf("%d",&ch);
		    printf("\n");
		    for(int i=0;i<=40;i++)
		       printf("-");
		    printf("\n\n");
		}
		c=c+1;
		switch(ch)
		{
			case 1:
				create();
				break;
			case 2:
				print_forward();
				break;
			case 3:
				print_backward();
				break;
			case 4:
				delete_end();
				break;
			case 5:
				delete_begin();
				break;
			case 6:
				printf("Exited sucessfully\n");
				exit(0);
				break;
			default:
				printf("Wrong choice");
				break;		
		}
	}
	return 0;
}
node *XOR(node* a,node* b)
{
	return (node*)((uintptr_t)(a)^(uintptr_t)(b));
}
void create()
{
	node *temp;
	int val;
	temp=(node*)malloc(sizeof(node));
	printf("Enter the data: ");
	scanf("%d",&val);
	temp->data=val;
	temp->npx=XOR(tail,NULL); //tail will be the previous node for the new node
	if(head==NULL)
	{
		head=temp;
	}
	if(tail!=NULL)
	{
		node* next=XOR(tail->npx,NULL);//required for updating the pointer field of tail node,store the adress of previous value of tail.
		tail->npx=XOR(temp,next);//temp is next value and next is previous value
	}
	tail=temp;
	printf("Insertion sucessfull\n");
}
void print_forward()
{
	node *curr=head;
	node *prev=NULL;
	node *next;
	printf("Following are the nodes of linklist:\n");
	while(curr!=NULL)
	{
		printf("%d   ",curr->data);
		next=XOR(prev,curr->npx);
		prev=curr;
		curr=next;
    }
    printf("\n");
}
void print_backward()
{
	node *curr=tail;
	node *next=NULL;
	node *prev;
	printf("Following are the nodes of linklist:\n");
	while(curr!=NULL)
	{
		printf("%d   ",curr->data);
		prev=XOR(next,curr->npx);
		next=curr;
		curr=prev;
   }
   printf("\n");
}
void delete_end()
{
	if(head==NULL)
	{
		printf("Empty list\n");
	}
	else
	{
		node *temp,*prev;
	    temp=tail;
	    prev=XOR(temp->npx,NULL);
	    prev->npx=XOR(XOR(prev->npx,temp),NULL);//the inner xor will bring the address of the previous of prev node
	    printf("The deleted item is %d\n",temp->data);
	    free(temp);
	    tail=prev;	
	}
}
void delete_begin()
{
	if(head==NULL)
	{
		printf("Empty List\n");
	}
	else
	{
		node *temp,*next;
	    temp=head;
     	next=XOR(NULL,temp->npx);
    	next->npx=XOR(NULL,XOR(temp,next->npx));//inner xor will store the address of next of next node.
	    head=next;
	    printf("The deleted item is %d\n",temp->data);
	    free(temp);
	}
	
}
