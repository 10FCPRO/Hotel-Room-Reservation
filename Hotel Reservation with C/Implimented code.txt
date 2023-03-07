#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>
#include <unistd.h>

int CHOICEB;
int CHOICEV;
int done = 0;
int taken_roomyV[10] = {};
int taken_roomyB[10] = {};
int taken_flagB;
int taken_flagV;
int rw; //room wanted
int atV = 0; //already taken Vacation trip
int atB = 0; //already taken Business trip
int taken_roomV[10] = {0,0,0,0,0,0,0,0,0,0};
int taken_roomB[10] = {0,0,0,0,0,0,0,0,0,0};
int nc = 0; //names count
int Menu();
int Reservation();
void Rooms_VT();
void Rooms_BT();
void receiptss();
void packageB();
void packageV();

struct customers
{
    char names[6];
    float receiptB;
    float receiptV;
    int CHOICEB;
    int CHOICEV;
    int Brooms;
    int Vrooms;
    float total;
};

struct customers cust[20];

int main()
{
    for(;;)
    {
        done = 0;
        for(int y = 1;y != 0;)
        {
            int trip = 0;
            char sure = 'N';
            int cho = Menu();
            if (cho == 1){
                printf("Hello, please enter your name below:\n");
                scanf("%s",&cust[nc].names);
                system("cls");
                printf("Welcome Mr(s). %s,to Kimchi Hotel\n",cust[nc].names);
                trip = Reservation();
                do{
                    if (trip == 1)
                    {
                        printf("Great choice!\n");
                        Rooms_VT();
                    }else if (trip == 2)
                    {
                        printf("Great choice!\n");
                        Rooms_BT();
                    }
                    system("cls");
                    printf("Do you want anything else? (1/0):\n");
                    scanf("%d",&y);
                    while(y != 0 && y != 1)
                    {
                        printf("Wrong choice please enter it again:\n");
                        scanf("%d",&y);
                    }
                    if (y == 1)
                    trip = Reservation();
                }while (y == 1);
                if(done == 1)
                {
                    if(cust[nc].Vrooms > 0)
                    {
                        packageV();
                        cust[nc].receiptV = 50 * cust[nc].Vrooms;
                        cust[nc].receiptV += cust[nc].CHOICEV;
                        if(cust[nc].CHOICEV == 30)
                            printf("You have selected the breakfast only package\n");
                        else if(cust[nc].CHOICEV == 50)
                            printf("You have selected the half board package\n");
                        else if(cust[nc].CHOICEV == 70)
                            printf("You have selected the all inclusive package\n");
                        printf("Your payment is $%0.2f per night for %d VACATION rooms\n\n",cust[nc].receiptV,cust[nc].Vrooms);
                        sleep(2);
                    }
                    if(cust[nc].Brooms > 0)
                    {
                        packageB();
                        cust[nc].receiptB = 100 * cust[nc].Brooms;
                        cust[nc].receiptB += cust[nc].CHOICEB;
                        if(cust[nc].CHOICEB == 30)
                            printf("You have selected the breakfast only package\n");
                        else if(cust[nc].CHOICEB == 50)
                            printf("You have selected the half board package\n");
                        else if(cust[nc].CHOICEB == 70)
                            printf("You have selected the all inclusive package\n");
                        printf("Your payment was $%0.2f per night for %d BUSINESS rooms before the 20%% discount\n",cust[nc].receiptB,cust[nc].Brooms);
                        sleep(1);
                        cust[nc].receiptB *= 0.8;
                        printf("Your payment is now $%0.2f per night for %d BUSINESS rooms after the 20%% discount\n\n",cust[nc].receiptB,cust[nc].Brooms);
                        sleep(1);
                    }
                    cust[nc].total = cust[nc].receiptB + cust[nc].receiptV;
                    printf("Total is = $%0.2f\n",cust[nc].total);
                    sleep(3);
                    system("cls");
                    nc++;
                }
            }
            else if (cho == 2)
            {
                if(nc == 0)
                {
                    printf("No receipts yet.\n");
                    sleep(1);
                    system("cls");
                }
                receiptss();
            }
            else if (cho == 0)
            {
                printf("Are you sure you want to exit? (Y/N)\n");
                scanf(" %c",&sure);
                while (toupper(sure) != 'Y' && toupper(sure) != 'N')
                {
                    printf("Wrong input. Please read carefully and insert your choice again.\n");
                    scanf(" %c",&sure);
                }
                if(toupper(sure) == 'Y')
                    return 0;
                system("cls");
            }

    }
}
    return 0;
}

