
/*_________________________Osama Zeidan Project | Big Integers Calculator______________________________*/

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <ctype.h>

typedef char *String; // string data type

/*_________________________Structs and Types______________________________*/

typedef struct Node
{
    bool isNegative;
    int Data;
    struct Node *Next;
    struct Node *Previous;
} Node; // Doubly List Node...

typedef Node *DoublyList;
typedef Node *DoublyNode;
typedef Node *PtrToDoublyNode;

typedef struct node
{
    DoublyList Data;
    struct node *Next;
} node; //  Queue Node...

typedef node *QueueNode;
typedef node *Queue;
typedef node *PtrToQueueNode;
/*__________________________Functions ProtoTypes______________________________*/

int IsEmptyQueue(Queue queue);
Queue CreateQueue();
void MakeEmptyQueue(Queue queue);
void Dequeue(Queue queue);
DoublyList Front(Queue queue);
void Enqueue(DoublyList x, Queue queue);
void DisposeQueue(Queue queue);
int QueueSize(Queue queue);
void CreatDoublyList(DoublyList *list);
int IsEmpty(DoublyList list);
int IsLast(DoublyNode node);
DoublyNode lastDoublyNode(DoublyList list);
DoublyNode FindNode(int x, DoublyList list);
void DeleteNode(int x, DoublyList list);
void Insert(int x, DoublyList list, DoublyNode prevNode);
void PrintList(DoublyList list);
void MakeEmpty(DoublyList *list);
int Size(DoublyList list);
void DisposeList(DoublyList *list);
DoublyList CopyList(DoublyList list, DoublyList copiedList);
int CompareLists(DoublyList list1, DoublyList list2);
void RemoveLeftZeros(DoublyList *list);
DoublyList Sub(DoublyList list1, DoublyList list2);
DoublyList Add(DoublyList list1, DoublyList list2);
DoublyList Mul(DoublyList list1, DoublyList list2);
DoublyList Div(DoublyList list1, DoublyList list2, DoublyList *remainder);
DoublyList LongDiv(DoublyList list1, DoublyList list2, DoublyList *mod);

/*_________________________Main Function______________________________*/

