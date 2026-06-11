#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <set>
#include <iomanip>
#include <algorithm>
#include <limits>

using namespace std;

struct Gejala {
    string kode;
    string deskripsi;
};

struct Penyakit {
    string kode;
    string nama;
    string deskripsi;
    string penanganan;
    vector<string> kodeGejala;
    vector<string> gejalaPenting;
    int minGejala;
};

//Knowledge Base
vector<Gejala> daftarGejala = {
    {"G01", "Demam tinggi (suhu tubuh > 39 derajat C)"},
    {"G02", "Nafsu makan menurun atau tidak mau makan"},
    {"G03", "Tubuh lemas dan lesu"},
    {"G04", "Mulut berbusa / mengeluarkan air liur berlebihan"},
    {"G05", "Luka pada mulut, lidah, atau gusi"},
    {"G06", "Luka pada kaki / celah kuku"},
    {"G07", "Bengkak pada kaki"},
    {"G08", "Pincang / kesulitan berjalan"},
    {"G09", "Diare / mencret"},
    {"G10", "Feses berdarah"},
    {"G11", "Perut kembung"},
    {"G12", "Kesulitan bernapas / napas cepat"},
    {"G13", "Batuk-batuk"},
    {"G14", "Keluar cairan dari hidung"},
    {"G15", "Mata berair / keluar kotoran mata"},
    {"G16", "Kulit gatal / banyak menggaruk"},
    {"G17", "Bulu rontok / kulit bersisik"},
    {"G18", "Terdapat bintik atau bercak pada kulit"},
    {"G19", "Benjolan atau pembengkakan pada tubuh"},
    {"G20", "Penurunan produksi susu"},
    {"G21", "Ambing (payudara) bengkak dan kemerahan"},
    {"G22", "Susu berubah warna / bernanah"},
    {"G23", "Kejang-kejang"},
    {"G24", "Berputar-putar / kehilangan keseimbangan"},
    {"G25", "Perubahan perilaku (gelisah atau tidak responsif)"},
    {"G26", "Feses berwarna hitam / gelap"},
    {"G27", "Pembengkakan di daerah leher / rahang"},
    {"G28", "Keguguran / aborsi spontan"},
    {"G29", "Badan kurus meski makan banyak"},
    {"G30", "Terdapat cacing pada feses"},
};

