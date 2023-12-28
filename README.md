#define _CRT_SECURE_NO_WARNINGS
# include <iostream>
#include<string>
# include <fstream>
using namespace std;

class Detalii {
public:
	virtual string detalii() = 0;
};

class Entitate {
public: 
	virtual string tipEntitate() = 0;
};

class Mobila :public Detalii, public Entitate {
	const int id;
	char* culoare;
	int nrSertare;
	string material;
	static int garantie;
public:

	string detalii() {
		return "Mobila este una de calitate, venita din Elvetia";
	}
	string tipEntitate() {
		return "Acesta este o mobila";
	}

	Mobila() : id(105)
	{
		this->nrSertare = 4;
		this->culoare = new char[strlen("alba") + 1];
		strcpy_s(this->culoare, strlen("alba") + 1, "alba");
		this->material = "lemn";
	}

	Mobila(int nrSertare, string material) :id(105)
	{
		this->nrSertare = nrSertare;
		this->culoare = new char[strlen("neagra") + 1];
		strcpy_s(this->culoare, strlen("neagra") + 1, "neagra");
		this->material = material;

	}

	Mobila(int id, const char* culoare, int nrSertare, string material) :id(id)
	{
		this->culoare = new char[strlen(culoare) + 1];
		strcpy_s(this->culoare, strlen(culoare) + 1, culoare);
		this->nrSertare = nrSertare;
		this->material = material;
	}

	void afisare() {
		cout << "Mobila cu id-ul " << id << " are " << nrSertare << " sertare, este de culoare  " << culoare << " si este din " << material << ". Mobila este in garantie pentru " << garantie << " ani" << endl;

	}


	Mobila(const Mobila& m) :id(m.id) {

		this->culoare = new char[strlen(m.culoare) + 1];
		strcpy_s(this->culoare, strlen(m.culoare) + 1, m.culoare);
		this->nrSertare = m.nrSertare;
		this->material = m.material;
	}

	~Mobila() {
		if (this->culoare != NULL) {
			delete[]this->culoare;
		}
	}
	virtual string detaliiSuplimentare() {
		return "Nu se poate preciza nimic";
	}
	Mobila& operator=(const Mobila& m) {
		if (this != &m) {
			this->nrSertare = m.nrSertare;
			if (this->culoare)
				delete[] this->culoare;
			this->culoare = new char[strlen(m.culoare) + 1];
			strcpy_s(this->culoare, strlen(m.culoare) + 1, m.culoare);
			this->material = m.material;
		}
		return *this;
	}


	int getId() {
		return id;
	}

	int getNrSertare() {
		return nrSertare;
	}

	void setNrSertare(int nrSertare) {
		this->nrSertare = nrSertare;
	}

	const char* getCuloare() {
		return culoare;
	}

	void setCuloare(const char* culoare) {
		if (this->culoare) {
			delete[] this->culoare;
		}
		this->culoare = new char[strlen(culoare) + 1];
		strcpy(this->culoare, culoare);
	}

	string getMaterial() {
		return material;
	}

	void setMaterial(const string& material) {
		this->material = material;
	}

	static int getGarantie() {
		return garantie;
	}

	static void setGarantie(int garantie) {
		Mobila::garantie = garantie;
	}

	friend void emitereFactura(Mobila m);

	friend ostream& operator<<(ostream& os, const Mobila& mobila) {
		os << "Mobila cu id-ul " << mobila.id << " are " << mobila.nrSertare << " sertare, este de culoare " << mobila.culoare << " si este din " << mobila.material << ". Mobila este in garantie pentru " << Mobila::garantie << " ani" << endl;
		return os;
	}




	friend istream& operator>>(istream& input, Mobila& mobila) {
		cout << "Introduceti culoarea: ";
		char buffer[100];
		input >> buffer;
		delete[] mobila.culoare;
		mobila.culoare = new char[strlen(buffer) + 1];
		strcpy(mobila.culoare, buffer);
		cout << "Introduceti numarul de sertare: ";
		input >> mobila.nrSertare;
		cout << "Introduceti materialul: ";
		input >> mobila.material;
		return input;
	}

	void serializare(string MobilaFisier) {
		ofstream f(MobilaFisier, ios::out | ios::binary);
		//pt char*
		int dimCuloare = strlen(this->culoare);
		f.write((char*)&dimCuloare, sizeof(dimCuloare));
		f.write(this->culoare, dimCuloare + 1);

		// pt numerice
		f.write((char*)&this->nrSertare, sizeof(this->nrSertare));

		//pt string
		int dimMaterial = this->material.size();
		f.write((char*)&dimMaterial, sizeof(dimMaterial));
		f.write(this->material.c_str(), dimMaterial + 1);
		f.close();
	}
	void deserializare(string MobilaFisier) {
		ifstream f(MobilaFisier, ios::in | ios::binary);
		if (f.is_open())
		{
			if (this->culoare != NULL) {
				delete[]this->culoare;
			}
	

			//pt char*
			int dimCuloare = 0;
			f.read((char*)&dimCuloare, sizeof(dimCuloare));
			this->culoare = new char[dimCuloare + 1];
			f.read(this->culoare, dimCuloare + 1);

			// pt numerice
			f.read((char*)&this->nrSertare, sizeof(this->nrSertare));
			
			//pt string
			int dimMaterial = 0;
			f.read((char*)&dimMaterial, sizeof(dimMaterial));
			char* aux = new char[dimMaterial + 1];
			f.read(aux, dimMaterial + 1);//+1 este pentru terminatorul de sir
			this->material = aux;
			delete[]aux;
			f.close();

		}
		else {
			cout << "Fisierul cautat nu exista!" << endl;
		}
	}