int main(void)
{

    String fileName = (String)(malloc(sizeof(char) * 20));
    printf("Welcome To Osama's BIG-INTEGERS Calculator.....\n_______________________________________________________________________\n\n");
    printf("Please Enter The Name Of The Integers File: ");

    Queue integers = CreateQueue(integers); // Queue for All Integers in File...
    DoublyList number = NULL;
    CreatDoublyList(&number);
    PtrToDoublyNode lastNode = number;

    FILE *outFile = NULL; // Output File...
    scanf("%s", fileName);
    FILE *file = fopen(fileName, "r");
    while (file == NULL)
    {
        printf("The File Does Not Exisit!\n");
        printf("Please Enter The Name Of The Integers File: ");
        scanf("%s", fileName);
        file = fopen(fileName, "r");
    } // end while()

    int counter = 0;
    char c = ' ';

    while ((fscanf(file, "%c", &c)) != EOF) // Reading From File...
    {

        if (counter == 0 && c == '-')
        {
            number->isNegative = true;
            counter++;
            continue;
        } // end if()

        else if (c > 47 && c < 58)
        {

            Insert(c - '0', number, lastNode);
            lastNode = lastNode->Next;
            counter = 1;
        } // end else if()

        else
        {
            counter = 0;
            if (!IsEmpty(number))
            {
                Enqueue(number, integers);
            } // end if()

            CreatDoublyList(&number);
            MakeEmpty(&number);
            lastNode = number;
        } // end else
    }     // end while()

    int line = 0;
    PrintQueue(integers);

    int choose[2];
    int operation = 0;
    DoublyList integer1 = NULL;
    DoublyList integer2 = NULL;
    Queue results = CreateQueue(&results);
    PtrToQueueNode queuePtr = integers->Next;
    DoublyList tmpList = NULL;
    PtrToDoublyNode tmpPtr = NULL;
    while (true) // Choosing An Operation and Numbers...
    {

        printf("\nPlease, Choose 2 Numbers to Make Operations On It: \n\n");
        printf("1st Integer: ");
        scanf("%d", &choose[0]);
        printf("2nd Integer: ");
        scanf("%d", &choose[1]);

        queuePtr = integers->Next;

        if (choose[1] < choose[0])
        {
            int tmp = choose[1];
            choose[1] = choose[0];
            choose[0] = tmp;
        } // end if()

        while (choose[0] < 1 || choose[1] > Size(integers)) // Checking if the input is valid...
        {
            printf("\nPlease, Choose 2 Numbers to Make Operations On It: \n\n");
            printf("1st Integer: ");
            scanf("%d", &choose[0]);
            printf("2nd Integer: ");
            scanf("%d", &choose[1]);
        } // end while()

        for (int i = 0; i < choose[1]; i++)
        {
            if (i == choose[0] - 1)
            {
                integer1 = CopyList(integer1, queuePtr->Data);
            } // end if()
            if (i == choose[1] - 1)
            {
                integer2 = CopyList(integer2, queuePtr->Data);
            } // end else if()
            queuePtr = queuePtr->Next;
        } // end for()

        printf("Choose an Operation:\n\n"); // Main Menue For Program...
        printf("1. Add the Two Integers.\n");
        printf("2. Subtract the Two Integers.\n");
        printf("3. Multiply the Two Integers.\n");
        printf("4. Divide the Two Integers.\n");
        printf("5. Print All Previous Results On an Output File.\n");
        printf("6. Exit.\n\n");
        scanf("%d", &operation);
        switch (operation)
        {
        case 1: // Add Case

            printf("The Sum of Choosed Integers is: ");
            DoublyList tmp1 = NULL;
            if (integer1->isNegative ^ integer2->isNegative)
            {
                tmp1 = Sub(integer1, integer2);
            } // end if()
            else
            {
                tmp1 = Add(integer1, integer2);
            } // end else
            Enqueue(tmp1, results);
            PrintList(tmp1);
            break;
        case 2: // Sub case

            printf("The Subtraction of choosed Integers is: ");
            DoublyList tmp2 = NULL;
            if (integer1->isNegative ^ integer2->isNegative)
            {
                tmp2 = Add(integer1, integer2);
            } // end if()
            else
            {
                tmp2 = Sub(integer1, integer2);
            } // end else
            Enqueue(tmp2, results);
            PrintList(tmp2);
            break;
        case 3: // Mul case

            printf("The Multiplication of the Choosed Integers is: ");
            DoublyList tmp3 = Mul(integer1, integer2);
            if (integer1->isNegative ^ integer2->isNegative)
            {
                tmp3->isNegative = true;
            } // end if()
            PrintList(tmp3);
            Enqueue(tmp3, results);
            break;
        case 4: // Div case

            printf("The Division for the Choosed Integers is: ");
            DoublyList tmp5 = NULL;
            if (CompareLists(integer1, integer2) == -1)
            {
                DoublyList temp = integer1;
                integer1 = integer2;
                integer2 = temp;
            } // end if()
            DoublyList tmp4 = LongDiv(integer1, integer2, &tmp5);
            if (integer1->isNegative ^ integer2->isNegative)
            {
                tmp4->isNegative = true;
            } // end if()
            PrintList(tmp4);
            printf("Remainder: ");
            PrintList(tmp5);
            break;
        case 5: // Print on a file case

            printf("Enter the Output File Name: ");
            scanf("%s", fileName);
            outFile = fopen(fileName, "a");
            int queueSize = QueueSize(results);
            for (int i = 0; i < queueSize; i++)
            {
                tmpList = Front(results);
                tmpPtr = tmpList->Next;
                int listSize = Size(tmpList);
                fprintf(outFile, "\nResult(%d): ", (i + 1));
                if (tmpList->isNegative)
                {
                    fprintf(outFile, "-");
                } // end if()
                for (int j = 0; j < listSize; j++)
                {
                    fprintf(outFile, "%d", tmpPtr->Data);
                    tmpPtr = tmpPtr->Next;
                } // end for()
                Dequeue(results);
            } // end for()
            fclose(outFile);
            break;
        case 6: // Exit the Program Case

            printf("Good Bye...!");
            MakeEmptyQueue(integers);
            free(fileName);
            exit(0);
            break;
        default:
            printf("Invalid Choose!, Try Again!...");
        } // end switch()

    } // end BIG while()

    MakeEmptyQueue(integers);
    free(fileName);

} // end main()

