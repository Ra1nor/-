#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

// Функция для отображения содержимого файла
void displayFileContent(const char *fileName) {
    FILE *file = fopen(fileName, "r");
    if (file == NULL) {
        perror("Ошибка при открытии файла");
        return;
    }
    int num;
    while (fscanf(file, "%d", &num) != EOF) {
        printf("%d ", num);
    }
    fclose(file);
    printf("\n");
}

// Функция для поиска значения в файле
void findValueInFile(const char *fileName, int value) {
    FILE *file = fopen(fileName, "r");
    if (file == NULL) {
        perror("Ошибка при открытии файла");
        return;
    }
    int num, found = 0;
    while (fscanf(file, "%d", &num) != EOF) {
        if (num == value) {
            printf("Найдено значение: %d\n", value);
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("Значение %d не найдено.\n", value);
    }
    fclose(file);
}

// Функция для отображения всех чётных чисел из файла
void displayEvenNumbers(const char *fileName) {
    FILE *file = fopen(fileName, "r");
    if (file == NULL) {
        perror("Ошибка при открытии файла");
        return;
    }
    int num;
    printf("Чётные числа: ");
    while (fscanf(file, "%d", &num) != EOF) {
        if (num % 2 == 0) {
            printf("%d ", num);
        }
    }
    fclose(file);
    printf("\n");
}

// Функция для записи умножения "столбиком" в новый файл
void multiplicationToNewFile(const char *fileName, const char *newFileName) {
    FILE *file = fopen(fileName, "r");
    if (file == NULL) {
        perror("Ошибка при открытии исходного файла");
        return;
    }

    int a, b;
    if (fscanf(file, "%d%d", &a, &b) != 2) {
        printf("Недостаточно данных для выполнения умножения.\n");
        fclose(file);
        return;
    }
    fclose(file);

    FILE *newFile = fopen(newFileName, "w");
    if (newFile == NULL) {
        perror("Ошибка при создании нового файла");
        return;
    }

    fprintf(newFile, "%d\n", a);
    fprintf(newFile, "x\n");
    fprintf(newFile, "%d\n", b);
    fprintf(newFile, "-----------\n");

    int result = a * b;
    fprintf(newFile, "%d\n", result);
    fclose(newFile);

    printf("Результат умножения записан в файл %s\n", newFileName);
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Введите имя файла как аргумент командной строки.\n");
        return 1;
    }

    const char *fileName = argv[1];

    // Пример использования функций
    FILE *file = fopen(fileName, "w");
    if (file == NULL) {
        perror("Ошибка при создании файла");
        return 1;
    }

    printf("Введите целые числа для записи в файл (для завершения введите нечисловое значение):\n");
    int num;
    while (scanf("%d", &num) == 1) {
        fprintf(file, "%d ", num);
    }
    fclose(file);

    printf("Содержимое файла:\n");
    displayFileContent(fileName);

    printf("Введите число для поиска в файле: ");
    int valueToFind;
    scanf("%d", &valueToFind);
    findValueInFile(fileName, valueToFind);

    printf("Чётные числа в файле:\n");
    displayEvenNumbers(fileName);

    multiplicationToNewFile(fileName, "result.txt");

    return 0;
}