	bool operator&&(const Mobila& m) {
		return this->nrSertare == m.nrSertare && this->material == m.material;
	}

	Mobila& operator +=(int nrSertare) {
		this->nrSertare += nrSertare;
		return *this;
	}

	int operator()() {
		return garantie;
	}

	Mobila& operator+=(const Mobila& m) {
		this->nrSertare += m.nrSertare;
		return *this;
	}
};
int Mobila::garantie = 5;
void emitereFactura(Mobila m) {
	cout << "****************************************" << endl;
	cout << "            FACTURA MOBILA             " << endl;
	cout << "****************************************" << endl;
	cout << "ID Mobila: " << m.id << endl;
	cout << "Nr. Sertare: " << m.nrSertare << endl;
	cout << "Culoare: " << m.culoare << endl;
	cout << "Material: " << m.material << endl;
	cout << "Garantie: " << Mobila::garantie << " ani" << endl;
	cout << "****************************************" << endl;
}

class Angajat: public Detalii, public Entitate {
	const int id;
	char* nume;
	int varsta;
	int aniVechime;
	bool eBarbat;
	static int varstaPensionare;
public:

	string detalii() {
		return "Angajatul este unul muncitor";
	}
	string tipEntitate() {
		return "Acesta este un angajat";
	}


	Angajat() :id(223)
	{
		this->nume = new char[strlen("Popescu") + 1];
		strcpy_s(this->nume, strlen("Popescu") + 1, "Popescu");
		this->varsta = 38;
		this->aniVechime = 15;
		this->eBarbat = true;
	}

	Angajat(const char* nume, int varsta) : id(278)
	{
		this->nume = new char[strlen(nume) + 1];
		strcpy(this->nume, nume);
		this->varsta = varsta;
		this->aniVechime = 10;
		this->eBarbat = true;

	}
	Angajat(int id, int aniVechime) :id(id)
	{
		this->nume = new char[strlen("Angajatul X") + 1];
		strcpy_s(this->nume, strlen("Angajatul X") + 1, "Angajatul X");
		this->varsta = 55;
		this->aniVechime = aniVechime;
		this->eBarbat = true;

	}
	Angajat(int id, const char* nume, int varsta, int aniVechime, bool eBarbat) :id(id)
	{
		this->nume = new char[strlen(nume) + 1];
		strcpy(this->nume, nume);
		this->varsta = varsta;
		this->aniVechime = aniVechime;
		this->eBarbat = eBarbat;

	}


	

	Angajat(const Angajat& a) :id(a.id) {
		this->nume = new char[strlen(a.nume) + 1];
		strcpy_s(this->nume, strlen(a.nume) + 1, a.nume);
		this->varsta = a.varsta;
		this->aniVechime = a.aniVechime;
		this->eBarbat = a.eBarbat;
	}

	~Angajat() {
		if (this->nume != NULL) {
			delete[]this->nume;
		}
	}

	Angajat& operator=(const Angajat& a) {
		if (this != &a)
		{
			if (this->nume != NULL) {
				delete[]this->nume;
			}
			this->nume = new char[strlen(a.nume) + 1];
			strcpy_s(this->nume, strlen(a.nume) + 1, a.nume);
			this->varsta = a.varsta;
			this->aniVechime = a.aniVechime;
			this->eBarbat = a.eBarbat;
		}
		return *this;
	}

	const int getId() {
		return this->id;
	}

	char* getNume() {
		return nume;
	}

	void setNume(const char* nume) {
		if (this->nume) {
			delete[] this->nume;
		}
		this->nume = new char[strlen(nume) + 1];
		strcpy(this->nume, nume);
	}

	int getVarsta() {
		return varsta;
	}

	void setVarsta(int varsta) {
		this->varsta = varsta;
	}

	int getAniVechime() {
		return aniVechime;
	}

	void setAniVechime(int aniVechime) {
		this->aniVechime = aniVechime;
	}

	bool isBarbat() {
		return eBarbat;
	}

	void setBarbat(bool eBarbat) {
		this->eBarbat = eBarbat;
	}

	static int getVarstaPensionare() {
		return varstaPensionare;
	}

	static void setVarstaPensionare(int varstaPensionare) {
		Angajat::varstaPensionare = varstaPensionare;
	}

	explicit operator int() {
		return (this->eBarbat ? 65 - varsta : 60 - varsta);
	}

	friend ostream& operator<<(ostream& os, const Angajat& angajat) {
		os << "Angajatul cu numele de " << angajat.nume << " in varsta de " << angajat.varsta << ", are " << angajat.aniVechime << " ani vechime si " << (angajat.eBarbat ? "e Barbat" : "e Femeie");
		return os;
	}

	friend istream& operator>>(istream& input, Angajat& angajat) {
		cout << "Introduceti numele: ";
		char buffer[100];
		input >> buffer;
		delete[] angajat.nume;
		angajat.nume = new char[strlen(buffer) + 1];
		strcpy(angajat.nume, buffer);
		cout << "Introduceti varsta: ";
		input >> angajat.varsta;
		cout << "Introduceti ani vechime: ";
		input >> angajat.aniVechime;
		cout << "Este barbat? (1 pentru da, 0 pentru nu): ";
		input >> angajat.eBarbat;
		return input;
	}

	Angajat operator+(const Angajat& a) {
		Angajat copie = *this;
		copie.aniVechime += a.aniVechime;
		return copie;
	}

	Angajat& operator-=(int ani) {
		this->aniVechime -= ani;
		return *this;
	}

	bool operator>(const Angajat& a) {
		return aniVechime > a.aniVechime;
	}

	int operator()() {
		return this->varsta;
	}

