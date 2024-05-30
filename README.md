# levenshtein.c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Function to calculate the Levenshtein distance between two strings
int levenshtein_distance(const char *a, const char *b)
{
    int n = strlen(a);
    int m = strlen(b);

    // If one of the strings is empty, return the length of the other
    if (n == 0) return m;
    if (m == 0) return n;

    // Create a 2D array to store distances
    int **d = (int **)malloc((n + 1) * sizeof(int *));
    for (int i = 0; i <= n; i++)
    {
        d[i] = (int *)malloc((m + 1) * sizeof(int));
    }

    // Initialize the distance matrix
    for (int i = 0; i <= n; i++)
    {
        d[i][0] = i;
    }
    for (int j = 0; j <= m; j++)
    {
        d[0][j] = j;
    }

    // Populate the distance matrix
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            int cost = (a[i - 1] == b[j - 1]) ? 0 : 1;
            d[i][j] = d[i - 1][j] + 1; // Deletion
            if (d[i][j] > d[i][j - 1] + 1) d[i][j] = d[i][j - 1] + 1; // Insertion
            if (d[i][j] > d[i - 1][j - 1] + cost) d[i][j] = d[i - 1][j - 1] + cost; // Substitution
        }
    }

    int result = d[n][m];

    // Free allocated memory
    for (int i = 0; i <= n; i++)
    {
        free(d[i]);
    }
    free(d);

    return result;
}

int main()
{
    const char fragment[256];
    fgets(fragment, sizeof(fragment), stdin);

    const char *valid_model = "func(myFunction)";

    int min_operations = levenshtein_distance(fragment, valid_model);
    printf("Numarul minim de operatii necesare: %d\n", min_operations);

    return 0;
}