/*_________________________Add Function______________________________*/

/*
This Function To Add Two Integers Represented as a Linked Lists
The Time Complexity Of this Function is O(n)
*/
DoublyList Add(DoublyList list1, DoublyList list2)
{

    DoublyList result = NULL; // list to save the result
    long list1Size = Size(list1);
    long list2Size = Size(list2);

    int sum = 0;
    int carry = 0;

    if (list1Size < list2Size)
    {
        DoublyList tmp = list1;
        list1 = list2;
        list2 = tmp;
        long tmp1 = list1Size;
        list1Size = list2Size;
        list2Size = tmp1;

    } // end if()
    result = CopyList(result, list1);
    PtrToDoublyNode lastResultNode = lastDoublyNode(result);
    PtrToDoublyNode lastList2Node = lastDoublyNode(list2);

    while (lastList2Node != list2) // loop to add the data in lists nodes
    {
        sum = lastList2Node->Data + lastResultNode->Data + carry;
        carry = sum / 10;
        sum = sum % 10;
        lastResultNode->Data = sum;
        lastResultNode = lastResultNode->Previous;
        lastList2Node = lastList2Node->Previous;
    }                  // end while()
    while (carry == 1) // checking for carry after the smallest number end
    {
        if (lastResultNode == result)
        {
            Insert(carry, result, result);
            carry = 0;
            break;
        } // end if()
        sum = lastResultNode->Data + carry;
        carry = sum / 10;
        sum = sum % 10;
        lastResultNode->Data = sum;
        lastResultNode = lastResultNode->Previous;
    } // end while()
    return result;

} // end Add()

/*_________________________Sub Function______________________________*/

/*
This Function To Subtract Two Integers Represented as a Linked Lists
The Time Complexity Of this Function is O(n), this function is similar to Add function in Time Complexity
This Function Subtract the Smaller integer from the Larger integer, and returns the result...
*/
DoublyList Sub(DoublyList list1, DoublyList list2)
{

    int compare = CompareLists(list1, list2); // checking the different between the integers (lists)...
    int borrow = 0;
    DoublyList result = NULL;

    if (compare == -1)
    {
        DoublyList tmp = list1;
        list1 = list2;
        list2 = tmp;
    } // end if()

    PtrToDoublyNode list1Ptr = lastDoublyNode(list1); // Ptr to the least digit (the most right digit) fot integer 1...
    PtrToDoublyNode list2Ptr = lastDoublyNode(list2); // Ptr to the least digit (the most right digit) for integer 2...
    int sub = 0;

    while (list2Ptr->Previous != NULL) // subtract the data in nodes...
    {
        sub = (list1Ptr->Data - borrow) - list2Ptr->Data;
        if (sub < 0)
        {
            list1Ptr->Data = sub + 10;
            borrow = 1;
            list1Ptr = list1Ptr->Previous;
            list2Ptr = list2Ptr->Previous;
            continue;
        } // end if()
        list1Ptr->Data = sub;
        list1Ptr = list1Ptr->Previous;
        list2Ptr = list2Ptr->Previous;
        borrow = 0;
    } // end while()

    while (borrow == 1) // checking for borrow after the smallest integer ends
    {
        sub = list1Ptr->Data - borrow;
        if (sub < 0)
        {
            sub += 10;
            borrow = 1;
            list1Ptr->Data = sub;
            list1Ptr = list1Ptr->Previous;
            continue;
        } // end if()
        list1Ptr->Data = sub;
        list1Ptr = list1Ptr->Previous;

        borrow = 0;
    } // end while()

    return list1;
} // end Sub()

/*_________________________Mul Function______________________________*/

