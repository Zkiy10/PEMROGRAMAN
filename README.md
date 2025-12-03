# PEMROGRAMAN
#include <iostream>
#include <cctype>
#include <algorithm>
#include <string>
using namespace std;

struct Mahasiswa {
    string nama;
    string nim;
    string tahunmasuk;
    string niu;
    string kodefakultas;
    string nif;
    string prodi;
    float tugas, uts, uas, rata;
};

Mahasiswa dataMhs[100];
int jumlaharr = 0;

void mainmenu();
void insertionSort();
void datamahasiswa();
void tampilkandata();
void BinarySearch(int jumlaharr, string target);
void sortRata();
void nilaiMaksMin();

// Fungsi hitung rata-rata
float hitungRata(float tugas, float uts, float uas) {
    return (tugas + uts + uas) / 3;
}

// Fungsi validasi nilai (tidak boleh negatif, maksimal 100)
float cekNilai(string jenisNilai) {
    float nilai;
    do {
        cout << "Nilai " << jenisNilai << " (0-100): ";
        cin >> nilai;
        
        if (nilai < 0 || nilai > 100) {
            cout << "Nilai harus antara 0 dan 100! Ulangi." << endl;
        }
    } while (nilai < 0 || nilai > 100);
    return nilai;
}

string cekNIU() {
    string niuInput;
    do {
        cout << "NIM (NIU, harus 6 digit angka): ";
        cin >> niuInput;

        bool panjangOK = (niuInput.length() == 6);
        bool angkaSemua = all_of(niuInput.begin(), niuInput.end(), ::isdigit);

        if (!panjangOK || !angkaSemua) {
            cout << "NIU harus 6 digit dan semuanya angka! Ulangi." << endl;
        }
    } while (niuInput.length() != 6 || !all_of(niuInput.begin(), niuInput.end(), ::isdigit));
    return niuInput;
}

void datamahasiswa() {
    int jumlah;
    cout << "Jumlah data mahasiswa yang ingin dimasukkan: ";
    cin >> jumlah;
    
    for (int i = 0; i < jumlah; i++) {
        cout << "Data mahasiswa ke-" << i + 1 << endl;
        cout << "Nama : ";
        cin >> dataMhs[jumlaharr].nama;
        cout << "NIM (Tahun Masuk): ";
        cin >> dataMhs[jumlaharr].tahunmasuk;
        
        dataMhs[jumlaharr].niu = cekNIU();
        
        cout << "NIM (Kode Fakultas) : ";
        cin >> dataMhs[jumlaharr].kodefakultas;
        cout << "NIM (Nomor Induk Fakultas): ";
        cin >> dataMhs[jumlaharr].nif;
        cout << "Prodi : ";
        cin >> dataMhs[jumlaharr].prodi;
        
        // Gunakan fungsi cekNilai untuk validasi
        dataMhs[jumlaharr].tugas = cekNilai("Tugas");
        dataMhs[jumlaharr].uts = cekNilai("UTS");
        dataMhs[jumlaharr].uas = cekNilai("UAS");
        
        // Hitung rata-rata
        dataMhs[jumlaharr].rata = hitungRata(dataMhs[jumlaharr].tugas, dataMhs[jumlaharr].uts, dataMhs[jumlaharr].uas);
        
        jumlaharr++;
    }
    insertionSort();
    
    string s = "/";
    for (int i = 0; i < jumlaharr; i++) {
        dataMhs[i].nim = dataMhs[i].tahunmasuk + s + dataMhs[i].niu + s + dataMhs[i].kodefakultas + s + dataMhs[i].nif;
    }
    mainmenu();
}

void insertionSort() {
    for (int i = 1; i < jumlaharr; i++) {
        Mahasiswa key = dataMhs[i];
        int j = i - 1;

        while (j >= 0 && dataMhs[j].niu > key.niu) {
            dataMhs[j + 1] = dataMhs[j];
            j--;
        }
        dataMhs[j + 1] = key;
    }
}

