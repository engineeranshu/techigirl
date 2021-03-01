# techigirl

/*

 performs text editing operations on a source string

*/
#include<stdio.h>
#include<string.h>
#include<ctype.h>

#define max_len 100
#define not_found -1


char *delete(char *source, int index, int n);
char *do_edit(char *source, char command);
char get_command();
char *insert(char *source, const char *to_insert, int index);
int pos(const char *source, const char *to_find);


int main()
{
    char source[max_len], command;
    printf("enter the source string:");
    printf("\n");
    gets(source);
    for( command = get_command();
            command != 'q';
            command = get_command())
    {
        do_edit(source, command);
        printf("new source: %s\n\n",source);
    }

    printf("string after editing: %s\n", source);
    return (0);
}

/*

 * returns source after deleting n characters beginning with source [index].
 * if source is too short for full deletion, as many characters are
 * deleted as possible.
 * pre: all parameters are defined and
 *.     strlen(source) - index - n < max_len
 * Post: source is modified and returned.

*/
char *delete(char *source, /* input/output - string from which to delete part. */
             int index,  /* input-index of first char to delete.               */
             int n )  /* input - number of chars to delete.                      */
{
    char rest_str[max_len];   /* copy of source substring following
                                   characters to delete.  */

    /*

    * if there are no characters in source following portion to
    *  delete, delete rest of string.

     */

    if( strlen(source) <= index + n)
    {
        source[index]= '\0';

        /* otherwise, copy the portion following the portion to delete
         * and place it in source beginning at the index positio.
         */

    }
    else
    {
        strcpy(rest_str,&source[index+n]);
        strcpy(&source[index], rest_str);
    }
    return (source);
}

/*
 * performs the edit operation specified by command
 * pre: command and source are defined.
 * post: after scanning additional information needed, performs a
 *       deletion (command ='D') or insertion (command ='I') or
 *       finds a substring ('F') and displays result: returns
 *       (possibly modified) source.
 */


char *do_edit( char *source,  /* input/output - string to modify or search */
               char command)   /* input - character indicating operation.   */
{
    char str[max_len]; /* work string */
    int index;
    switch(command)
    {
    case 'D':
        printf("string to delete>");
        gets(str);
        index = pos(source, str);
        if(index == not_found)
            printf("'%s' not found\n",str);
        else
            delete(source,index,strlen(str));
        break;

    case 'I':
        printf("string to insert>");
        gets(str);
        printf("position of insertion");
        scanf("%d",&index);
        insert(source, str, index);
        break;

    case 'F':
        printf("string to find>");
        gets(str);
        index = pos(source, str);
        if(index == not_found)
        {
            printf("'%s' not found",str);
            printf("\n");
        }
        else
            printf("'%s' found at position %d\n", str, index);
        break;
    default :
        printf("invalid edit command '%c'\n ", command);
    }

    return (source);
}


/*
 * prompt for and get a character representing an edit command and
 * convert it to uppercase. returns the uppercase character and ignore
 * rest of input line.
 */

char get_command()
{
    char command, ignore;
    printf("enter d(delete), I(insert), f(find), q(quit)>");
    scanf("%c", &command);

    do
        ignore = getchar();
    while( ignore !='\n');

    return( toupper(command));
}


/*
 * returns source after inserting to_insert at position index of
 * source. if source[index] doesn't exist, adds to to_insert at end of
 * source.
 * pre : all parameters are defined, space available for source is
 *       enough to accommodate insertion, and
 *       strlen(source) - index -n < max_len
 * post : source is modified and returned.

 */


char *insert(char *source,  /* input/output - target of insertion. */
             const char *to_insert,  /* input - string to insert.  */
             int index)             /* input - position where to_insert
                                                is to be inserted. */
{
    char rest_str[max_len];
    if( strlen(source) <= index)
    {
        strcat( source, to_insert);
    }
    else
    {
        strcpy(rest_str,&source[index]);
        strcpy(&source[index], to_insert);
        strcat(source, rest_str);
    }
    return (source);
}


/*
 * returns index of first occurrence of to_find in source or
 * value of not_found if to_find is not in source.
 * pre : both parameters are defined.
 */

int pos(const char *source, const char *to_find)
{
    int i=0, find_len, found = 0, position;
    char substring[max_len];

    find_len =strlen(to_find);
    while (!found && i<=strlen(source) - find_len)
    {
        strncpy(substring, &source[i], find_len);
        substring[find_len] = '\0';

        if(strcmp(substring, to_find) ==0)
            found = 1;
        else
            ++i;
    }
    if( found )
        position = i ;
    else
        position = not_found;

    return(position);


}


    