/*
This Function To Multiplicate Two Integers Represented as a Linked Lists
The Time Complexity Of this Function is O(n^2),
This Function multiply the each digit in the first integer in all digits in the second integer (Normal Multiplication)...
*/
DoublyList Mul(DoublyList list1, DoublyList list2)
{
    DoublyList result = NULL;
    CreatDoublyList(&result);
    Insert(0, result, result);
    DoublyList tmp = NULL;
    CreatDoublyList(&tmp);

    long list1Size = Size(list1);
    long list2Size = Size(list2);

    PtrToDoublyNode lastList1Node = lastDoublyNode(list1);
    PtrToDoublyNode lastList2Node = lastDoublyNode(list2);
    PtrToDoublyNode lastList2NodeTmp = lastList2Node;

    int mul = 0;
    int carry = 0;

    for (int i = 0; i < list1Size; i++)
    {
        for (int j = 0; j < list2Size; j++)
        {
            mul = (lastList1Node->Data * lastList2Node->Data) + carry;
            carry = mul / 10;
            mul = mul % 10;
            Insert(mul, tmp, tmp);
            if (j == list2Size - 1 && carry > 0)
            {
                Insert(carry, tmp, tmp);
                carry = 0;
                break;
            } // end if()
            lastList2Node = lastList2Node->Previous;
        } // end for()
        for (int k = 0; k < i; k++)
        {
            InsertLast(0, tmp);
        } // end for()

        result = Add(result, tmp);
        MakeEmpty(&tmp);

        lastList1Node = lastList1Node->Previous;
        lastList2Node = lastList2NodeTmp;
    } // end for()
      // RemoveLeftZeros(&result);
    return result;
} // end Mul()

/*_________________________LongDiv & Div Function__________________________________________*/

/*
This Function To Divide Two Integers Represented as a Linked Lists
The Time Complexity Of this Function is O(n), Because it depends on Sub only...
*/
DoublyList Div(DoublyList list1, DoublyList list2, DoublyList *remainder)
{
    if (IsEmpty(list1) || IsEmpty(list2))
    {
        return NULL;
    } // end if()

    DoublyList result = NULL;
    CreatDoublyList(&result);

    if (CompareLists(list1, list2) == -1)
    {
        // DoublyList temp = list1;
        // list1 = list2;
        // list2 = temp;
        Insert(0, result, result);
        *remainder = list2;
        return result;
    } // end if()

    DoublyList counter = NULL;
    CreatDoublyList(&counter);
    Insert(0, counter, counter);

    DoublyList one = NULL;
    CreatDoublyList(&one);
    Insert(1, one, one);

    int list1Size = Size(list1);
    int list2Size = Size(list2);

    result = Sub(list1, list2);

    int resultSize = Size(result);
    for (int i = 0; i < resultSize - list2Size; i++)
    {
        Insert(0, list2, list2);
    } // end for()

    while (CompareLists(result, list2) != -1)
    {

        counter = Add(counter, one);
        result = Sub(result, list2);
    } // end while()
    *remainder = result;
    return Add(counter, one);
} // end Div()

/*
This Function To Divide Two Integers Represented as a Linked Lists
The Time Complexity Of this Function is O(n), Because it depends on Sub and Add only
This Function divide the two integers by The (Long Division Method)...
*/
DoublyList LongDiv(DoublyList list1, DoublyList list2, DoublyList *mod)
{
    DoublyList tmp1 = NULL;
    CreatDoublyList(&tmp1);

    DoublyList remainder = NULL;
    CreatDoublyList(&remainder);

    DoublyList result = NULL;
    CreatDoublyList(&result);

    if (Size(list1) == Size(list2))
    {
        return Div(list1, list2, &remainder);
        *mod = remainder;
    } // end if()

    PtrToDoublyNode dividendPtr = list1->Next;
    InsertLast(dividendPtr->Data, tmp1);

    while (CompareLists(tmp1, list2) == -1)
    {
        dividendPtr = dividendPtr->Next;
        InsertLast(dividendPtr->Data, tmp1);

    } // end while()
    if (dividendPtr->Next == NULL)
    {
        return Div(list1, list2, &remainder);
        *mod = remainder;
    } // end if()

    while (dividendPtr->Next != NULL)
    {

        tmp1 = Div(tmp1, list2, &remainder);
        InsertLast(tmp1->Next->Data, result);
        dividendPtr = dividendPtr->Next;
        tmp1 = remainder;
        InsertLast(dividendPtr->Data, tmp1);

    } // end BIG while()

    tmp1 = Div(tmp1, list2, &remainder);
    InsertLast(tmp1->Next->Data, result);
    *mod = remainder;
    return result;
} // end LongDiv()