vector<Penyakit> daftarPenyakit = {
    {
        "P01",
        "Penyakit Mulut dan Kuku (PMK)",
        "Penyakit sangat menular yang menyerang sapi dan hewan berkuku belah lainnya.",
        "Isolasi hewan yang terinfeksi. Berikan antibiotik untuk mencegah infeksi sekunder. "
        "Lakukan desinfeksi kandang. Hubungi dokter hewan segera. Vaksinasi rutin untuk pencegahan.",
        {"G01","G02","G03","G04","G05","G06","G07","G08"},
        {"G04","G05","G06"},
        3
    },
    {
        "P02",
        "Diare / Enteritis Bakteri",
        "Peradangan saluran cerna akibat infeksi bakteri seperti E. coli atau Salmonella.",
        "Berikan terapi cairan (rehidrasi oral atau infus). Antibiotik sesuai anjuran dokter hewan. "
        "Isolasi hewan sakit. Jaga kebersihan kandang dan pakan.",
        {"G02","G03","G09","G10","G26","G01"},
        {"G09"},
        2
    },
    {
        "P03",
        "Kembung (Bloat / Timpani)",
        "Akumulasi gas berlebihan di dalam rumen akibat fermentasi pakan yang terlalu cepat.",
        "Posisikan sapi berdiri dengan kepala lebih tinggi. Berikan obat anti-bloat (Poloxalene). "
        "Jika parah, lakukan trokarisasi (tusuk rumen) oleh dokter hewan. Batasi pakan hijauan basah.",
        {"G11","G03","G12","G02"},
        {"G11"},
        2
    },
    {
        "P04",
        "Pneumonia (Radang Paru-Paru)",
        "Infeksi bakteri atau virus pada paru-paru, sering terjadi saat cuaca dingin atau lembap.",
        "Berikan antibiotik (Procaine Penicillin atau Oxytetracycline). Jaga sapi tetap hangat dan kering. "
        "Konsultasi dokter hewan. Vaksinasi IBR dan BRSV untuk pencegahan.",
        {"G01","G12","G13","G14","G03","G02","G15"},
        {"G12","G13"},
        3
    },
    {
        "P05",
        "Mastitis (Radang Ambing)",
        "Infeksi bakteri pada kelenjar susu yang umum terjadi pada sapi perah.",
        "Terapi antibiotik intramammary. Lakukan pemerahan lebih sering. "
        "Jaga kebersihan kandang dan ambing. Konsultasi dokter hewan untuk kultur bakteri.",
        {"G20","G21","G22","G01","G03"},
        {"G21","G22"},
        2
    },
    {
        "P06",
        "Scabies / Kudis (Mange)",
        "Penyakit kulit akibat tungau Sarcoptes scabiei yang menyebabkan gatal hebat.",
        "Berikan obat antiparasit (Ivermectin injeksi atau pour-on). "
        "Mandikan sapi dengan acarisida. Desinfeksi kandang. Isolasi hewan yang terinfeksi.",
        {"G16","G17","G18","G03","G02"},
        {"G16","G17"},
        2
    },
    {
        "P07",
        "Cacingan (Helminthiasis)",
        "Infeksi cacing parasit pada saluran pencernaan sapi.",
        "Berikan obat cacing (Albendazole, Fenbendazole, atau Ivermectin). "
        "Rotasi padang penggembalaan. Jaga kebersihan pakan dan air minum.",
        {"G29","G02","G09","G30","G03"},
        {"G29","G30"},
        2
    },
    {
        "P08",
        "Anthrax (Penyakit Limpa)",
        "Penyakit bakterial sangat berbahaya akibat Bacillus anthracis. ZOONOSIS - dapat menular ke manusia.",
        "SEGERA HUBUNGI DOKTER HEWAN DAN DINAS PETERNAKAN. Jangan membuka bangkai. "
        "Isolasi ketat seluruh kawasan. Antibiotik Penicillin dosis tinggi. Vaksinasi darurat kawasan.",
        {"G01","G03","G19","G27","G12","G23"},
        {"G19","G27"},
        3
    },
    {
        "P09",
        "Brucellosis",
        "Infeksi bakteri Brucella abortus yang menyebabkan keguguran dan infertilitas. ZOONOSIS.",
        "Tidak ada pengobatan efektif pada sapi. Sapi positif Brucella harus diafkir (culling). "
        "Laporkan ke dinas peternakan. Vaksinasi strain 19 pada betina muda untuk pencegahan.",
        {"G28","G20","G03","G02"},
        {"G28"},
        2
    },
    {
        "P10",
        "Pink Eye (Keratokonjungtivitis Infeksiosa)",
        "Infeksi bakteri Moraxella bovis pada mata yang menyebabkan peradangan kornea.",
        "Berikan antibiotik tetes/salep mata (Oxytetracycline). Suntikan antibiotik sistemik. "
        "Lindungi sapi dari sinar matahari langsung dan lalat. Isolasi hewan terinfeksi.",
        {"G15","G03","G01"},
        {"G15"},
        1
    },
    {
        "P11",
        "Lumpy Skin Disease (LSD)",
        "Penyakit viral yang ditandai benjolan pada kulit, ditularkan oleh serangga.",
        "Tidak ada pengobatan antivirus khusus. Berikan antibiotik untuk mencegah infeksi sekunder. "
        "Gunakan insektisida untuk mengendalikan vektor serangga. Vaksinasi rutin.",
        {"G18","G19","G01","G02","G03","G20"},
        {"G18","G19"},
        3
    },
    {
        "P12",
        "Tetanus",
        "Penyakit akibat toksin bakteri Clostridium tetani yang masuk melalui luka.",
        "Berikan antitoksin tetanus segera. Antibiotik Penicillin. Tempatkan di ruang gelap dan tenang. "
        "Infus untuk nutrisi. Pencegahan dengan vaksinasi Clostridial.",
        {"G23","G24","G25","G03"},
        {"G23","G24"},
        2
    },
    {
        "P13",
        "Demam Tiga Hari (Ephemeral Fever)",
        "Penyakit viral yang ditularkan serangga, biasanya sembuh sendiri dalam 3 hari.",
        "Berikan anti-inflamasi (NSAID). Terapi cairan jika diperlukan. Istirahatkan sapi. "
        "Hindari pemaksaan kerja saat sakit. Kendalikan populasi nyamuk dan lalat.",
        {"G01","G03","G08","G12","G14","G02"},
        {"G01","G03"},
        3
    },
};