void tampilkandata() {
    if (jumlaharr == 0) {
        cout << "Belum ada data mahasiswa yang tersimpan." << endl;
        mainmenu();
        return;
    }  
        
    for (int i = 0; i < jumlaharr; i++) {
        cout << "Mahasiswa ke-" << i + 1 << endl;
        cout << "Nama   : " << dataMhs[i].nama << endl;
        cout << "NIM    : " << dataMhs[i].nim << endl;
        cout << "Prodi  : " << dataMhs[i].prodi << endl;
        cout << "Tugas  : " << dataMhs[i].tugas << endl;
        cout << "UTS    : " << dataMhs[i].uts << endl;
        cout << "UAS    : " << dataMhs[i].uas << endl;
        cout << "Rata-rata : " << dataMhs[i].rata << endl << endl;
    }
    mainmenu();
}

void BinarySearch(int jumlaharr, string target) {
    int left = 0;
    int right = jumlaharr - 1;
    bool found = false;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (dataMhs[mid].niu == target) {
            cout << "Data mahasiswa ditemukan pada urutan ke-" << mid + 1 << endl;
            cout << "Data Mahasiswa" << endl;
            cout << "Nama  : " << dataMhs[mid].nama << endl;
            cout << "NIM   : " << dataMhs[mid].nim << endl;
            cout << "Prodi : " << dataMhs[mid].prodi << endl;
            cout << "Tugas : " << dataMhs[mid].tugas << endl;
            cout << "UTS   : " << dataMhs[mid].uts << endl;
            cout << "UAS   : " << dataMhs[mid].uas << endl;
            cout << "Rata-rata : " << dataMhs[mid].rata << endl << endl;
            found = true;
            break;
        } else if (dataMhs[mid].niu < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    if (!found) {
        cout << "Data mahasiswa dengan NIU " << target << " tidak ditemukan." << endl;
    }
    mainmenu();
}

void sortRata() {
    for (int i = 0; i < jumlaharr - 1; i++) {
        for (int j = 0; j < jumlaharr - i - 1; j++) {
            if (dataMhs[j].rata < dataMhs[j + 1].rata) {
                swap(dataMhs[j], dataMhs[j + 1]);
            }
        }
    }
    cout << "\nData berhasil diurutkan berdasarkan rata-rata nilai (descending).\n";
    mainmenu();
}

void nilaiMaksMin() {
    if (jumlaharr == 0) {
        cout << "Belum ada data mahasiswa." << endl;
        mainmenu();
        return;
    }
    
    int idxMax = 0, idxMin = 0;
    for (int i = 1; i < jumlaharr; i++) {
        if (dataMhs[i].rata > dataMhs[idxMax].rata) idxMax = i;
        if (dataMhs[i].rata < dataMhs[idxMin].rata) idxMin = i;
    }

    cout << "\n=== NILAI TERTINGGI ===\n";
    cout << "Nama  : " << dataMhs[idxMax].nama << endl;
    cout << "Rata  : " << dataMhs[idxMax].rata << endl;

    cout << "\n=== NILAI TERENDAH ===\n";
    cout << "Nama  : " << dataMhs[idxMin].nama << endl;
    cout << "Rata  : " << dataMhs[idxMin].rata << endl;
    mainmenu();
}

void mainmenu() {
    char inputmainmenu;
    cout << endl;
    cout << "==MAIN MENU==" << endl;
    cout << "1. Input data mahasiswa" << endl;
    cout << "2. Tampilkan semua data mahasiswa (urut berdasarkan NIU)" << endl;
    cout << "3. Search data mahasiswa (berdasarkan NIU)" << endl;
    cout << "4. Sorting berdasarkan rata-rata nilai" << endl;
    cout << "5. Tampilkan mahasiswa nilai tertinggi & terendah" << endl;
    cout << "0. Keluar" << endl;
    cout << "Masukkan pilihan : ";
    cin >> inputmainmenu;

    switch (inputmainmenu) {
        case '1': {
            datamahasiswa();
            break;
        }
        case '2': {
            tampilkandata();
            break;
        }
        case '3': {
            string niuInput = cekNIU();  
            BinarySearch(jumlaharr, niuInput);
            break;
        }
        case '4': {
            sortRata();
            break;
        }
        case '5': {
            nilaiMaksMin();
            break;
        }
        case '0':
            cout << "Keluar..." << endl;
            return;
        default:
            cout << "Input salah." << endl;
            mainmenu();
            break;
    }
    cout << endl;
}

int main() {
    mainmenu();
    return 0;
}