/*_________________________Doubly Linked List Library___________________________________________________________*/

/*_________________________CreateDoublyList Function______________________________*/

/*
This Function to Create a New Head Node (New Doubly List)...
*/
void CreatDoublyList(DoublyList *list)
{

    // if(*list != NULL)
    // {
    //     DisposeList(&(*list));
    // }
    (*list) = (DoublyList)malloc(sizeof(Node));

    if (*list == NULL)
    {
        printf("Out Of Memory!");
        exit(0);
    } // end if()
    (*list)->Next = NULL;
    (*list)->Previous = NULL;
    (*list)->isNegative = false;

} // end of MakeEmpty()

/*__________________________IsEmpty Function______________________________*/

int IsEmpty(DoublyList list)
{
    return list->Next == NULL;
} // end of IsEmpty()

/*_________________________IsLast Function_______________________________*/

int IsLast(DoublyNode node)
{
    return node->Next == NULL;
} // end of IsLast()

/*__________________________FindNode Function______________________________*/

/*
This Function To Search on Data in the Linked List
The Time Complexity of thid function is O(n)...
*/
DoublyNode FindNode(int x, DoublyList list)
{
    DoublyNode tmp = list->Next;
    while (tmp != NULL && tmp->Data != x)
    {
        tmp = tmp->Next;
    } // end of while()
    return tmp;
} // end of Find()

/*______________________DeleteNode Function__________________________________*/

/*
The Time Complexity of This Function is sane as FindNode Function...
*/
void DeleteNode(int x, DoublyList list)
{
    DoublyNode tmp1 = list->Next;
    while (tmp1 != NULL && tmp1->Data != x)
    {
        tmp1 = tmp1->Next;
    }
    if (tmp1 != NULL)
    {
        if (tmp1->Previous != NULL)
        {
            tmp1->Previous->Next = tmp1->Next;
        }
        if (tmp1->Next != NULL)
        {
            tmp1->Next->Previous = tmp1->Previous;
        }
        free(tmp1);
    }
} // end of Delete()

/*________________________Insert Function________________________________*/

/*
This Function Inserts a New Node After specific Node in The list...
->>>>> Time Complexioty: O(n)
*/
void Insert(int x, DoublyList list, DoublyNode prevNode)
{
    DoublyNode node = malloc(sizeof(Node));
    if (node == NULL)
    {
        printf("Memory Is Full...\n");
        return;
    }
    else
    {
        node->Data = x;
        node->Next = prevNode->Next;
        node->Previous = prevNode;
        prevNode->Next = node;
        if (node->Next != NULL)
        {
            node->Next->Previous = node;
        } // end if()
    }     // end else
}

/*______________________PrintListFunction__________________________________*/

/*
----> Time Complexity: O(n)...
*/
void PrintList(DoublyList list)
{
    DoublyNode tmp = list;
    if (list != NULL)
    {
        if (tmp->Next == NULL)
        {
            printf("Empty List!\n");
        }
        else
        {
            while (tmp->Next->Data == 0 && tmp->Next->Next != NULL)
            {
                tmp->Next = tmp->Next->Next;
            }
            if (list->isNegative == true)
            {
                printf("-");
            } // end if()
            while (tmp->Next != NULL)
            {
                printf("%d", tmp->Next->Data);
                tmp = tmp->Next;
            }
            printf("\n");
        }
    } // end BIG if()
    else
    {
        printf("Null");
    }
} // end of PrintList()

/*_______________________MakeEmpty Function_________________________________*/

/*
This Function Deletes all nodes in the list except the Head Node...
Time Complexity of this function is O(n)
*/
void MakeEmpty(DoublyList *list)
{
    if ((*list)->Next != NULL)
    {
        DoublyNode tmp1 = (*list)->Next;
        DoublyNode tmp2 = tmp1->Next;
        while (tmp2 != NULL)
        {
            free(tmp1);
            tmp1 = tmp2;
            tmp2 = tmp2->Next;
        } // end while()
        free(tmp1);
        (*list)->Next = NULL;
        (*list)->Previous = NULL;
    }

} // end MakeEmpty()

