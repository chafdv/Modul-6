# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 6 DOUBLY LINKED LIST (BAGIAN PERTAMA) </h1>
<p align="center">Osha Alfida Valyana / 103112430202</p>

## Dasar Teori

Doubly linked list adalah struktur data yang lebih kompleks dibandingkan dengan singly linked list, tetapi memiliki beberapa keunggulan. Keunggulan utamanya adalah memungkinkan penelusuran data secara efisien ke dua arah. Hal ini karena setiap node dalam daftar memiliki pointer ke node sebelumnya dan pointer ke node berikutnya. Dengan begitu, proses penyisipan dan penghapusan node dapat dilakukan dengan cepat dan mudah, serta penelusuran data bisa dilakukan dari kedua arah.

## Guided

### Soal 1

```cpp
#include <iostream>
using namespace std;

// ==================
// Struktur Node
// ==================
struct Node {
    int data;
    Node* prev;
    Node* next;
};

// Pointer global
Node* head = nullptr;
Node* tail = nullptr;

// =================================
// FUNGSI UTAMA 
// =================================

// FUNGSI: Insert Depan (Dikoreksi)
void insertDepan(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->prev = nullptr;
    newNode->next = head;

    if (head != nullptr) {
        head->prev = newNode;
    } else {
        tail = newNode; 
    }

    head = newNode;
    cout << "Data " << data << " berhasil ditambahkan di depan. \n";
}

// FUNGSI: Insert Belakang 
void insertBelakang(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->next = nullptr;
    newNode->prev = tail;

    if (tail != nullptr) {
        tail->next = newNode;
    } else {
        head = newNode;
    }

    tail = newNode;
    cout << "Data " << data << " berhasil ditambahkan di belakang. \n";
}

// FUNGSI: Insert Setelah Data tertentu 
void insertSetelah(int target, int data) {
    // 1. Cari target
    Node* current = head; 
    while (current != nullptr && current->data != target) {
        current = current->next;
    }

    if (current == nullptr) {
        cout << "Data target " << target << " tidak ditemukan. \n";
        return;
    }
    
    // 2. Jika target adalah tail, panggil insertBelakang
    if (current == tail) {
        insertBelakang(data);
        return;
    }
    
    // 3. Insert di Tengah
    Node* newNode = new Node();
    newNode->data = data;
    
    newNode->next = current->next;
    newNode->prev = current;
    
    current->next->prev = newNode; // Tautan ke prev di node setelah current
    current->next = newNode;
    
    cout << "Data " << data << " berhasil disisipkan setelah " << target << ".\n";
}

// FUNGSI: Hapus di depan 
void hapusDepan() {
    if (head == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    Node* temp = head;
    head = head->next;

    if (head != nullptr) {
        head->prev = nullptr;
    } else {
        tail = nullptr;
    }

    cout << "Data " << temp->data << " dihapus dari depan. \n";
    delete temp;
}

// FUNGSI: Hapus di belakang 
void hapusBelakang() {
    if (tail == nullptr) {
        cout << "List kosong. \n";
        return;
    }

    Node* temp = tail;
    tail = tail->prev;

    if (tail != nullptr) {
        tail->next = nullptr;
    } else {
        head = nullptr;
    }

    cout << "Data " << temp->data << " dihapus dari belakang. \n";
    delete temp;
}

// FUNGSI: Hapus Data Tertentu 
void hapusData(int target) {
    if (head == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    Node* current = head; 
    
    // Cari node target
    while (current != nullptr && current->data != target) {
        current = current->next;
    }

    if (current == nullptr) {
        cout << "Data " << target << " tidak ditemukan. \n";
        return;
    }

    // KASUS 1: Hapus Head
    if (current == head) {
        hapusDepan();
    } 
    // KASUS 2: Hapus Tail
    else if (current == tail) {
        hapusBelakang();
    } 
    // KASUS 3: Hapus di Tengah
    else {
        current->prev->next = current->next;
        current->next->prev = current->prev;
        
        cout << "Data " << target << " dihapus. \n";
        delete current;
    }
}

// FUNGSI: Update Data 
void updateData(int oldData, int newData) {
    Node* current = head;
    
    // Cari node oldData
    while (current != nullptr && current->data != oldData) {
        current = current->next;
    }

    if (current == nullptr) {
        cout << "Data " << oldData << " tidak ditemukan untuk diperbarui. \n";
    } else {
        current->data = newData;
        cout << "Data " << oldData << " berhasil diperbarui menjadi " << newData << ". \n";
    }
}

// FUNGSI: Tampil dari Depan 
void tampilDepan() {
    if (head == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    cout << "Isi list (dari depan): ";
    Node* current = head;
    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }
    cout << "\n";
}

// FUNGSI: Tampil dari Belakang 
void tampilBelakang() {
    if (tail == nullptr) {
        cout << "List kosong.\n";
        return;
    }

    cout << "Isi list (dari belakang): ";
    Node* current = tail;
    while (current != nullptr) {
        cout << current->prev << " ";
        current = current->prev;
    }
    cout << "\n";
}

// ====================================
// MAIN PROGRAM (MENU INTERAKTIF)
// ====================================
int main() {
    int pilihan, data, target, oldData, newData;

    do {
        cout << "\n===== MENU DOUBLE LINKED LIST =====\n";
        cout << "1. Insert Depan\n";
        cout << "2. Insert Belakang\n";
        cout << "3. Insert Setelah Data\n";
        cout << "4. Hapus Depan\n";
        cout << "5. Hapus Belakang\n";
        cout << "6. Hapus Data Tertentu\n";
        cout << "7. Update Data\n";
        cout << "8. Tampil dari Depan\n";
        cout << "9. Tampil dari Belakang\n";
        cout << "0. Keluar\n";
        cout << "===================================\n";
        cout << "Pilih menu: ";
        
        if (!(cin >> pilihan)) {
            cout << "Input tidak valid.\n";
            cin.clear();
            cin.ignore(10000, '\n');
            pilihan = -1;
            continue;
        }

        switch (pilihan) {
            case 1:
                cout << "Masukkan data: ";
                cin >> data;
                insertDepan(data);
                break;
            case 2:
                cout << "Masukkan data: ";
                cin >> data;
                insertBelakang(data);
                break;
            case 3:
                cout << "Masukkan data target: ";
                cin >> target;
                cout << "Masukkan data baru: ";
                cin >> data;
                insertSetelah(target, data);
                break;
            case 4:
                hapusDepan();
                break;
            case 5:
                hapusBelakang();
                break;
            case 6:
                cout << "Masukkan data yang ingin dihapus: ";
                cin >> target;
                hapusData(target);
                break;
            case 7:
                cout << "Masukkan data lama: ";
                cin >> oldData;
                cout << "Masukkan data baru: ";
                cin >> newData;
                updateData(oldData, newData);
                break;
            case 8:
                tampilDepan();
                break;
            case 9:
                tampilBelakang();
                break;
            case 0:
                cout << " Keluar dari program.\n";
                break;
            default:
                cout << "Pilihan tidak valid.\n";
        }

    } while (pilihan != 0);

    return 0;
}
```
 
