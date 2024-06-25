# Analisis-Algoritma-Pengurutann
Membuat kode program menggunakan bahasa pemrograman yang dipilih untuk mengimplementasikan dan menganalisis algoritma pengurutan data angka secara ascending (dari kecil ke besar). Algoritma yang harus diimplementasikan adalah:  Bubble Sort Insertion Sort Selection Sort Merge Sort Quick Sort

#include <iostream>
#include <vector>
#include <algorithm>
#include <chrono>

using namespace std;
using namespace std::chrono;

// Bubble Sort
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; i++) {
        for (int j = 0; j < n-i-1; j++) {
            if (arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
            }
        }
    }
}

// Insertion Sort
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}

// Selection Sort
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n-1; i++) {
        int min_idx = i;
        for (int j = i+1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        swap(arr[min_idx], arr[i]);
    }
}

// Merge Sort
void merge(vector<int>& arr, int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;
    vector<int> L(n1), R(n2);

    for (int i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(vector<int>& arr, int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

// Quick Sort
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = (low - 1);
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return (i + 1);
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

    //Pengukuran Waktu Eksekusi
    void measureTime(void (*sortFunc)(vector<int>&), vector<int>& arr) {
    auto start = high_resolution_clock::now();
    sortFunc(arr);
    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop - start);
    cout << duration.count() << " μs" << endl;
}

void measureTimeMerge(void (*sortFunc)(vector<int>&, int, int), vector<int>& arr) {
    auto start = high_resolution_clock::now();
    sortFunc(arr, 0, arr.size() - 1);
    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop - start);
    cout << duration.count() << " μs" << endl;
}

void measureTimeQuick(void (*sortFunc)(vector<int>&, int, int), vector<int>& arr) {
    auto start = high_resolution_clock::now();
    sortFunc(arr, 0, arr.size() - 1);
    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop - start);
    cout << duration.count() << " μs" << endl;
}

    //Pengujian Kondisi Data dan Tabel Running Time
    int main() {
    vector<int> sizes = {10, 100, 500, 1000, 10000};

    for (int size : sizes) {
        cout << "Size: " << size << endl;
        
        vector<int> arr_random(size);
        generate(arr_random.begin(), arr_random.end(), rand);

        vector<int> arr_reverse(size);
        iota(arr_reverse.rbegin(), arr_reverse.rend(), 1);

        vector<int> arr_sorted(size);
        iota(arr_sorted.begin(), arr_sorted.end(), 1);

        cout << "Bubble Sort:" << endl;
        vector<int> arr = arr_random;
        measureTime(bubbleSort, arr);

        arr = arr_reverse;
        measureTime(bubbleSort, arr);

        arr = arr_sorted;
        measureTime(bubbleSort, arr);

        cout << "Insertion Sort:" << endl;
        arr = arr_random;
        measureTime(insertionSort, arr);

        arr = arr_reverse;
        measureTime(insertionSort, arr);

        arr = arr_sorted;
        measureTime(insertionSort, arr);

        cout << "Selection Sort:" << endl;
        arr = arr_random;
        measureTime(selectionSort, arr);

        arr = arr_reverse;
        measureTime(selectionSort, arr);

        arr = arr_sorted;
        measureTime(selectionSort, arr);

        cout << "Merge Sort:" << endl;
        arr = arr_random;
        measureTimeMerge(mergeSort, arr);

        arr = arr_reverse;
        measureTimeMerge(mergeSort, arr);

        arr = arr_sorted;
        measureTimeMerge(mergeSort, arr);

        cout << "Quick Sort:" << endl;
        arr = arr_random;
        measureTimeQuick(quickSort, arr);

        arr = arr_reverse;
        measureTimeQuick(quickSort, arr);

        arr = arr_sorted;
        measureTimeQuick(quickSort, arr);

        cout << endl;
    }

    return 0;
}