/*_______________________Size Function_________________________________*/

/*
This Function Calculate The Size of The List, The Time Complexity of it: O(n)
*/
int Size(DoublyList list)
{
    int size = 0;
    DoublyNode tmp = list;
    while (tmp->Next != NULL)
    {
        size++;
        tmp = tmp->Next;
    } // end while()
    return size;
} // end Size()

/*_____________________________LastDoublyNode Function_____________________________________________*/

/*
This Function returns the Last Node in the list...
The Time Complexity of It as the Previous Function...
*/
DoublyNode lastDoublyNode(DoublyList list)
{
    DoublyNode tmp = list;
    while (!IsLast(tmp))
    {
        tmp = tmp->Next;
    } // end while()
    return tmp;
} // end lastDoublyNode()

/*________________________________InsertLast Function__________________________________________*/

/*
This Function Inserts a New Node in The last of the List
----> Time Complexity of it: O(n)
*/
void InsertLast(int x, DoublyList list)
{
    DoublyNode tmp = lastDoublyNode(list);
    DoublyNode node = malloc(sizeof(Node));
    if (node == NULL)
    {
        printf("Memory Is Full...\n");
        return;
    }
    else
    {
        Insert(x, list, tmp);
    } // end else
} // end of InsertLast()

/*_______________________________DisposeList Function___________________________________________*/

/*
This Function Deletes All Nodes In the List, and The Head Node...
-----> Time Complexity O(n)...
*/
void DisposeList(DoublyList *list)
{
    PtrToDoublyNode tmp1 = *list;
    PtrToDoublyNode tmp2 = tmp1->Next;

    while (tmp2 != NULL)
    {
        free(tmp1);
        tmp1 = tmp2;
        tmp2 = tmp2->Next;
    } // end while()
    free(tmp1);
    *list = NULL;
} // end DisposeList()

/*_______________________________CopyList Function___________________________________________*/

/*
This Function To Copy List to Another List....
-----> Time Complexity O(n)
*/
DoublyList CopyList(DoublyList list, DoublyList copiedList)
{
    DoublyList newList = NULL;
    CreatDoublyList(&newList);
    newList->isNegative = copiedList->isNegative;

    DoublyNode tmp1 = copiedList->Next;
    DoublyNode lastNode = newList;

    if (copiedList == NULL)
    {
        printf("Null Pointer");
    }
    else
    {
        while (tmp1 != NULL)
        {
            Insert(tmp1->Data, newList, lastNode);
            lastNode = lastNode->Next;
            tmp1 = tmp1->Next;
        }
    }

    return newList;
} // end of CopyList()

/*_______________________________CompareLists Function___________________________________________*/

/*
This Function Compares Between Two Integers (lists)
returns 1 >>>> list1 > list2
returns 0 >>>> list1 == list2
returns -1 >>> list1 < list2
*/
int CompareLists(DoublyList list1, DoublyList list2)
{
    // if(list1->isNegative == true && list2->isNegative == false)
    // {
    //     return -1;
    // } // end if()
    // else if(list1->isNegative == false && list2->isNegative == true)
    // {
    //     return 1;
    // } // end else if()

    long size1 = Size(list1);
    long size2 = Size(list2);

    PtrToDoublyNode tmp1 = list1->Next;
    PtrToDoublyNode tmp2 = list2->Next;

    if (size1 > size2)
    {
        return 1;
    } // end if()
    else if (size2 > size1)
    {
        return -1;
    } // end if()
    else
    {
        for (int i = 0; i < size1; i++)
        {
            if (tmp1->Data > tmp2->Data)
            {
                return 1;
            } // end if()
            else if (tmp1->Data < tmp2->Data)
            {
                return -1;
            } // end else if()
            tmp1 = tmp1->Next;
            tmp2 = tmp2->Next;
        } // end for()
        return 0;
    } // end else
} // end CompareLists()

