# markify.c

Markify is a...

## Dependencies

    #include <stdio.h> // fprintf, stdin, stdout, stderr
    #include <stdlib.h> // EXIT_FAILURE, EXIT_SUCCESS
    #include <stdbool.h>

## Prototypes

    static inline bool isSpace(char ch);

## Constants

The macro `MAXIMUM_LINE_WIDTH` determines the maximum line-width.


    #define MAXIMUM_LINE_WIDTH 4096


We categorize lines into four types; *empty*, *code*, *markdown*, and *empty markdown*. These
types are captured in the following enum:

## Types

    typedef enum
    {
        LineType_empty = 0,
        LineType_code = 1,
        LineType_markdown = 2,
        LineType_markdown_empty = 3
    }
    LineType;

## Functions

The function `determineLineType` determines the type of a line.


    static inline LineType determineLineType(char const * line)
    {
        char const * ptr = line;


/// First, we remove all trailing spaces in the input line.


        while (isSpace(*ptr))
        {
            ptr++;
        }

        if (*ptr == '\n')
        {
            return LineType_empty;
        }
        else if (ptr[0] == '/' && ptr[1] == '/' && ptr[2] == '/')
        {
            if (ptr[3] == ' ')
            {
                return LineType_markdown;
            }
            else if (ptr[3] == '\n')
            {
                return LineType_markdown_empty;
            }
            else
            {
                return LineType_code;
            }
        }
        else
        {
            return LineType_code;
        }
    }


`isSpace` returns true if the character `ch` is either a space or a tab.


    static inline bool isSpace(char ch)
    {
        return ch == ' ' || ch == '\t';
    }


The main function blah blah blah


    int main()
    {
        bool removeEmptyLines = true;

        char line [MAXIMUM_LINE_WIDTH];

        FILE * file = fopen("main.c", "r");

        while (fgets(line, MAXIMUM_LINE_WIDTH, file) != NULL)
        {
            switch (determineLineType(line))
            {
                case LineType_empty:

                    if (!removeEmptyLines)
                    {
                        fputs(line, stdout);
                    }

                    continue;

                case LineType_code:

                    if (removeEmptyLines)
                    {
                        fprintf(stdout, "\n");
                    }

                    fprintf(stdout, "    ");
                    fputs(line, stdout);

                    removeEmptyLines = false;

                    continue;

                case LineType_markdown:

                    fputs(line + 4, stdout);

                    removeEmptyLines = true;

                    continue;

                case LineType_markdown_empty:

                    fputc('\n', stdout);

                    removeEmptyLines = true;

                    continue;
            }
        }

        return EXIT_SUCCESS;
    }
Program ended with exit code: 0
