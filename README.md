# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 6 DOUBLY LINKED LIST (BAGIAN PERTAMA) </h1>
<p align="center">Osha Alfida Valyana / 103112430202</p>

## Dasar Teori

Doubly linked list adalah struktur data yang lebih kompleks dibandingkan dengan singly linked list, tetapi memiliki beberapa keunggulan. Keunggulan utamanya adalah memungkinkan penelusuran data secara efisien ke dua arah. Hal ini karena setiap node dalam daftar memiliki pointer ke node sebelumnya dan pointer ke node berikutnya. Dengan begitu, proses penyisipan dan penghapusan node dapat dilakukan dengan cepat dan mudah, serta penelusuran data bisa dilakukan dari kedua arah.

## Guided

### Soal 1

Linked List

⁠ cpp
#include <iostream>
using namespace std;

// Struktur Node
struct Node {
    int data;
    Node* next;
};

// Pointer head
Node* head = nullptr;

// Fungsi untuk membuat node baru
Node* createNode(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->next = nullptr;
    return newNode;
}

// ========== INSERT DEPAN ==========
void insertDepan(int data) {
    Node* newNode = createNode(data);
    newNode->next = head;
    head = newNode;
    cout << "Data " << data << " berhasil ditambahkan di depan.\n";
}

// ========== INSERT BELAKANG ==========
void insertBelakang(int data) {
    Node* newNode = createNode(data);
    if (head == nullptr) {
        head = newNode;
    } else {
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        temp->next = newNode;
    }
    cout << "Data " << data << " berhasil ditambahkan di belakang.\n";
}

// ========== INSERT SETELAH ==========
void insertSetelah(int target, int dataBaru) {
    Node* temp = head;
    while (temp != nullptr && temp->data != target) {
        temp = temp->next;
    }

    if (temp == nullptr) {
        cout << "Data " << target << " tidak ditemukan!\n";
    } else {
        Node* newNode = createNode(dataBaru);
        newNode->next = temp->next;
        temp->next = newNode;
        cout << "Data " << dataBaru << " berhasil disisipkan setelah " << target << ".\n";
    }
}

// ========== DELETE FUNCTION ==========
void hapusNode(int data) {
    if (head == nullptr) {
        cout << "List kosong!\n";
        return;
    }

    Node* temp = head;
    Node* prev = nullptr;

    // Jika data ada di node pertama
    if (temp != nullptr && temp->data == data) {
        head = temp->next;
        delete temp;
        cout << "Data " << data << " berhasil dihapus.\n";
        return;
    }

    // Cari node yang akan dihapus
    while (temp != nullptr && temp->data != data) {
        prev = temp;
        temp = temp->next;
    }

    // Jika data tidak ditemukan
    if (temp == nullptr) {
        cout << "Data " << data << " tidak ditemukan!\n";
        return;
    }

    prev->next = temp->next;
    delete temp;
    cout << "Data " << data << " berhasil dihapus.\n";
}

// ========== UPDATE FUNCTION ==========
void updateNode(int dataLama, int dataBaru) {
    Node* temp = head;
    while (temp != nullptr && temp->data != dataLama) {
        temp = temp->next;
    }

    if (temp == nullptr) {
        cout << "Data " << dataLama << " tidak ditemukan!\n";
    } else {
        temp->data = dataBaru;
        cout << "Data " << dataLama << " berhasil diupdate menjadi " << dataBaru << ".\n";
    }
}

// ========== DISPLAY FUNCTION ==========
void tampilkanList() {
    if (head == nullptr) {
        cout << "List kosong!\n";
        return;
    }

    Node* temp = head;
    cout << "Isi Linked List: ";
    while (temp != nullptr) {
        cout << temp->data << " -> ";
        temp = temp->next;
    }
    cout << "NULL\n";
}

// ========== MAIN PROGRAM ==========
int main() {
    int pilihan, data, target, dataBaru;

    do {
        cout << "\n=== MENU SINGLE LINKED LIST ===\n";
        cout << "1. Insert Depan\n";
        cout << "2. Insert Belakang\n";
        cout << "3. Insert Setelah\n";
        cout << "4. Hapus Data\n";
        cout << "5. Update Data\n";
        cout << "6. Tampilkan List\n";
        cout << "0. Keluar\n";
        cout << "Pilih: ";
        cin >> pilihan;

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
                cin >> dataBaru;
                insertSetelah(target, dataBaru);
                break;
            case 4:
                cout << "Masukkan data yang ingin dihapus: ";
                cin >> data;
                hapusNode(data);
                break;
            case 5:
                cout << "Masukkan data lama: ";
                cin >> data;
                cout << "Masukkan data baru: ";
                cin >> dataBaru;
                updateNode(data, dataBaru);
                break;
            case 6:
                tampilkanList();
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
	⁠![Screenshot Soal 1](https://github.com/salfiayu/Modul-4/blob/main/Modul%204/screenshoot/Screenshot%20(85).png)


Program ini membuat singly linked list dengan fitur menambah, menghapus, mengubah, dan menampilkan data melalui menu interaktif. Setiap node menyimpan data dan pointer ke node berikutnya.

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