Output
⁠![Output guided](https://github.com/chafdv/Modul-6/blob/main/Output/guided6.png)

Kode tersebut merupakan program Doubly Linked List dalam bahasa C++ yang berfungsi untuk mengelola data secara dua arah (maju dan mundur). Program ini memungkinkan pengguna menambah, menghapus, memperbarui, dan menampilkan data melalui menu interaktif. Setiap node memiliki pointer ke node sebelumnya dan berikutnya, sehingga data dapat diakses dari kedua arah dengan mudah. Tujuannya adalah untuk mempermudah pengelolaan data secara dinamis menggunakan struktur list dua arah.

---

## Unguided

### Soal 1

Buatlah implementasi ADT Doubly Linked list pada file “Doublylist.cpp” dan coba hasil implementasi ADT pada file “main.cpp”.

```cpp
#ifndef DOUBLYLIST_H
#define DOUBLYLIST_H

#include <iostream>
#include <string>
using namespace std;

struct Kendaraan {
    string nopol;
    string warna;
    int thnBuat;
};

typedef Kendaraan infotype;

struct ElmList {
    infotype info;
    ElmList* next;
    ElmList* prev;
};

typedef ElmList* address;

struct List {
    address First;
    address Last;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertLast(List &L, address P);
address findElm(List L, string nopol);
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(address Prec, address &P);

#endif

```
 ⁠
Output
	⁠![Output Soal 1](https://github.com/chafdv/Modul-6/blob/main/Output/maincpp1.png)

Kode tersebut adalah header file untuk program Doubly Linked List yang menyimpan data kendaraan. Di dalamnya terdapat struktur node dengan pointer ke elemen sebelum dan sesudahnya, serta deklarasi fungsi untuk membuat, menambah, mencari, menampilkan, dan menghapus data pada list.

---

### Soal 2

Carilah elemen dengan nomor polisi D001 dengan membuat fungsi baru.

```cpp
#include "doublylist.h"

void CreateList(List &L) {
    L.First = NULL;
    L.Last = NULL;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = NULL;
    P->prev = NULL;
    return P;
}

void dealokasi(address &P) {
    delete P;
    P = NULL;
}

void printInfo(List L) {
    address P = L.First;
    while (P != NULL) {
        cout << "Nomor Polisi : " << P->info.nopol << endl;
        cout << "Warna        : " << P->info.warna << endl;
        cout << "Tahun        : " << P->info.thnBuat << endl;
        cout << "------------------------------" << endl;
        P = P->next;
    }
}

void insertLast(List &L, address P) {
    if (L.First == NULL) {
        L.First = P;
        L.Last = P;
    } else {
        L.Last->next = P;
        P->prev = L.Last;
        L.Last = P;
    }
}

address findElm(List L, string nopol) {
    address P = L.First;
    while (P != NULL) {
        if (P->info.nopol == nopol) {
            return P;
        }
        P = P->next;
    }
    return NULL;
}

void deleteFirst(List &L, address &P) {
    if (L.First != NULL) {
        P = L.First;
        if (L.First == L.Last) {
            L.First = NULL;
            L.Last = NULL;
        } else {
            L.First = L.First->next;
            L.First->prev = NULL;
        }
        P->next = NULL;
    }
}

void deleteLast(List &L, address &P) {
    if (L.First != NULL) {
        P = L.Last;
        if (L.First == L.Last) {
            L.First = NULL;
            L.Last = NULL;
        } else {
            L.Last = L.Last->prev;
            L.Last->next = NULL;
        }
        P->prev = NULL;
    }
}

void deleteAfter(address Prec, address &P) {
    if (Prec != NULL && Prec->next != NULL) {
        P = Prec->next;
        Prec->next = P->next;
        if (P->next != NULL) {
            P->next->prev = Prec;
        }
        P->next = NULL;
        P->prev = NULL;
    }
}

```

Output 
	⁠![Output Soal 2](https://github.com/chafdv/Modul-6/blob/main/Output/maincpp2.png)

Kode ini berisi fungsi untuk membuat, menambah, menampilkan, mencari, dan menghapus data pada doubly linked list yang berisi informasi kendaraan. Setiap node terhubung dua arah sehingga data bisa diakses dan dihapus dari depan maupun belakang dengan mudah.

---

### Soal 3

Hapus elemen dengan nomor polisi D003 dengan procedure delete.
•⁠  ⁠procedure deleteFirst( input/output L : List,
 P : address )
•⁠  ⁠procedure deleteLast( input/output L : List,
 P : address )
•⁠  ⁠procedure deleteAfter( input Prec : address,
 input/output P : address )

```cpp
#include "DoublyList.h"

int main() {
    List L;
    CreateList(L);

    int n;
    cout << "Masukkan jumlah kendaraan: ";
    cin >> n;
    cin.ignore();

    for (int i = 0; i < n; i++) {
        infotype x;
        cout << "Masukkan nomor polisi: ";
        getline(cin, x.nopol);
        cout << "Masukkan warna kendaraan: ";
        getline(cin, x.warna);
        cout << "Masukkan tahun kendaraan: ";
        cin >> x.thnBuat;
        cin.ignore();
        insertLast(L, alokasi(x));
        cout << endl;
    }

    cout << "\nDATA LIST 1\n";
    printInfo(L);

    string cari;
    cout << "Masukkan Nomor Polisi yang dicari: ";
    getline(cin, cari);
    address found = findElm(L, cari);
    if (found != nullptr) {
        cout << "\nNomor Polisi : " << found->info.nopol << endl;
        cout << "Warna        : " << found->info.warna << endl;
        cout << "Tahun        : " << found->info.thnBuat << endl;
    } else {
        cout << "\nData tidak ditemukan.\n";
    }

    cout << "\nMasukkan Nomor Polisi yang akan dihapus: ";
    string hapus;
    getline(cin, hapus);

    address del = findElm(L, hapus);
    if (del != nullptr) {
        address P;
        if (del == L.First) {
            deleteFirst(L, P);
        } else if (del == L.Last) {
            deleteLast(L, P);
        } else {
            deleteAfter(del->prev, P);
        }
        cout << "Data dengan nomor polisi " << hapus << " berhasil dihapus.\n";
        dealokasi(P);
    } else {
        cout << "Data tidak ditemukan.\n";
    }

    cout << "\nDATA LIST 1 SETELAH HAPUS:\n";
    printInfo(L);

    return 0;
}

```
 
Output 
	⁠![Output Soal 3](https://github.com/chafdv/Modul-6/blob/main/Output/maincpp3.png)

Kode tersebut berfungsi untuk mengelola data kendaraan menggunakan doubly linked list. Program meminta pengguna memasukkan beberapa data kendaraan, lalu menampilkannya. Setelah itu, pengguna dapat mencari data berdasarkan nomor polisi dan menghapus data tersebut baik di awal, tengah, maupun akhir list.

## Referensi

1.⁠ ⁠[(https://www.geeksforgeeks.org/dsa/doubly-linked-list/)] [diakses 27-10-2025]