	void serializare(string NumeFisier)
	{
		ofstream fi(NumeFisier, ios::out | ios::binary);
		int dimNume = strlen(this->nume);
		fi.write((char*)&dimNume, sizeof(dimNume));
		fi.write(this->nume, dimNume + 1);


		fi.write((char*)&this->varsta, sizeof(this->varsta));
		fi.write((char*)&this->aniVechime, sizeof(this->aniVechime));
		fi.write((char*)& this->eBarbat, sizeof(this->eBarbat));
		fi.close();
	}

};

int Angajat::varstaPensionare = 65;

class Fabrica:public Detalii, public Entitate {
	const int anInfiintare;
	char* locatie;
	float suprafata;
	int nrAngajati;
	string* numeAngajati;
	static int nrTotalFabrici;
public:

	string detalii() {
		return "Fabrica merge foarte bine";
	}
	string tipEntitate() {
		return "Acesta este o fabrica";
	}
	Fabrica() : anInfiintare(1998)
	{
		this->locatie = new char[strlen("Necunoscut") + 1];
		strcpy_s(this->locatie, strlen("Necunoscut") + 1, "Necunoscut");
		this->suprafata = 0;
		this->nrAngajati = 0;
		this->numeAngajati = NULL;
		nrTotalFabrici++;
	}

	Fabrica(const char* locatie) : anInfiintare(2000) {
		this->locatie = new char[strlen(locatie) + 1];
		strcpy_s(this->locatie, strlen(locatie) + 1, locatie);
		this->suprafata = 0;
		this->nrAngajati = 0;
		this->numeAngajati = NULL;
		nrTotalFabrici++;
	}

	Fabrica(int anInfiintare, const char* locatie, float suprafata, int nrAngajati, string* numeAngajati) : anInfiintare(anInfiintare) {
		this->locatie = new char[strlen(locatie) + 1];
		strcpy_s(this->locatie, strlen(locatie) + 1, locatie);
		this->suprafata = suprafata;
		this->nrAngajati = nrAngajati;
		this->numeAngajati = new string[this->nrAngajati];
		for (int i = 0; i < this->nrAngajati; i++)
		{
			this->numeAngajati[i] = numeAngajati[i];
		}
		nrTotalFabrici++;
	}

	void afisare()
	{
		cout << this->anInfiintare << ", " << this->locatie << ", " << this->suprafata << ", " << this->nrAngajati << ", ";
		for (int i = 0; i < this->nrAngajati; i++) {
			cout << this->numeAngajati[i] << ", ";
		}
		cout << "Aceasta fabrica este 1 din cele " << nrTotalFabrici << " existente!";
		cout << endl;
	}


	Fabrica(const Fabrica& f) :anInfiintare(f.anInfiintare) {
		this->locatie = new char[strlen(f.locatie) + 1];
		strcpy_s(this->locatie, strlen(f.locatie) + 1, f.locatie);
		this->suprafata = f.suprafata;
		this->nrAngajati = f.nrAngajati;
		this->numeAngajati = new string[this->nrAngajati];
		for (int i = 0; i < this->nrAngajati; i++)
		{
			this->numeAngajati[i] = f.numeAngajati[i];
		}
		nrTotalFabrici++;
	}


	~Fabrica() {
		if (this->locatie) {
			delete[] this->locatie;
		}

		if (this->numeAngajati) {
			delete[] this->numeAngajati;
		}
	}

	Fabrica& operator=(const Fabrica& f) {
		if (this != &f) {
			this->locatie = new char[strlen(f.locatie) + 1];
			strcpy_s(this->locatie, strlen(f.locatie) + 1, f.locatie);
			this->suprafata = f.suprafata;
			this->nrAngajati = f.nrAngajati;
			if (this->numeAngajati) {
				delete[] this->numeAngajati;
			}
			this->numeAngajati = new string[nrAngajati];
			for (int i = 0; i < this->nrAngajati; i++) {
				this->numeAngajati[i] = f.numeAngajati[i];
			}
		}
		return *this;
	}




	const int getAnInfiintare() {
		return anInfiintare;
	}

	char* getLocatie() {
		return locatie;
	}

	float getSuprafata() {
		return suprafata;
	}

	int getNrAngajati() {
		return nrAngajati;
	}

	string* getNumeAngajati() {
		return numeAngajati;
	}

	static int getNrTotalFabrici() {
		return nrTotalFabrici;
	}

	static void setNrTotalFabrici(int nrFabrici) {
		Fabrica::nrTotalFabrici = nrFabrici;
	}

	void setLocatie(const char* locatie) {
		if (this->locatie) {
			delete[] this->locatie;
		}
		this->locatie = new char[strlen(locatie) + 1];
		strcpy(this->locatie, locatie);
	}

	void setSuprafata(float suprafata) {
		this->suprafata = suprafata;
	}

	void setNumeAngajati(int nrAngajati, const string* numeAngajati) {
		this->nrAngajati = nrAngajati;
		if (this->numeAngajati) {
			delete[] this->numeAngajati;
		}
		this->numeAngajati = new string[nrAngajati];
		for (int i = 0; i < nrAngajati; i++) {
			this->numeAngajati[i] = numeAngajati[i];
		}
	}

	friend void afisareRaport(Fabrica f);

	friend ostream& operator<<(ostream& os, const Fabrica& fab) {
		os << "An infiintare: " << fab.anInfiintare << ", Locatie: " << fab.locatie << ", Suprafata: " << fab.suprafata << " mp, Numar angajati: " << fab.nrAngajati << ", Nume angajati: ";
		for (int i = 0; i < fab.nrAngajati; i++) {
			os << fab.numeAngajati[i];
			if (i < fab.nrAngajati - 1) {
				os << ", ";
			}
		}
		os << ", Total fabrici: " << fab.nrTotalFabrici;
		return os;
	}

