#include <iostream>
#include <fstream>
#include <vector>
#include <cmath>
#include <algorithm>
// Функція для зчитування даних з файлу та побудови таблиці
std::vector<std::vector<double>> readTableFromFile(const std::string& filename) {
    std::ifstream file(filename);
    if (!file.is_open()) {
        throw std::runtime_error("Помилка відкриття файлу " + filename);
    }
    std::vector<std::vector<double>> table;
    double x, U, T;
    std::string text;
    while (file >> x >> U >> T >> text) {
        table.push_back({x, U, T});
    }
    return table;
}
// Функція для обчислення мінімального значення серед параметрів
double computeMin(const std::vector<double>& params) {
    return *std::min_element(params.begin(), params.end());
}
// Алгоритм 1
double algorithm1(double r, double m, double k) {
    return r - m * k;
}
// Алгоритм 2
double algorithm2(double z, double y, double x) {
    return z * y * (x - y);
}
// Функція для обчислення текстового параметру
double computeTextParameter(const std::string& text, const std::string& filename) {
    std::ifstream file(filename);
    if (!file.is_open()) {
        return 0; // Файл не відкрито, повертаємо нуль
    }
    std::string word;
    double value;
    while (file >> word >> value) {
        if (word == text) {
            return value;
        }
    }
    return 0; // Слово не знайдено, повертаємо нуль
}
int main() {
    double x, y, z;
    std::string text;
    std::cout << "Введіть значення x, y, z та текст: ";
    std::cin >> x >> y >> z >> text;
    try {
        std::vector<std::vector<double>> table4 = readTableFromFile("dat1.dat");
        std::vector<std::vector<double>> table5 = readTableFromFile("dat2.dat");
        std::vector<std::vector<double>> table6 = readTableFromFile("dat3.dat");
        double r = 0;
        double m = computeMin({x, y, z});
        double k = computeTextParameter(text, "dat3.dat");
        double result = algorithm1(r, m, k);
        std::cout << "Результат обчислення func_regr(r,m,k): " << result << std::endl;
    } catch (const std::runtime_error& e) {
        std::cerr << "Помилка: " << e.what() << std::endl;
    }
    return 0;
}