/*_______________________________RemoveLeftZeros Function___________________________________________*/

void RemoveLeftZeros(DoublyList *list)
{
    PtrToDoublyNode tmp = (*list)->Next;
    while (tmp->Next != NULL && tmp->Data == 0)
    {
        PtrToDoublyNode nextNode = tmp->Next;
        free(tmp);
        tmp = nextNode;
    }
    (*list)->Next = tmp;
} // end of RemoveLeftZeros()

int IsEmptyQueue(Queue queue)
{
    return queue->Next == NULL;
} // end of IsEmptyStack()

/*______________________________Queue Library___________________________________________*/

/*___________________________________CreateQueue Function_______________________________________*/

Queue CreateQueue()
{
    Queue queue = malloc(sizeof(node));
    if (queue == NULL)
    {
        printf("Out Of Memory!");
        return NULL;
    } // end if()
    else
    {
        queue->Next = NULL;
        MakeEmptyQueue(queue);
        return queue;
    } // end esle
} // end of CreateStack()

/*_____________________________________MakeEmptyQueue Function_____________________________________*/
/*
This Function Deletes the Queue Nodes Except the Head Node...
In Addition, this Function free the LinkedLists in The Queue...
*/
void MakeEmptyQueue(Queue queue)
{
    if (queue == NULL)
    {
        printf("THE MEMORY IS OUT OF SPACE!");
    } // end of if()
    else
    {
        while (!IsEmptyQueue(queue))
        {
            Dequeue(queue);
        } // end of while()
    }     // end of else
} // end of MakeEmptyStack()

/*_____________________________________Dequeue Function_____________________________________*/

void Dequeue(Queue queue)
{
    Queue temp = NULL;
    if (IsEmptyQueue(queue))
    {
        printf("THE QUEUE IS EMPTY!");
    } // end of if()
    else
    {
        temp = queue->Next;
        queue->Next = queue->Next->Next;
        DisposeList(&(temp->Data));
        free(temp);
    } // end of else
} // end of Dequeue()

/*____________________________________Front Function______________________________________*/
/*
Returns The Firs Node
---->>> Time Complexity: O(1) (Constant)...
*/
DoublyList Front(Queue queue)
{
    if (IsEmptyQueue(queue))
    {
        printf("THE STACK IS EMPTY!");
    } // end of if()
    else
    {
        return queue->Next->Data;
    } // end of else
} // end of Top()

/*__________________________________Enqueue Function________________________________________*/

/*
This Function Add a New Node To the Last
---> Time Complexity: O(n)
*/
void Enqueue(DoublyList x, Queue queue)
{
    QueueNode tmp = malloc(sizeof(node));
    PtrToQueueNode ptr = queue;
    if (tmp == NULL)
    {
        printf("Out Of Memory!");
    } // end of if()
    else
    {
        tmp->Data = x;
        while (ptr->Next != NULL)
        {
            ptr = ptr->Next;
        } // end while()
        tmp->Next = NULL;
        ptr->Next = tmp;
    } // end of else
} // end of Push()

/*__________________________________DisposeQueue Function________________________________________*/
/*
This Function Deletes All Nodes in the Queue, and The Head Node...
*/
void DisposeQueue(Queue queue)
{
    MakeEmptyQueue(queue);
    free(queue);
} // end of DisposeStack()

/*_________________________________QueueSize Function_________________________________________*/

/*
This Function Returns the Size Of the Queue...
*/
int QueueSize(Queue queue)
{
    QueueNode tmp = queue->Next;
    int size = 0;
    while (tmp != NULL)
    {
        size++;
        tmp = tmp->Next;
    } // end while()
    return size;
} // end of StackSize()

/*_________________________________PrintQueue Function_________________________________________*/

void PrintQueue(Queue queue)
{
    int line = 0;
    Queue tmp = queue;
    if (queue->Next == NULL)
    {
        printf("Empty Queue!\n");
    } // end if()
    else
    {
        while (tmp->Next != NULL)
        {
            tmp = tmp->Next;
            printf("%d. ", ++line);
            PrintList(tmp->Data);
        } // end while()
    }
} // end PrintQueue()

/*_________________________________End Of File_________________________________________*/