	friend istream& operator>>(istream& input, Fabrica& fabrica) {
		cout << "Introduceti locatia fabricii: ";
		char buffer[100];
		input >> buffer;
		delete[] fabrica.locatie;
		fabrica.locatie = new char[strlen(buffer) + 1];
		strcpy(fabrica.locatie, buffer);
		cout << "Introduceti suprafata fabricii: ";
		input >> fabrica.suprafata;
		cout << "Introduceti numarul de angajati: ";
		input >> fabrica.nrAngajati;
		delete[] fabrica.numeAngajati;
		fabrica.numeAngajati = new string[fabrica.nrAngajati];
		for (int i = 0; i < fabrica.nrAngajati; ++i) {
			cout << "Introduceti numele angajatului " << i + 1 << ": ";
			input >> fabrica.numeAngajati[i];
		}
		return input;
	}

	Fabrica& operator+=(string numeAngajat) {
		string* noiNumeAngajati = new string[nrAngajati + 1];
		for (int i = 0; i < this->nrAngajati; i++) {
			noiNumeAngajati[i] = this->numeAngajati[i];
		}
		noiNumeAngajati[nrAngajati] = numeAngajat;
		this->nrAngajati++;
		if (this->numeAngajati) {
			delete[] numeAngajati;
		}
		this->numeAngajati = noiNumeAngajati;
		return *this;
	}

	string& operator[](int index) {
		if (index >= 0 && index < nrAngajati) {
			return numeAngajati[index];
		}
		else {
			string nume = "Nu e niciun nume acolo!";
			return nume;
		}
	}

	bool operator==(const Fabrica& f) {
		return (anInfiintare == f.anInfiintare && nrAngajati == f.nrAngajati);
	}

	bool operator!() {
		return nrAngajati == 0;
	}

	//citire si scriere in fisiere text

	friend ifstream& operator>>(ifstream& in, Fabrica& fabrica) {
		char buffer[100];
		in >> buffer;
		delete[] fabrica.locatie;
		fabrica.locatie = new char[strlen(buffer) + 1];
		strcpy(fabrica.locatie, buffer);
		in>> fabrica.suprafata;
		in>> fabrica.nrAngajati;
		delete[] fabrica.numeAngajati;
		fabrica.numeAngajati = new string[fabrica.nrAngajati];
		for (int i = 0; i < fabrica.nrAngajati; ++i) {
			in >> fabrica.numeAngajati[i];
		}
		return in;
	}

	friend ofstream& operator<<(ofstream& out, const Fabrica& fab) {
		out << fab.locatie << fab.suprafata << fab.nrAngajati;
		for (int i = 0; i < fab.nrAngajati; i++)\
		{
			out << fab.numeAngajati[i];
			
		}
	
		return out;
	}

	void dezerializare(string numeFisier) 
	{
		ifstream k(numeFisier, ios::in | ios::binary);
		if (k.is_open()) 
		{
			int dimLocatie = 0;
			k.read((char*)&dimLocatie, sizeof(dimLocatie));
			this->locatie = new char[dimLocatie + 1];
			k.read(this->locatie, dimLocatie + 1);

			k.read((char*)&this->suprafata, sizeof(this->suprafata));
			k.read((char*)&this->nrAngajati, sizeof(this->nrAngajati));

		}
		else
		{
			cout << "Nu s a gasit fisierul" << endl;
		}

	}


};
int Fabrica::nrTotalFabrici = 1;


void afisareRaport(Fabrica f) {
	cout << "========================================" << endl;
	cout << "             RAPORT FABRICA            " << endl;
	cout << "========================================" << endl;
	cout << "An Infiintare: " << f.anInfiintare << endl;
	cout << "Locatie: " << f.locatie << endl;
	cout << "Suprafata: " << f.suprafata << " m^2" << endl;
	cout << "Nr. Angajati: " << f.nrAngajati << endl;
	cout << "Nume Angajati:" << endl;
	for (int i = 0; i < f.nrAngajati; i++) {
		cout << "  " << i + 1 << ". " << f.numeAngajati[i] << endl;
	}
	cout << "Nr. Total Fabrici: " << f.nrTotalFabrici << endl;
	cout << "========================================" << endl;
}


class Departament {
	string denumire;
	int nrAngajati;
	Angajat* angajati;
	bool areSindicat;
public:
	Departament() {
		denumire = "";
		nrAngajati = 0;
		angajati = NULL;
		areSindicat = 0;
	}

	Departament(const string& denumire, int nrAngajati, Angajat* angajati, bool areSindicat) {
		this->denumire = denumire;
		this->nrAngajati = nrAngajati;
		this->angajati = new Angajat[nrAngajati];
		for (int i = 0; i < nrAngajati; ++i) {
			this->angajati[i] = angajati[i];
		}

		this->areSindicat = areSindicat;
	}

	Departament(const Departament& d) {
		this->denumire = d.denumire;
		this->nrAngajati = d.nrAngajati;
		this->angajati = new Angajat[d.nrAngajati];
		for (int i = 0; i < d.nrAngajati; ++i) {
			this->angajati[i] = d.angajati[i];
		}

		this->areSindicat = d.areSindicat;
	}

	// Operator de atribuire
	Departament& operator=(const Departament& d) {
		if (this != &d) {
			delete[] this->angajati;
			this->denumire = d.denumire;
			this->nrAngajati = d.nrAngajati;
			this->angajati = new Angajat[d.nrAngajati];
			for (int i = 0; i < d.nrAngajati; ++i) {
				this->angajati[i] = d.angajati[i];
			}
			this->areSindicat = d.areSindicat;
		}
		return *this;
	}