int Menu()
{
    int choice;
    printf("For reservation, press 1\n");
    printf("For receipts, press 2\n");
    printf("If you want to exit, press 0\n");
    scanf(" %d",&choice);
    while (choice != 1 && choice != 2 && choice != 0)
    {
        printf("Wrong input. Please read carefully and insert your choice again.\n");
        scanf(" %d",&choice);
    }
    system("cls");
    return choice;
}
int Reservation()
{
    int trip;
    printf("What are you looking for?\n");
    printf("For a vacation trip, press 1\n");
    printf("For a business trip (comes with a 20%% discount), press 2\n");
    printf("If you want to exit, press 0\n");
    scanf("%d",&trip);
    while (trip != 1 && trip != 2 && trip != 0)
    {
        printf("Wrong input. Please read carefully and insert your choice again.\n");
        scanf(" %d",&trip);
    }
    return trip;
}
void Rooms_VT()
{
    int rc; //rooms count
    if (atV == 10)
    {
        printf("But sadly, no rooms are available Mr(s).%s, please come back later.\n",cust[nc].names);
        sleep(4);
    }
    else
    {
        printf("How many rooms do you wish to book, Mr.%s?\n",cust[nc].names);
        scanf("%d",&rc);
        while (rc < 1 || rc > 10 || 10 - atV < rc)
        {
            if (rc < 1 || rc > 10)
                printf("Wrong input. Please read carefully and insert your choice again, else press 0 if you want to leave.\n");
            if (10 - atV < rc)
                printf("Sorry Mr(s). %s, there are only %d room(s) left.\nIf you want another number of room(s) please enter it, else press 0 if you want to leave.\n",cust[nc].names,10 - atV);
            scanf("%d",&rc);
            if(rc == 0)
                return;
        }
        printf("These are the available room(s):\n");
        for(int k = 0; k < 10; k++)
            if(taken_roomV[k] == 0)
                printf("%d ",k+1);
        printf("Please select your room(s):\n");
        for(int k = 0; k < rc; )
        {
            scanf("%d",&rw);
            while (rw < 1 || rw > 10)
            {
                printf("Wrong room number, please enter a correct number.\n");
                scanf("%d",&rw);
            }
            taken_flagV = 0;
            for(int j = 0; j < 10; j++)
            {
                if(rw == taken_roomyV[j])
                {
                    printf("Room already taken\n");
                    taken_flagV = 1;
                    break;
                }
            }
            if (taken_flagV == 0)
            {
                printf("Room reserved\n");
                done = 1;
                taken_roomyV[atV] = rw;
                atV++;
                taken_roomV[rw - 1] = 1;
                k++;
            }

        }
    }
    cust[nc].Vrooms += rc;
}