void cetakGaris(char karakter = '=', int panjang = 60) {
    cout << string(panjang, karakter) << endl;
}

void cetakJudul(const string& judul) {
    cetakGaris();
    int spasi = (60 - judul.size()) / 2;
    cout << string(spasi, ' ') << judul << endl;
    cetakGaris();
}

void bersihkanLayar() {
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

void jeda() {
    cout << "\nTekan Enter untuk melanjutkan...";
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    cin.get();
}

struct HasilDiagnosa {
    Penyakit penyakit;
    int jumlahCocok;
    double persentase;
    vector<string> gejalaCocok;
};

//Inference Engine
vector<HasilDiagnosa> forwardChaining(const set<string>& gejalaPasien) {
    vector<HasilDiagnosa> hasilList;

    for (const auto& penyakit : daftarPenyakit) {
        vector<string> cocok;
        bool adaGejalaPenting = false;

        for (const auto& kg : penyakit.kodeGejala) {
            if (gejalaPasien.count(kg)) {
                cocok.push_back(kg);
            }
        }

        for (const auto& gp : penyakit.gejalaPenting) {
            if (gejalaPasien.count(gp)) {
                adaGejalaPenting = true;
                break;
            }
        }
        
        if (adaGejalaPenting && (int)cocok.size() >= penyakit.minGejala) {
            double persen = ((double)cocok.size() / penyakit.kodeGejala.size()) * 100.0;
            hasilList.push_back({penyakit, (int)cocok.size(), persen, cocok});
        }
    }

    sort(hasilList.begin(), hasilList.end(), [](const HasilDiagnosa& a, const HasilDiagnosa& b) {
        return a.persentase > b.persentase;
    });

    return hasilList;
}

//User Interface
void tampilkanMenuUtama() {
    bersihkanLayar();
    cetakJudul("SISTEM PAKAR PENYAKIT SAPI TERNAK");
    cout << "  Metode  : Forward Chaining" << endl;
    cout << "  Basis   : " << daftarPenyakit.size() << " Penyakit | "
         << daftarGejala.size() << " Gejala" << endl;
    cetakGaris('-');
    cout << "  [1] Mulai Konsultasi / Diagnosa" << endl;
    cout << "  [2] Lihat Daftar Penyakit" << endl;
    cout << "  [3] Lihat Daftar Gejala" << endl;
    cout << "  [4] Tentang Sistem" << endl;
    cout << "  [0] Keluar" << endl;
    cetakGaris();
    cout << "  Pilihan Anda: ";
}

void tampilkanDaftarGejala() {
    bersihkanLayar();
    cetakJudul("DAFTAR GEJALA");
    for (const auto& g : daftarGejala) {
        cout << "  " << g.kode << " : " << g.deskripsi << endl;
    }
    jeda();
}

void tampilkanDaftarPenyakit() {
    bersihkanLayar();
    cetakJudul("DAFTAR PENYAKIT");
    for (size_t i = 0; i < daftarPenyakit.size(); i++) {
        cout << "  " << setw(2) << (i+1) << ". [" << daftarPenyakit[i].kode << "] "
             << daftarPenyakit[i].nama << endl;
        cout << "      Gejala terkait: " << daftarPenyakit[i].kodeGejala.size()
             << " | Min. diagnosa: " << daftarPenyakit[i].minGejala << " gejala" << endl;
    }
    jeda();
}

void tampilkanTentangSistem() {
    bersihkanLayar();
    cetakJudul("TENTANG SISTEM");
    cout << endl;
    cout << "  Nama Sistem : Sistem Pakar Identifikasi Penyakit Sapi Ternak" << endl;
    cout << "  Metode      : Forward Chaining" << endl;
    cout << "  Bahasa      : C++" << endl;
    cout << "  Basis Data  : " << daftarPenyakit.size() << " jenis penyakit" << endl;
    cout << "                " << daftarGejala.size() << " jenis gejala" << endl;
    cout << endl;
    cetakGaris('-');
    cout << "  CARA KERJA FORWARD CHAINING:" << endl;
    cetakGaris('-');
    cout << "  1. Pengguna memasukkan gejala yang dialami sapi" << endl;
    cout << "  2. Sistem mencocokkan fakta (gejala) dengan basis" << endl;
    cout << "     pengetahuan (aturan penyakit)" << endl;
    cout << "  3. Jika gejala penting terpenuhi DAN jumlah cocok" << endl;
    cout << "     >= minimum, maka penyakit didiagnosa positif" << endl;
    cout << "  4. Sistem menampilkan hasil dengan persentase" << endl;
    cout << "     kecocokan dari tertinggi ke terendah" << endl;
    cout << endl;
    cetakGaris('-');
    cout << "  DISCLAIMER:" << endl;
    cetakGaris('-');
    cout << "  Hasil diagnosa bersifat PENDUKUNG KEPUTUSAN." << endl;
    cout << "  Selalu konsultasikan dengan dokter hewan untuk" << endl;
    cout << "  penanganan yang tepat." << endl;
    cout << endl;
    jeda();
}

void tampilkanHasilDiagnosa(const vector<HasilDiagnosa>& hasil,
                             const set<string>& gejalaPasien) {
    bersihkanLayar();
    cetakJudul("HASIL DIAGNOSA");

    cout << "  Gejala yang diinput: " << gejalaPasien.size() << " gejala" << endl;
    cetakGaris('-');

    if (hasil.empty()) {
        cout << endl;
        cout << "  [!] Tidak ditemukan penyakit yang cocok." << endl;
        cout << "  Kemungkinan gejala tidak cukup atau sapi dalam" << endl;
        cout << "  kondisi sehat. Konsultasikan ke dokter hewan." << endl;
        cout << endl;
    } else {
        cout << "  Ditemukan " << hasil.size() << " kemungkinan penyakit:" << endl;
        cout << endl;

        for (size_t i = 0; i < hasil.size(); i++) {
            const auto& h = hasil[i];
            cout << "  +" << string(56, '-') << "+" << endl;
            cout << "  | #" << (i+1) << " " << left << setw(51)
                 << h.penyakit.nama << "|" << endl;
            cout << "  +" << string(56, '-') << "+" << endl;
            cout << "  | Kode          : " << h.penyakit.kode << endl;
            cout << "  | Kecocokan     : " << h.jumlahCocok << "/"
                 << h.penyakit.kodeGejala.size() << " gejala ("
                 << fixed << setprecision(1) << h.persentase << "%)" << endl;


            int bar = (int)(h.persentase / 5);
            cout << "  | Progress      : [" << string(bar, '#')
                 << string(20-bar, '.') << "] " << endl;

            cout << "  | Gejala cocok  : ";
            for (size_t j = 0; j < h.gejalaCocok.size(); j++) {
                if (j > 0) cout << ", ";
                cout << h.gejalaCocok[j];
            }
            cout << endl;

            cout << "  | Deskripsi     : ";
            string desc = h.penyakit.deskripsi;
            if (desc.size() > 40) {
                cout << desc.substr(0, 40) << endl;
                cout << "  |               " << desc.substr(40) << endl;
            } else {
                cout << desc << endl;
            }

            cout << "  | Penanganan    : ";
            string pen = h.penyakit.penanganan;
            int batas = 40;
            while ((int)pen.size() > batas) {
                int potong = batas;
                while (potong > 0 && pen[potong] != ' ') potong--;
                if (potong == 0) potong = batas;
                cout << pen.substr(0, potong) << endl;
                cout << "  |               ";
                pen = pen.substr(potong + 1);
            }
            cout << pen << endl;
            cout << endl;
        }
    }
    jeda();
}

void mulaiKonsultasi() {
    bersihkanLayar();
    cetakJudul("KONSULTASI DIAGNOSA PENYAKIT SAPI");
    cout << endl;
    cout << "  Silakan jawab pertanyaan berikut dengan:" << endl;
    cout << "  [Y] = Ya / Ada gejala tersebut" << endl;
    cout << "  [T] = Tidak / Tidak ada gejala" << endl;
    cout << endl;
    cetakGaris('-');

    set<string> gejalaPasien;
    int nomorSapi;

    cout << "  Nomor/ID Sapi yang diperiksa: ";
    cin >> nomorSapi;
    cin.ignore();

    cout << endl;
    cout << "  Mulai pemeriksaan gejala..." << endl;
    cetakGaris('-');
    cout << endl;

    for (const auto& g : daftarGejala) {
        cout << "  [" << g.kode << "] " << g.deskripsi << endl;
        cout << "  Jawaban (Y/T): ";
        char jawaban;
        cin >> jawaban;
        cin.ignore();
        if (toupper(jawaban) == 'Y') {
            gejalaPasien.insert(g.kode);
        }
        cout << endl;
    }

    vector<HasilDiagnosa> hasil = forwardChaining(gejalaPasien);

    tampilkanHasilDiagnosa(hasil, gejalaPasien);
}

void konsultasiCepat() {
    bersihkanLayar();
    cetakJudul("KONSULTASI CEPAT (INPUT KODE GEJALA)");
    cout << endl;
    cout << "  Mode ini memungkinkan Anda langsung memasukkan" << endl;
    cout << "  kode gejala tanpa menjawab satu per satu." << endl;
    cout << endl;
    cout << "  Ketik kode gejala (contoh: G01 G03 G09)" << endl;
    cout << "  Ketik 'LIHAT' untuk melihat daftar gejala" << endl;
    cout << "  Ketik 'SELESAI' atau 'DONE' untuk proses diagnosa" << endl;
    cout << endl;
    cetakGaris('-');

    set<string> gejalaPasien;
    string input;

    while (true) {
        cout << "  > ";
        cin >> input;

        for (auto& c : input) c = toupper(c);

        if (input == "SELESAI" || input == "DONE") break;

        if (input == "LIHAT") {
            cout << endl;
            for (const auto& g : daftarGejala) {
                cout << "    " << g.kode << " : " << g.deskripsi << endl;
            }
            cout << endl;
            continue;
        }

        bool valid = false;
        for (const auto& g : daftarGejala) {
            if (g.kode == input) {
                valid = true;
                gejalaPasien.insert(input);
                cout << "  [+] Ditambahkan: " << g.deskripsi << endl;
                break;
            }
        }
        if (!valid) {
            cout << "  [!] Kode tidak ditemukan. Coba lagi." << endl;
        }
    }

    if (gejalaPasien.empty()) {
        cout << endl << "  Tidak ada gejala yang diinput." << endl;
        jeda();
        return;
    }

    vector<HasilDiagnosa> hasil = forwardChaining(gejalaPasien);
    tampilkanHasilDiagnosa(hasil, gejalaPasien);
}


void menuKonsultasi() {
    bersihkanLayar();
    cetakJudul("PILIHAN KONSULTASI");
    cout << "  [1] Konsultasi Lengkap (jawab satu per satu)" << endl;
    cout << "  [2] Konsultasi Cepat (input kode gejala langsung)" << endl;
    cout << "  [0] Kembali" << endl;
    cetakGaris();
    cout << "  Pilihan: ";
    int pilihan;
    cin >> pilihan;
    cin.ignore();
    if (pilihan == 1) mulaiKonsultasi();
    else if (pilihan == 2) konsultasiCepat();
}


int main() {
    int pilihan;

    while (true) {
        tampilkanMenuUtama();
        cin >> pilihan;
        cin.ignore();

        switch (pilihan) {
            case 1: menuKonsultasi();          break;
            case 2: tampilkanDaftarPenyakit(); break;
            case 3: tampilkanDaftarGejala();   break;
            case 4: tampilkanTentangSistem();  break;
            case 0:
                bersihkanLayar();
                cetakJudul("TERIMA KASIH");
                cout << "  Sistem Pakar Penyakit Sapi Ternak" << endl;
                cout << "  Program selesai." << endl;
                cetakGaris();
                return 0;
            default:
                cout << "  [!] Pilihan tidak valid!" << endl;
                break;
        }
    }

    return 0;
}