	~Departament() {
		delete[] angajati;
	}

	friend ostream& operator<<(ostream& out, const Departament& d) {
		out << "Denumire departament: " << d.denumire << endl;
		out << "Numar angajati: " << d.nrAngajati << endl;
		out << "Angajati: " << endl;
		for (int i = 0; i < d.nrAngajati; i++) {
			out << d.angajati[i] << endl << endl;
		}
		out << endl << endl;
		out << "Are sindicat:(1-Da sau 0-Nu) " << d.areSindicat << endl;
		return out;
	}


	friend ofstream& operator<<(ofstream& out, const Departament& d) {
		out  << d.denumire << endl;
		out  << d.nrAngajati << endl;
		
		for (int i = 0; i < d.nrAngajati; i++) {
			out << d.angajati[i] << endl << endl;
		}
		
		out << d.areSindicat << endl;
		return out;
	}
	friend ifstream& operator>>(ifstream& in, Departament& d) 
	{
		in >> ws;
		getline(in, d.denumire);

		in >> d.nrAngajati;
		if (d.angajati != NULL) {
			delete[]d.angajati;
		}
		d.angajati = new Angajat[d.nrAngajati];
		for (int i = 0; i < d.nrAngajati; i++) {
			in >> d.angajati[i];
		}
		in >> d.areSindicat;
		return in;

	}

	



	

	
	

	string getDenumire() {
		return this->denumire;
	}

	void setDenumire(string denumire) {
		this->denumire = denumire;
	}

	int getNrAngajati() {
		return this->nrAngajati;
	}

	Angajat* getAngajat() {
		return this->angajati;
	}

	void setAngajati(int nrAngajati, Angajat* angajati) {
		delete[] angajati;
		this->nrAngajati = nrAngajati;
		this->angajati = new Angajat[nrAngajati];
		for (int i = 0; i < nrAngajati; ++i) {
			this->angajati[i] = angajati[i];
		}
	}

	bool getAreSindicat() {
		return this->areSindicat;
	}

	void setAreSindicat(bool areSindicat) {
		this->areSindicat = areSindicat;
	}

	Angajat operator[](int index) {
		if (index >= 0 && index < this->nrAngajati)
		{
			return this->angajati[index];
		}
		else {
			Angajat a;
			return a;
		}
	}

	explicit operator int() {
		return this->nrAngajati;
	}

	
	

};

class FabricaDeLemn:public Fabrica
{ 
	string calitateLemn;
	bool areUtilajeBuneDePrelucrare;

public:
	FabricaDeLemn() :Fabrica()
	{
		this->calitateLemn = " Foarte Buna";
		this->areUtilajeBuneDePrelucrare = true;

	}

	FabricaDeLemn(string calitateLemn, bool areUtilajeBuneDePrelucare) : Fabrica("Deva")
	{
		this->calitateLemn = calitateLemn;
		this->areUtilajeBuneDePrelucrare = areUtilajeBuneDePrelucare;

	}

	~FabricaDeLemn() 
	{ 

	}

	string getCalitateLemn() {
		return this->calitateLemn;
	}
	bool getAreUtilajeBuneDePrelucrare() {
		return this->areUtilajeBuneDePrelucrare;
	}
	void setCalitateLemn(string calitateLemn) {
		this->calitateLemn = calitateLemn;
	}
	void setAreUtilajeBuneDePrelucrare(bool areUtilajeBuneDePrelucrare) 
	{
		this->areUtilajeBuneDePrelucrare = areUtilajeBuneDePrelucrare;

	}
	FabricaDeLemn(const FabricaDeLemn& f) :Fabrica (f)
	{

		this->calitateLemn = f.calitateLemn;
		this->areUtilajeBuneDePrelucrare = f.areUtilajeBuneDePrelucrare;

	}
	FabricaDeLemn& operator=(const FabricaDeLemn& f) 
	{
		if (this != &f)
		{
			Fabrica::operator=(f);
			this->calitateLemn = f.calitateLemn;
			this->areUtilajeBuneDePrelucrare = f.areUtilajeBuneDePrelucrare;
		}
			return *this;
		
	}
};
class MobilaCuModel: public Mobila 
{
	string  tipModel;
	bool areCerereMare;
public:

	MobilaCuModel() :Mobila()
	{
		this->tipModel = " Cu floricele";
		this->areCerereMare = false;

	}

	MobilaCuModel(string tipModel, bool areCerereMare) :Mobila(3, "lemn")
	{ 
		this->tipModel = tipModel;
		this->areCerereMare = areCerereMare;
	}


	~MobilaCuModel() {

	}

	MobilaCuModel(const MobilaCuModel& m) :Mobila(m)  //constr copiere
	{
		this->tipModel = m.tipModel;
		this->areCerereMare = m.areCerereMare;
	}
	
	//operator =

	MobilaCuModel& operator=(const MobilaCuModel& m) 
	{    
		if(this!=&m)

	    {
		Mobila::operator=(m);
		this->tipModel = m.tipModel;
		this->areCerereMare = m.areCerereMare;
	    }
		return *this;
		
	}

	string getTipModel() {
		return this->tipModel;
	}
	bool getAreCerereMare() {
		return this->areCerereMare;
	}


	void setTipModel(string tipModel) {
		this->tipModel = tipModel;
	}

	void setAreCerereMare(bool areCerereMare) {
		this->areCerereMare = areCerereMare;
	}
};

