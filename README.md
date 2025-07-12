# Student-Database-Management-System
# 📚 Student Database Management System – C (Personal Project)

A personal project developed in **C** to manage student records using **singly linked lists**. The system provides a command-line interface to perform common operations on a dynamic student database.

## 🔧 Features

- ✅ Add, edit, search, and delete student records  
- 🧹 Delete all records with a single command  
- 🎲 Insert randomly generated student data for testing  
- 💾 Save and load data using file operations  
- 📝 Generate a formatted report file of all records  
- 📜 Menu-driven user interface for intuitive usage

## 🛠️ Tech Stack

- **Language:** C  
- **Data Structures:** Singly Linked List  
- **Core Concepts:** Dynamic memory management, file handling, modular programming

## 🎯 Objective

This project was built to practice and strengthen core skills in C programming, including data structures and file I/O, by developing a fully functional console-based student management system.

## SOURCE CODE:

<pre lang="markdown"> ```// Student databse system using c programming 
#include<stdio.h>
#include<stdlib.h>
#include<stdbool.h>
#include<string.h>
#include<windows.h> 
#include<time.h>
//#include<unistd.h> // for linux os
struct StudentInfo
{
    char ID[10];
    char name[20];
    char email[30];
    char phone[14]; //eg: +911234567890
    int numberofcourses;
    struct StudentInfo *next;
};

struct CourseInfo
{
    char StudentID[10];
    char Code[10];
    char Name[20];
    struct CourseInfo *next;
};

//head pointers of the linked list
struct StudentInfo* studentHead=NULL;
struct CourseInfo* courseHead=NULL;

int TotalStudents=0;
int TotalCourse =0;
char StudentID[10];
bool Running=true;

void Menu();
void AddNewStudent();
void ShowAllStudent();
void SearchStudent(char StudentID[10]);
void EditStudent(char StudentID[10]);
void DeleteStudent(char StudentID[10]);
void DeleteAllStudents();
void SaveToFile();
void LoadFromFile();
void GenerateReport();

void ReturnFunction();
void ExitProject();
int  IsAlreadyExists(char GivenLine[300], char InfoType, char StudentID[300]);
void RandomData();
void AddStudentDirectly(struct StudentInfo* , struct CourseInfo*);


int main()
{
    while(1)
    {
        Menu();
        int Option;
        scanf("%d",&Option);
        switch(Option)
        {
            case 1:
            system("cls");
            printf("\n----Add A New Student's Details----\n");
            AddNewStudent();
            ReturnFunction();
            break;

            case 2:
            system("cls");
            printf("\n----All Student's Details----\n");
            ShowAllStudent();
            ReturnFunction();
            break;

            case 3:
            system("cls");
            printf("\n----Student Search Details----\n");
            char id[10];
            printf("Enter the student ID: ");
            scanf("%s",id);
            SearchStudent(id);
            ReturnFunction();
            break;

            case 4:
            system("cls");
            printf("\n----Edit Student Details----\n");
            char id1[10];
            printf("Enter the student ID: ");
            scanf("%s",id1);
            EditStudent(id1);
            ReturnFunction();
            break;

            case 5:
            system("cls");
            printf("\n----Delete A Student Details----\n");
            char id2[10];
            printf("Enter the student ID: ");
            scanf("%s",id2);
            DeleteStudent(id2);
            ReturnFunction();
            break;

            case 6:
            system("cls");
            printf("\n----Delete All Student Details----\n");
            DeleteAllStudents();
            ReturnFunction();
            break;

            case 7:
            system("cls");
            RandomData();
            ReturnFunction();
            break;
            
            case 8:
            system("cls");
            SaveToFile();
            ReturnFunction();
            break;

            case 9:
            system("cls");
            LoadFromFile();
            ReturnFunction();
            break;

            case 10:
            system("cls");
            GenerateReport();
            ReturnFunction();
            break;

            case 11:
            ExitProject();
            
            default:
            printf("Invalid options... try again\n");
            ReturnFunction();
            break;
        }
    }
}

void Menu()
{
    printf("\nStudent Database Management System Using C\n");
    printf("\t   MENU:\n");
    printf("----------------------------\n");
    printf("(1)  Add A New Student.\n");
    printf("(2)  Show All students.\n");
    printf("(3)  Search A Student.\n");
    printf("(4)  Edit A Student.\n");
    printf("(5)  Delete A Student.\n");
    printf("(6)  Delete All Student.\n");
    printf("(7)  Insert Random Student Data\n");
    printf("(8)  Store the information into file\n");
    printf("(9)  Take information from file\n");
    printf("(10) Generate Report File\n");
    printf("(11) Stop the Program\n");
    printf("----------------------------\n");
    printf("Enter the Choice:\n");
}

void AddNewStudent()
{
    struct StudentInfo* newStudent =(struct StudentInfo*)malloc(sizeof(struct StudentInfo));
    if(!newStudent)
    {
        printf("memory allocation failed!\n");
        return ;
    }

    char CourseCode[300];
    char CourseName[400];
    int NumberOfCourses;

    int IsValidID=0;
    while(!IsValidID)
    {
        printf("Enter the ID: ");
        scanf("%s",newStudent->ID);

        if(IsAlreadyExists(newStudent->ID,'i',newStudent->ID)>0)
        printf("Error: ID already exists\n");
        else if(strlen(newStudent->ID)>10 ||strlen(newStudent->ID)<=0)
        printf("Error: Invalid ID Length\n");
        else
        IsValidID=1;
    }

    printf("Enter the name: ");
    scanf(" %[^\n]",newStudent->name);
    printf("Enter the Email: ");
    scanf("%s",newStudent->email);
    printf("Enter the phone number: ");
    scanf("%s",newStudent->phone);
    printf("Number of courses: ");
    scanf("%d",&NumberOfCourses);
    newStudent->numberofcourses=NumberOfCourses;

    newStudent->next=studentHead;
    studentHead=newStudent;
    TotalStudents++;

    for(int i=0;i<NumberOfCourses;i++)
    {
        struct CourseInfo* newCourse=(struct CourseInfo*)malloc(sizeof(struct CourseInfo));
        if(!newCourse)
        {
            printf("Memory allocation failed!\n");
            return ;
        }

        printf("Enter the course %d code:",i+1);
        scanf("%s",CourseCode);
        printf("Enter the course %d name:",i+1);
        scanf(" %[^\n]",CourseName);

        strcpy(newCourse->StudentID,newStudent->ID);
        strcpy(newCourse->Code,CourseCode);
        strcpy(newCourse->Name,CourseName);

        newCourse->next=courseHead;
        courseHead=newCourse;
        TotalCourse++;

    }

    printf("Student Details Added Successfully\n");

    
}

void ShowAllStudent()
{
    printf("|********|*****************|*********************|*****************|******************|\n");
    printf("|   ID   |      Name       |        Email        |       Phone     |   No.of Courses  |\n");
    printf("|********|*****************|*********************|*****************|******************|\n");

    struct StudentInfo* Current = studentHead; // temp node to iterate through the links
    while(Current!=NULL)
    {
        printf("|--------|-----------------|---------------------|-----------------|------------------|\n");
        printf("|%-8s|%-17s|%-21s|%-17s|%-18d|\n",Current->ID,Current->name,Current->email,Current->phone,Current->numberofcourses);
        printf("|--------|-----------------|---------------------|-----------------|------------------|\n");
        Current=Current->next;
    }
    printf("\n");

}

void SearchStudent(char StudentID[10])
{
    if(studentHead==NULL)
    {
        printf("No Student record present!\n");
        return;
    }
    else
    {
        struct StudentInfo* temp= studentHead; //making a copy of student head pointer
        while(temp!=NULL)
        {
            if(strcmp(temp->ID,StudentID)==0) // ==0 means, same
            {
                printf("Student's ID=           %s\n",temp->ID);
                printf("Student's Name=         %s\n",temp->name);
                printf("Student's Phone number= %s\n",temp->phone);
                printf("Student's Email ID=     %s\n",temp->email);
                printf("Courses enrolled=       %d\n",temp->numberofcourses);

                //printf the course details:
                struct CourseInfo* details= courseHead; // making a copy of course head pointer
                int coursefound=0;
                while(details!=NULL)
                {
                    if(strcmp(details->StudentID,StudentID)==0)
                    {
                        printf("Course code -> %s\nCourse name -> %s\n",details->Code,details->Name);
                        coursefound=1;
                    }
                    details=details->next;
                }
                if(coursefound==0)
                {
                    printf("No course details found!\n");
                }
                return; // exit the funciton after finding the student.
            }
            temp=temp->next;
        }
        printf("No student details present with the student ID %s\n",StudentID);
        
    }
}

void EditStudent(char StudentID[10])
{
    if(studentHead==NULL)
    {
        printf("No student record present!\n");
        return;
    }
    else
    {
        struct StudentInfo* temp=studentHead;
        while(temp!=NULL)
        {

            if(strcmp(temp->ID,StudentID)==0)
            {
                getchar(); // to clear the input buffer

                //editing ID
                printf("Student's ID=           %s\n",temp->ID);
                printf("Enter the new ID: (press enter to skip)\n");
                char tempID[10];
                fgets(tempID,sizeof(tempID),stdin);
                if(tempID[0]!='\n')
                {
                    tempID[strcspn(tempID,"\n")]='\0';
                    strcpy(temp->ID,tempID);
                }

                //editing name
                printf("Student's Name=         %s\n",temp->name);
                printf("Enter the new Name: (press enter to skip)\n");
                char tempName[50];
                fgets(tempName,sizeof(tempName),stdin);
                if(tempName[0]!='\n')
                {
                    tempName[strcspn(tempName,"\n")]='\0';
                    strcpy(temp->name,tempName);
                }

                  //editing phone number
                printf("Student's Phone number=         %s\n",temp->phone);
                printf("Enter the new phone number: (press enter to skip)\n");
                char tempNumber[50];
                fgets(tempNumber,sizeof(tempNumber),stdin);
                if(tempNumber[0]!='\n')
                {
                    tempNumber[strcspn(tempNumber,"\n")]='\0';
                    strcpy(temp->phone,tempNumber);
                }

                  //editing Email
                printf("Student's Email ID=         %s\n",temp->email);
                printf("Enter the new Email ID: (press enter to skip)\n");
                char tempEmail[30];
                fgets(tempEmail,sizeof(tempEmail),stdin);
                if(tempEmail[0]!='\n')
                {
                    tempEmail[strcspn(tempEmail,"\n")]='\0';
                    strcpy(temp->email,tempEmail);
                }

                //editing the number of courses
                printf("Number of Courses enrolled=       %d\n",temp->numberofcourses);
                printf("Enter the new no.of courses: (press enter to skip)\n");
                char tempCourse[10]; //can't take input as number since the \n condition is checking for skipping
                fgets(tempCourse,sizeof(tempCourse),stdin);

                if(tempCourse[0]!='\n')
                {
                    tempCourse[strcspn(tempCourse,"\n")]='\0';
                    int newCount=atoi(tempCourse);
                    temp->numberofcourses=newCount;

                    //delete the old courses for this student.
                    struct CourseInfo* curr=courseHead;
                    struct CourseInfo* prev=NULL;

                    while(curr!=NULL)
                    {
                        if(strcmp(curr->StudentID,temp->ID)==0)
                        {
                            struct CourseInfo* toDelete=curr;
                            if(prev==NULL)
                            {
                                courseHead=curr->next;
                                curr=courseHead;
                            }
                            else
                            {
                                prev->next=curr->next;
                                curr=prev->next;
                            }
                            free(toDelete);
                            TotalCourse--;
                            continue; // recheck the current position
                        }
                        else
                        {
                            prev=curr;
                            curr=curr->next;
                        }
                    }

                    //Add new course details..
                    for(int i=0;i<newCount;i++)
                    {
                        struct CourseInfo* newCourse=(struct CourseInfo*)malloc(sizeof(struct CourseInfo));
                        if(!newCourse)
                        {
                            printf("Memory Allocation failed for course\n");
                            return;
                        }

                        //getchar();
                        printf("Enter the course %d code: ",i+1);
                        fgets(newCourse->Code,sizeof(newCourse->Code),stdin);
                        newCourse->Code[strcspn(newCourse->Code,"\n")]='\0';
                        printf("Enter the course %d name: ",i+1);
                        fgets(newCourse->Name,sizeof(newCourse->Name),stdin);
                        newCourse->Name[strcspn(newCourse->Name,"\n")]='\0';
                        
                        strcpy(newCourse->StudentID,temp->ID);

                        newCourse->next=courseHead;
                        courseHead=newCourse;
                        TotalCourse++;

                    }
                    printf("Course Details updated successfully\n");

                }
                printf("Student Details updated successfully\n");
                return ; // exit the funciton after the student details are edited, no need to iterate through the whole list after this.
            }
            temp=temp->next;
        }
        printf("No Student found with the id:%s\n",StudentID);
    }
}

void DeleteStudent(char StudentID[10])
{
    struct StudentInfo* current=studentHead;
    struct StudentInfo* prev=NULL;
    int found=0;

    while(current!=NULL)
    {
        if(strcmp(current->ID,StudentID)==0)
        {
            //Delete that student node
            if(prev==NULL)
            studentHead=current->next;
            else
            prev->next=current->next;

            //Delete the course nodes corresponding to this student also
            struct CourseInfo* currentCourse=courseHead;
            struct CourseInfo* prevCourse=NULL;

            while(currentCourse!=NULL)
            {
                if(strcmp(currentCourse->StudentID,StudentID)==0)
                {
                    struct CourseInfo* toDelete=currentCourse;
                    if(prevCourse==NULL)
                    courseHead=currentCourse->next;
                    else
                    prevCourse->next=currentCourse->next;

                    currentCourse=currentCourse->next;
                    free(toDelete);
                    TotalCourse--;
                    continue;
                }
                prevCourse=currentCourse;
                currentCourse=currentCourse->next;

            }
            free(current);
            TotalStudents--;
            printf("Student with ID %s is deleted!\n",StudentID);
            found=1;
            break;

        }
        prev=current;
        current=current->next;

    }
    if(!found)
    {
        printf("No student found with ID %s.\n",StudentID);
    }
}

void DeleteAllStudents()
{
    if(studentHead==NULL)
    {
    printf("No Student Record Present!\n");
    return;
    }
    char sure;
    printf("Are you sure to delete all the student records? press(y/n)\n");
    getchar(); // clear buffer (\n) gets here if not taken care of.
    scanf("%c",&sure);
    if(sure=='n'||sure=='N')
    return;

    struct StudentInfo* currentStudent;
    while(studentHead!=NULL)
    {
        currentStudent=studentHead;
        studentHead=studentHead->next;
        free(currentStudent);
    }

    struct CourseInfo* currentCourse;
    while(courseHead!=NULL)
    {
        currentCourse=courseHead;
        courseHead=courseHead->next;
        free(currentCourse);
    }

    TotalStudents=0;
    TotalCourse=0;

    printf("All student details have been deleted!\n");
}

void SaveToFile()
{
    FILE *fp=fopen("student_record.txt","w");
    if(!fp)
    {
        printf("Error opening file!\n");
        return;
    }

    struct StudentInfo* temp=studentHead;
    while(temp!=NULL)
    {
        fprintf(fp,"%s|%s|%s|%s|%d\n",temp->ID,temp->name,temp->email,temp->phone,temp->numberofcourses);

        struct CourseInfo* tempCourse=courseHead;
        while(tempCourse!=NULL)
        {
            if(strcmp(tempCourse->StudentID,temp->ID)==0)
            {
                fprintf(fp,"%s|%s\n",tempCourse->Code,tempCourse->Name);
            }
            tempCourse=tempCourse->next;
        }
        temp=temp->next;
    }
    fclose(fp);
    printf("Student record has been exported as \"student_record.txt\" in the directory\n");
}

void LoadFromFile()
{
    FILE *fp=fopen("student_record.txt","r");
    if(!fp)
    {
        printf("No student record present to import!\n");
        return ;
    }

    DeleteAllStudents(); // Delete the existing nodes before importing

    char line[200];
    while(fgets(line,sizeof(line),fp))
    {
        line[strcspn(line,"\n")]='\0'; //to remove the newline at every line

        //create and fill new nodes.
        struct StudentInfo *newStudent=(struct StudentInfo*)malloc(sizeof(struct StudentInfo));
        if(!newStudent)
        {
            printf("Memory allocation failed for node!\n");
            return ;
        }

        sscanf(line,"%[^|]|%[^|]|%[^|]|%[^|]|%d",newStudent->ID,newStudent->name,newStudent->email,newStudent->phone,&newStudent->numberofcourses);

        newStudent->next=NULL;

        // course information for this student
        struct CourseInfo* courseList =NULL;

        for(int i=0;i<newStudent->numberofcourses;i++)
        {
            if(!fgets(line,sizeof(line),fp))
            break;
            line[strcspn(line,"\n")]='\0';

            struct CourseInfo* newCourse=(struct CourseInfo*)malloc(sizeof(struct CourseInfo));
            if(!newCourse)
            {
                printf("Memory allocation failed for course details!\n");
                return ;
            }

            sscanf(line,"%[^|]|%[^|n]",newCourse->Code,newCourse->Name);
            strcpy(newCourse->StudentID,newStudent->ID);
            newCourse->next=courseList;
            courseList=newCourse;

        }
        AddStudentDirectly(newStudent,courseList);
    }
    fclose(fp);
    printf("Student record has been formed from the file!\n");
}

void GenerateReport()
{
    FILE *fp=fopen("student_report.txt","w");
    if(!fp)
    {
        printf("No Record present to create the report!\n");
        return ;
    }
    struct StudentInfo* temp=studentHead;
    if(!temp)
    {
        printf("No records present to create the report!!!!\n");
        return ;
    }

    fprintf(fp,"************ STUDENT RECORD ************\n\n");

    while(temp!=NULL)
    {
        fprintf(fp,"# Student ID:        %s\n",temp->ID);
        fprintf(fp,"# Name:              %s\n",temp->name);
        fprintf(fp,"# Email:             %s\n",temp->email);
        fprintf(fp,"# Phone:             %s\n",temp->phone);
        fprintf(fp,"# Number of Courses: %d\n",temp->numberofcourses);
        fprintf(fp,"-------------------------------------\n");

        struct CourseInfo* course= courseHead;
        int courseIndex=1;

        while(course!=NULL)
        {
            if(strcmp(course->StudentID,temp->ID)==0)
            {
                fprintf(fp," [%d] Course Code :%s \n",courseIndex,course->Code);
                fprintf(fp,"Course Name: %s\n",course->Name);
                courseIndex++;
            }
            course=course->next;
        }
        if(courseIndex==1)
        {
            fprintf(fp,"No Course record for this student!\n");
            
        }
        fprintf(fp,"=====================================\n\n");
        temp=temp->next;
    
    }
    fclose(fp);
    printf("Student Report has been generated and saved as\"student_report.txt\" in the directory\n");
}

int IsAlreadyExists(char GivenLine[300],char InfoType,char StudentID[300])
{
    int EmailExists=0;
    int PhoneExists=0;
    int IDExists=0;
    
    struct StudentInfo* current=studentHead;
    while(current!=NULL)
    {
        if(strcmp(GivenLine,current->ID)==0)
        {
            IDExists++;
        }
        if (strcmp(GivenLine, current->email)==0 && strcmp(StudentID,current->ID)!=0)
        {
            EmailExists++;
        }
        if (strcmp(GivenLine,current->phone)==0 && strcmp(StudentID,current->ID)!=0)
        {
            PhoneExists++;
        }
        current=current->next;
    }

    if(InfoType=='i')
    {
        return IDExists;
    }
    else if(InfoType=='e')
    {
        return EmailExists;
    }
    else if(InfoType=='p')
    {
        return PhoneExists;
    }
    else
    {
        return 0;
    }
}

void ReturnFunction()
{
    getchar();
    char Option;
    printf(" Go back(b)? or Exit(0)?: ");
        Option=getchar();
        
    if(Option == '0')
    {
        ExitProject();
    }
    else
    {
        system("cls");
    }
}

void ExitProject()
{
    system("cls");
    int i;
    char ThankYou[100]     = " ****** Thank You! ******\n";
    for(i=0; i<strlen(ThankYou); i++)
    {
        printf("%c",ThankYou[i]);
        Sleep(40);
    }
    exit(0);
}

void AddStudentDirectly(struct StudentInfo* newStudent, struct CourseInfo* courseList)
{
    // to insert at the begining of the list
    newStudent->next=studentHead;
    studentHead=newStudent;
    TotalStudents++;

    struct CourseInfo* current =courseList;

    while(current!=NULL)
    {
        struct CourseInfo* temp=current->next;
        current->next=courseHead;
        courseHead=current;
        TotalCourse++;

        current=temp;
    }
    printf("Random Student Details Added Successfully\n");
}

void RandomData()
{
    srand(time(NULL)); //seeding with time funciton to generate random values
    
    struct StudentInfo* newStudent = (struct StudentInfo*)malloc(sizeof(struct StudentInfo));
    if(!newStudent)
    {
        printf("Memory Allocation failed!\n");
        return ;
    }

    int IsValidID=0;
    while(!IsValidID)
    {
        sprintf(newStudent->ID,"ID%04d",rand() %10000);
        if(IsAlreadyExists(newStudent->ID,'i',newStudent->ID)==0)
        IsValidID=1;
    }

    sprintf(newStudent->name,"Student%d",rand()%1000);
    sprintf(newStudent->email,"student@%dgmail.com",rand()%1000);
    sprintf(newStudent->phone,"+91%010d",rand()%1000000000);
    newStudent->numberofcourses =1 + rand() % 3;

    struct CourseInfo* courseList =NULL;
    for(int i=0;i<newStudent->numberofcourses;i++)
    {
        struct CourseInfo* newCourse = (struct CourseInfo*)malloc(sizeof(struct CourseInfo));
        if(!newCourse)
        {
            printf("Memory Allocation Failed!\n");
            return ;
        }

        sprintf(newCourse->StudentID,"%s",newStudent->ID);
        sprintf(newCourse->Code,"CS%03d",rand() % 500);
        sprintf(newCourse->Name,"Course_%03d",rand() % 500);

        newCourse->next=courseList;
        courseList=newCourse;
        
    }
    AddStudentDirectly(newStudent,courseList);

} ``` </pre>