void Rooms_BT()
{
    int rc; //rooms count
    if (atB == 10)
    {
        printf("But sadly, no rooms are available Mr(s).%s, please come back later.\n",cust[nc].names);
        sleep(4);
    }
    else
    {
        printf("How many rooms do you wish to book, Mr(s).%s?\n",cust[nc].names);
        scanf("%d",&rc);
        while (rc < 1 || rc > 10 || 10 - atB < rc)
        {
            if (rc < 1 || rc > 10)
                printf("Wrong input. Please read carefully and insert your choice again, else press 0 if you want to leave.\n.\n");
            if (10 - atB < rc)
                printf("Sorry Mr(s). %s, there are only %d room(s) left.\nIf you want another number of room(s) please enter it, else press 0 if you want to leave.\n",cust[nc].names,10 - atB);
            scanf("%d",&rc);
            if(rc == 0)
                return;
        }
        printf("These are the available room(s):\n");
        for(int k = 0; k < 10; k++)
            if(taken_roomB[k] == 0)
                printf("%d ",k+1);
        printf("Please select your room(s):\n");
        for(int k = 0; k < rc; )
        {
            scanf("%d",&rw);
            while (rw < 1 || rw > 10)
            {
                printf("Wrong room number, please enter a correct number.\n");
                scanf("%d",&rw);
            }
            taken_flagB = 0;
            for(int j = 0; j < 10; j++)
            {
                if(rw == taken_roomyB[j])
                {
                    printf("Room already taken\n");
                    taken_flagB = 1;
                    break;
                }
            }
            if (taken_flagB == 0)
            {
                printf("Room reserved\n");
                done = 1;
                taken_roomyB[atB] = rw;
                atB++;
                taken_roomB[rw - 1] = 1;
                k++;
            }

        }
    }
    cust[nc].Brooms += rc;
}

void packageV ()
{
    int choicePack;
    printf("There are 3 available packages for you VACATION trip room(s):\n");
    printf("1. For breakfast only with cost of $30 per night, press 1:\n\n");
    printf("2. For half board with cost of $50 per night, press 2:\n\n");
    printf("3. For all inclusive with cost of $70 per night, press 3:\n\n");
    printf("Kindly, select your package Mr(s).%s:\n",cust[nc].names);
    scanf("%d",&choicePack);
    while(choicePack != 1 && choicePack != 2 && choicePack != 3)
    {
        printf("Inappropriate input, please enter another correct number:\n");
        scanf("%d",&choicePack);
    }
    switch(choicePack)
    {
        case 1: cust[nc].CHOICEV = 30;break;
        case 2: cust[nc].CHOICEV = 50;break;
        case 3: cust[nc].CHOICEV = 70;break;
    }
}

void packageB ()
{
    int choicePack;
    printf("There are 3 available packages for your BUSINESS trip room(s):\n\n");
    printf("1. For breakfast only with cost of $30 per night, press 1:\n\n");
    printf("2. For half board with cost of $50 per night, press 2:\n\n");
    printf("3. For all inclusive with cost of $70 per night, press 3:\n\n");
    printf("Kindly, select your package Mr(s).%s:\n",cust[nc].names);
    scanf("%d",&choicePack);
    while(choicePack != 1 && choicePack != 2 && choicePack != 3)
    {
        printf("Inappropriate input, please enter another correct number:\n");
        scanf("%d",&choicePack);
    }
    switch(choicePack)
    {
        case 1: cust[nc].CHOICEB = 30;break;
        case 2: cust[nc].CHOICEB = 50;break;
        case 3: cust[nc].CHOICEB = 70;break;
    }
}

void receiptss(){
for(int j = 0; j < nc;j++)
    {
        printf("Customer %d name: %s\n",j + 1,cust[j].names);
        if (cust[j].Vrooms > 0)
        {
            printf("He/She took %d VACATION rooms\n",cust[j].Vrooms);
            printf("Their price was $%0.2f\n",cust[j].receiptV);
            if(cust[j].CHOICEV == 30)
                printf("With selected package: Breakfast only\n");
            else if(cust[j].CHOICEV == 50)
                printf("With selected package: Half board\n");
            else if(cust[j].CHOICEV == 70)
                printf("With selected package: All inclusive\n");
        }
        if (cust[j].Brooms > 0)
        {
            printf("He/She took %d BUSINESS rooms\n",cust[j].Brooms);
            printf("Their price was $%0.2f\n",cust[j].receiptB);
            if(cust[j].CHOICEB == 30)
                printf("With selected package: Breakfast only\n");
            else if(cust[j].CHOICEB == 50)
                printf("With selected package: Half board\n");
            else if(cust[j].CHOICEB == 70)
                printf("With selected package: All inclusive\n");
        }
        printf("Total price is $%0.2f\n\n",cust[j].total);
    }
}
