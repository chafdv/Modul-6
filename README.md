# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 6 DOUBLY LINKED LIST (BAGIAN PERTAMA) </h1>
<p align="center">Osha Alfida Valyana / 103112430202</p>

## Dasar Teori

Doubly linked list adalah struktur data yang lebih kompleks dibandingkan dengan singly linked list, tetapi memiliki beberapa keunggulan. Keunggulan utamanya adalah memungkinkan penelusuran data secara efisien ke dua arah. Hal ini karena setiap node dalam daftar memiliki pointer ke node sebelumnya dan pointer ke node berikutnya. Dengan begitu, proses penyisipan dan penghapusan node dapat dilakukan dengan cepat dan mudah, serta penelusuran data bisa dilakukan dari kedua arah.

## Guided

### Soal 1

```⁠ cpp
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
⁠![Output Soal 1](https://github.com/salfiayu/Modul-4/blob/main/Modul%204/screenshoot/Screenshot%20(85).png)

Kode tersebut merupakan program **Doubly Linked List** dalam bahasa **C++** yang berfungsi untuk mengelola data secara dua arah (maju dan mundur). Program ini memungkinkan pengguna menambah, menghapus, memperbarui, dan menampilkan data melalui menu interaktif. Setiap node memiliki pointer ke node sebelumnya dan berikutnya, sehingga data dapat diakses dari kedua arah dengan mudah. Tujuannya adalah untuk mempermudah pengelolaan data secara dinamis menggunakan struktur list dua arah.

---

## Unguided

### Soal 1

buatlah single linked list untuk Antrian yang menyimpan data pembeli( nama dan pesanan). program memiliki beberapa menu seperti tambah antrian,  layani antrian(hapus), dan tampilkan antrian. \*antrian pertama harus yang pertama dilayani

⁠ cpp
#include <iostream>
#include <string>
using namespace std;

struct Antrian {
    string nama;
    string pesanan;
    Antrian* next;
};

Antrian* head = nullptr;
Antrian* tail = nullptr;

void tambahAntrian(string nama, string pesanan) {
    Antrian* baru = new Antrian();
    baru->nama = nama;
    baru->pesanan = pesanan;
    baru->next = nullptr;

    if (head == nullptr) {
        head = tail = baru;
    } else {
        tail->next = baru;
        tail = baru;
    }
    cout << "Antrian " << nama << " berhasil ditambahkan.\n";
}

void layaniAntrian() {
    if (head == nullptr) {
        cout << "Antrian kosong.\n";
        return;
    }

    Antrian* temp = head;
    cout << "Melayani antrian: " << temp->nama << " - " << temp->pesanan << endl;
    head = head->next;
    delete temp;

    if (head == nullptr) {
        tail = nullptr;
    }
}

void tampilAntrian() {
    if (head == nullptr) {
        cout << "Antrian kosong.\n";
        return;
    }

    Antrian* temp = head;
    cout << "\n=== DAFTAR ANTRIAN ===\n";
    while (temp != nullptr) {
        cout << temp->nama << " - " << temp->pesanan << endl;
        temp = temp->next;
    }
}

int main() {
    int pilihan;
    string nama, pesanan;

    do {
        cout << "\n=== MENU ANTRIAN ===\n";
        cout << "1. Tambah Antrian\n";
        cout << "2. Layani Antrian\n";
        cout << "3. Tampilkan Antrian\n";
        cout << "0. Keluar\n";
        cout << "Pilih: ";
        cin >> pilihan;
        cin.ignore();

        switch (pilihan) {
            case 1:
                cout << "Masukkan nama    : ";
                getline(cin, nama);
                cout << "Masukkan pesanan : ";
                getline(cin, pesanan);
                tambahAntrian(nama, pesanan);
                break;
            case 2:
                layaniAntrian();
                break;
            case 3:
                tampilAntrian();
                break;
            case 0:
                cout << "Program selesai.\n";
                break;
            default:
                cout << "Pilihan tidak valid!\n";
        }
    } while (pilihan != 0);

    return 0;
}

 ⁠

	⁠Screenshoot  
	⁠![Screenshot Soal 1](https://github.com/salfiayu/Modul-4/blob/main/Modul%204/screenshoot/Screenshot%20(86).png)

Program ini menggunakan singly linked list untuk membuat sistem antrian. Data pembeli masuk ke belakang, dilayani dari depan (FIFO). Tersedia menu untuk tambah, layani (hapus depan), dan tampilkan seluruh antrian.

---

### Soal 2

buatlah program kode untuk membalik (reverse) singly linked list (1-2-3 menjadi 3-2-1) 

⁠ cpp
#include <iostream>
#include "pelajaran.h"
using namespace std;

Pelajaran create_pelajaran(string namaPel, string kodePel) {
    Pelajaran p;
    p.namaMapel = namaPel;
    p.kodeMapel = kodePel;
    return p;
}

void tampil_pelajaran(Pelajaran pel) {
    cout << "nama pelajaran : " << pel.namaMapel << endl;
    cout << "nilai          : " << pel.kodeMapel << endl;
}

 ⁠

	⁠Sreenshoot 
	⁠![Screenshot Soal 2](https://github.com/salfiayu/Modul-4/blob/main/Modul%204/screenshoot/Screenshot%20(87).png)

Program ini membuat singly linked list berisi 1 → 2 → 3, lalu membalik urutannya menjadi 3 → 2 → 1 menggunakan 3 pointer (prev, current, next) untuk memutar arah pointer satu per satu.


## Referensi

1.⁠ ⁠[(https://www.geeksforgeeks.org/dsa/doubly-linked-list/)] [diakses 27-10-2025]