void main()
{

	cout << "CLASA MOBILA" << endl << endl;
	Mobila mobila;
	mobila.afisare();
	cout << endl;
	Mobila mobila2(11, "pal");
	mobila2.afisare();
	cout << endl;
	Mobila mobila3(255, "violet", 6, "stejar");
	mobila3.afisare();
	cout << endl;
	cout << endl << endl << endl << endl;


	cout << "CLASA ANGAJAT" << endl << endl;



	Angajat angajat;
	cout << angajat;
	cout << endl;

	Angajat angajat2(234, 7);
	cout << angajat2;
	cout << endl;


	Angajat angajat3("Viorel", 31);
	cout << angajat3;
	cout << endl;
	cout << endl << endl << endl << endl;

	cout << "CLASA FABRICA" << endl << endl;

	Fabrica fabrica1;
	fabrica1.afisare();
	cout << endl;

	Fabrica fabrica2("OltChim");
	fabrica2.afisare();
	cout << endl;

	string angajati[] = { "Ionel","Dorel","Mihaela" };
	Fabrica fabrica3(2005, "Buzau", 257, 3, angajati);
	fabrica3.afisare();
	cout << endl;

	cout << endl << endl << endl << endl;

	cout << "GETTERI CLASA MOBILA:" << endl;
	cout << "ID: " << mobila.getId() << endl;
	cout << "Nr. Sertare: " << mobila.getNrSertare() << endl;
	cout << "Culoare: " << mobila.getCuloare() << endl;
	cout << "Material: " << mobila.getMaterial() << endl;
	cout << "Garantie: " << Mobila::getGarantie() << " luni" << endl;



	cout << "====================================" << endl << endl;

	cout << "GETTERI CLASA ANGAJAT:" << endl;
	cout << "Nume: " << angajat.getId() << endl;
	cout << "Nume: " << angajat.getNume() << endl;
	cout << "Varsta: " << angajat.getVarsta() << endl;
	cout << "Ani de vechime: " << angajat.getAniVechime() << endl;
	cout << "Este Barbat/Femeie: " << (angajat.isBarbat() ? "Barbat" : "Femeie") << endl;
	cout << "Varsta de pensionare: " << Angajat::getVarstaPensionare() << endl;


	cout << "====================================" << endl << endl;


	cout << "GETTERI CLASA FABRICA:" << endl;
	cout << "An Infiintare: " << fabrica3.getAnInfiintare() << endl;
	cout << "Locatie: " << fabrica3.getLocatie() << endl;
	cout << "Suprafata: " << fabrica3.getSuprafata() << " m^2" << endl;
	cout << "Nr. Angajati: " << fabrica3.getNrAngajati() << endl;
	cout << "Angajati: ";
	for (int i = 0; i < fabrica3.getNrAngajati(); i++) {
		cout << fabrica3.getNumeAngajati()[i] << " | ";
	}
	cout << endl;
	cout << "Nr. Total Fabrici: " << Fabrica::getNrTotalFabrici() << endl;



	cout << "====================================" << endl << endl;


	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl << endl << endl;

	cout << "GETTERI CLASA MOBILA DUPA APLICAREA SETTERILOR:" << endl;
	mobila.setNrSertare(6);
	mobila.setCuloare("negru");
	mobila.setMaterial("plastic");
	Mobila::setGarantie(12);
	cout << "ID: " << mobila.getId() << endl;
	cout << "Nr. Sertare: " << mobila.getNrSertare() << endl;
	cout << "Culoare: " << mobila.getCuloare() << endl;
	cout << "Material: " << mobila.getMaterial() << endl;
	cout << "Garantie: " << Mobila::getGarantie() << " luni" << endl;



	cout << "====================================" << endl << endl;

	cout << "GETTERI CLASA ANGAJAT DUPA APLICAREA SETTERILOR:" << endl;
	angajat.setNume("Andreea Esca");
	angajat.setVarsta(51);
	angajat.setAniVechime(25);
	angajat.setBarbat(false);
	Angajat::setVarstaPensionare(60);

	cout << "Nume: " << angajat.getId() << endl;
	cout << "Nume: " << angajat.getNume() << endl;
	cout << "Varsta: " << angajat.getVarsta() << endl;
	cout << "Ani de vechime: " << angajat.getAniVechime() << endl;
	cout << "Este Barbat/Femeie: " << (angajat.isBarbat() ? "Barbat" : "Femeie") << endl;
	cout << "Varsta de pensionare: " << Angajat::getVarstaPensionare() << endl;
	cout << "====================================" << endl << endl;


	cout << "GETTERI CLASA FABRICA DUPA APLICAREA SETTERILOR:" << endl;
	fabrica3.setLocatie("NouaLocatie");
	fabrica3.setSuprafata(2000.0);
	string numeAngajatiNoi[] = { "Gigel Ionel", "Calin Cleopatra", "Andreea Andrei", "Ionel Ionescu", "Laura Lavric" };
	fabrica3.setNumeAngajati(5, numeAngajatiNoi);
	fabrica3.setNrTotalFabrici(11);

	cout << "An Infiintare: " << fabrica3.getAnInfiintare() << endl;
	cout << "Locatie: " << fabrica3.getLocatie() << endl;
	cout << "Suprafata: " << fabrica3.getSuprafata() << " m^2" << endl;
	cout << "Nr. Angajati: " << fabrica3.getNrAngajati() << endl;
	cout << "Angajati: ";
	for (int i = 0; i < fabrica3.getNrAngajati(); i++) {
		cout << fabrica3.getNumeAngajati()[i] << " | ";
	}
	cout << endl;
	cout << "Nr. Total Fabrici: " << Fabrica::getNrTotalFabrici() << endl;

	cout << "====================================" << endl << endl;


	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl;

	cout << "CONSTRUCTOR DE COPIERE CLASA MOBILA:" << endl;
	Mobila mobila4 = mobila3;
	mobila4.afisare();
	cout << endl << endl;


	cout << "====================================" << endl << endl;

	cout << "CONSTRUCTOR DE COPIERE CLASA ANGAJAT:" << endl;
	Angajat angajat4 = angajat3;
	cout << angajat4;
	cout << endl << endl;


	cout << "====================================" << endl << endl;


	cout << "CONSTRUCTOR DE COPIERE CLASA FABRICA:" << endl;
	Fabrica fabrica4 = fabrica3;
	fabrica4.afisare();
	cout << endl << endl;
	cout << "====================================" << endl << endl;


	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << "FUNCTII GLOBALE(CU FRIEND):" << endl;
	emitereFactura(mobila4);
	cout << endl << endl;
	cout << endl << endl;

	afisareRaport(fabrica4);
	cout << endl << endl;

	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << "OPERATORI CLASA MOBILA:" << endl;
	Mobila mobila5;
	cout << mobila5 << endl;
	mobila5 = mobila3;
	cout << mobila5 << endl;
	cout << mobila3 << endl;
	cout << mobila2 << endl;

	cout << endl;

	cout << (mobila5 && mobila5) << endl;
	cout << (mobila5 && mobila3) << endl;
	cout << (mobila5 && mobila2) << endl;
	cout << endl;

	mobila5 += 41;
	cout << mobila5 << endl;
	cout << endl;

	mobila5 += mobila2;
	cout << mobila5 << endl;
	cout << endl;

	cout << mobila5() << endl;
	cout << mobila2() << endl;
	cout << endl;

	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << "OPERATORI CLASA ANGAJAT:" << endl;
	Angajat angajat5(43, "Bianca Maria", 21, 4, 0);
	cout << angajat5 << endl;
	cout << endl;

	Angajat angajat6;
	angajat6 = angajat3;
	cout << angajat5 << endl;
	cout << angajat6 << endl;
	cout << angajat3 << endl;


	cout << endl << endl;
	cout << angajat5() << endl;
	cout << angajat6() << endl;

	cout << endl << endl;
	cout << (int)angajat5 << endl;
	cout << (int)angajat6 << endl;

	cout << endl << endl;

	angajat6 -= 1;
	cout << angajat6 << endl;
	cout << endl << endl;

	Angajat angajat7;
	angajat7 = angajat5 + angajat6;

	cout << angajat5 << endl;
	cout << angajat6 << endl;
	cout << angajat7 << endl;

	cout << endl << endl;

	cout << (angajat5 > angajat6) << endl;
	cout << (angajat7 > angajat6) << endl;
	cout << (angajat6 > angajat5) << endl;
	cout << (angajat6 > angajat7) << endl;

	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << "OPERATORI CLASA FABRICA:" << endl;
	Fabrica fabrica5;
	cout << fabrica5 << endl;
	cout << endl;
	fabrica5 = fabrica3;
	cout << fabrica3 << endl;
	cout << endl;
	cout << fabrica5 << endl;
	cout << endl;

	fabrica5 += "Bianca M M";
	cout << fabrica5 << endl;

	cout << endl;

	cout << fabrica5[0] << endl;

	fabrica5[0] = "Bianca Ma";
	cout << fabrica5 << endl;
	cout << endl;
	cout << (fabrica1 == fabrica5) << endl;
	cout << (fabrica3 == fabrica4) << endl;
	cout << (fabrica5 == fabrica5) << endl;
	cout << endl;
	Fabrica fabrica6;
	cout << !fabrica6 << endl;
	cout << !fabrica2 << endl;
	cout << !fabrica3 << endl;
	cout << !fabrica4 << endl;
	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << "VECTOR CLASA FABRICA:" << endl;
	Fabrica* fabrici = new Fabrica[2];
	fabrici[0] = fabrica1;
	fabrici[1] = fabrica4;

	for (int i = 0; i < 2; i++) {
		cout << fabrici[i] << endl;
	}
	cout << endl;

	/*for (int i = 0; i < 2; i++) {
		cin >> fabrici[i];
	}*/


	cout << endl << endl;
	for (int i = 0; i < 2; i++) {
		cout << fabrici[i] << endl;
	}
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << "VECTOR CLASA ANGAJAT:" << endl;

	Angajat* angajatiVect = new Angajat[2];
	angajatiVect[0] = angajat2;
	angajatiVect[1] = angajat3;

	for (int i = 0; i < 2; i++) {
		cout << angajatiVect[i] << endl;
	}
	cout << endl;

	/*for (int i = 0; i < 2; i++) {
		cin >> angajatiVect[i];
	}*/


	cout << endl << endl;
	for (int i = 0; i < 2; i++) {
		cout << angajatiVect[i] << endl;
	}

	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << "VECTOR CLASA MOBILA:" << endl;
	Mobila* mobilaVect = new Mobila[2];
	mobilaVect[0] = mobila2;
	mobilaVect[1] = mobila3;

	for (int i = 0; i < 2; i++) {
		cout << mobilaVect[i] << endl;
	}
	cout << endl;

	/*for (int i = 0; i < 2; i++) {
		cin >> mobilaVect[i];
	}*/


	cout << endl << endl;
	for (int i = 0; i < 2; i++) {
		cout << mobilaVect[i] << endl;
	}

	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << "---------------------------------------" << endl;


	const int noRows = 1;
	const int nrCols = 1;
	Fabrica matrixFabrica[noRows][nrCols];
	//for (int i = 0; i < noRows; ++i) {
	//	for (int j = 0; j < nrCols; ++j) {
	//		cin >> matrixFabrica[i][j];
	//	}
	//}

	for (int i = 0; i < noRows; ++i) {
		for (int j = 0; j < nrCols; ++j) {
			cout << matrixFabrica[i][j] << endl << endl;
		}
	}


	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << "CLASA NOUA - DEPARTAMENT(HAS A):" << endl;
	Angajat departamentAngajati[] = { angajat3,angajat5,angajat6 };
	Departament d;
	cout << d.getDenumire() << endl;
	cout << d.getNrAngajati() << endl;
	for (int i = 0; i < d.getNrAngajati(); i++) {
		cout << d.getDenumire() << endl;

	}
	cout << d.getAreSindicat() << endl;
	d.setDenumire("GeoTehnic");
	d.setAreSindicat(true);
	d.setAngajati(2, angajatiVect);
	cout << d << endl << endl;

	Departament d1("Transport", 3, departamentAngajati, 0);
	cout << d1 << endl << endl;

	Departament d2 = d;
	cout << d2 << endl << endl;

	d = d1;
	cout << d << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << d1[1] << endl;
	cout << d1[-3] << endl;
	cout << "---------------------------------------" << endl;
	cout << (int)d1 << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << " FISIERE TEXT CITIRE SI AFISARE FABRICA";

	ofstream f("Fabrica.txt", ios::out);
	f << fabrica5;
	f.close();

	Fabrica fab;
	ifstream g("Fabrica.txt", ios::in);
	if (g.is_open())
	{
		g >> fab;
		cout << "Obiectul a fost citit din fisier" << endl;
	}

	else
	{
		cout << "Fisierul cautat nu exista!" << endl;
	}
	cout << fab << endl;

	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << " FISIERE TEXT CITIRE SI AFISARE DEPARTAMENT";

	ofstream fis("Departament.txt", ios::out);
	fis << d2;
	fis.close();

	Departament d3;
	ifstream gfis("Departament.txt", ios::in);
	if (gfis.is_open())
	{
		gfis >> d3;
		cout << " S-a putut citi din fisier" << endl;


	}
	else
	{
		cout << " Nu s-a putut citi din fisier" << endl;
	}
	cout << d3 << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;


	cout << " FISIERE BINARE CITIRE SI AFISARE~ ANGAJAT";

	angajat2.serializare("Angajat.bin");
	cout << "Obiectul a fost scris in binar!" << endl;
	Angajat angj;
	cout << angj << endl;
	cout << angajat2 << endl;
	
	
	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	
	cout << " FISIERE BINARE CITIRE SI AFISARE~ MOBILA";


	mobila.serializare("Mobila.bin");
	cout << "Obiectul a fost scris in binar!" << endl;
	Mobila mobila8;
	mobila8.deserializare("Mobila.bin");
	cout << mobila8 << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;

	cout << "---------------------------------------" << endl;
	cout << endl << endl;
	cout << " RELATIE IS-A PENTRU CLASA FABRICA: " << endl << endl;
	FabricaDeLemn f1;
	FabricaDeLemn f2("Buna", true);
	FabricaDeLemn f3 = f1;
	cout << "f3:" << endl;
	cout << f3.getCalitateLemn() << endl;
	cout << f3.getAreUtilajeBuneDePrelucrare() << endl;
	f3.setCalitateLemn("rea");
	f3.setAreUtilajeBuneDePrelucrare(false);
	cout << "f3 MODIFICAT:" << endl;
	cout << f3.getCalitateLemn() << endl;
	cout << f3.getAreUtilajeBuneDePrelucrare() << endl;
	FabricaDeLemn f4;
	f4 = f2;
	cout << "f4:" << endl;
	cout << f4.getCalitateLemn() << endl;
	cout << f4.getAreUtilajeBuneDePrelucrare() << endl;


	cout << " RELATIE IS-A PENTRU CLASA MOBILA: " << endl << endl;
	MobilaCuModel m;
	MobilaCuModel mob("Cu picatele", false);
	MobilaCuModel mob2 = mob;
	cout << "Mobila 3 cu model" << endl;
	cout << mob2.getTipModel() << endl;
	cout << mob2.getAreCerereMare() << endl;
	cout << "-------------------------------" << endl;
	mob2.setTipModel("Model simplu");
	mob2.setAreCerereMare(true);
	cout << mob2.getTipModel() << endl;
	cout << mob2.getAreCerereMare() << endl;
	cout << "-------------------------------" << endl;
	MobilaCuModel mob3;
	mob3 = mob2;
	cout << mob3.getTipModel() << endl;
	cout << mob3.getAreCerereMare() << endl;
	cout << "-------------------------------" << endl;
	cout << " Functii VIRTUALE/ABSTRACTE" << endl;
	cout << "-------------------------------" << endl;
	cout << endl << endl;
	Detalii* det[10];
	Entitate* ent[10];
	ent[0] = new Mobila(mobila2);
	ent[1] = new Angajat(angajat4);
	ent[2] = new Fabrica(fabrica1);
	ent[3] = new Mobila(mobila2);
	ent[4] = new Angajat(angajat2);
	ent[5] = new Mobila(mobila3);
	ent[6] = new Angajat(angajat3);
	ent[7] = new Fabrica(fabrica2);
	ent[8] = new Angajat(angajat4);
	ent[9] = new Mobila(mobila5);
	det[0] = new Mobila(mobila2);
	det[1] = new Angajat(angajat4);
	det[2] = new Fabrica(fabrica1);
	det[3] = new Mobila(mobila2);
	det[4] = new Angajat(angajat2);
	det[5] = new Mobila(mobila3);
	det[6] = new Angajat(angajat3);
	det[7] = new Fabrica(fabrica2);
	det[8] = new Angajat(angajat4);
	det[9] = new Mobila(mobila5);
	for (int i = 0; i < 10; i++)
	{
		cout << ent[i]->tipEntitate() << endl;
		cout << det[i]->detalii() << endl;
	}
	
		


	

	
}
