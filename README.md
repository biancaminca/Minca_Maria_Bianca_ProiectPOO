#define _CRT_SECURE_NO_WARNINGS
# include <iostream>
using namespace std;

class Mobila {
	const int id;
	char* culoare;
	int nrSertare;
	string material;
	static int garantie;
public:
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

	bool operator&&(const Mobila& m) {
		return this->nrSertare == m.nrSertare && this->material == m.material;
	}

	Mobila& operator +=(int nrSertare) {
		this->nrSertare += nrSertare;
		return *this;
	}

	int operator()()  {
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

class Angajat {
	const int id;
	char* nume;
	int varsta;
	int aniVechime;
	bool eBarbat;
	static int varstaPensionare;
public:

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
	

	void afisare() {
		cout << "Angajatul cu numele de " << nume << " in varsta de " << varsta << ", are " << aniVechime << " ani vechime si  " << (this->eBarbat ? "e Barbat" : "e Femeie") << endl;
		cout << "Angajatul mai are  de munca inca " << (this->eBarbat ? 65 - varsta : 60 - varsta) << " pana la pensionare!" << endl;
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
		if (this != &a) {
			if (this->nume != NULL) {
				delete[]this->nume;
			}
			this->nume = new char[strlen(a.nume) + 1];
			strcpy_s(this->nume, strlen(a.nume) + 1, a.nume);
			varsta = a.varsta;
			aniVechime = a.aniVechime;
			eBarbat = a.eBarbat;
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

	Angajat operator+(const Angajat& a)  {
		Angajat copie = *this;
		copie.aniVechime += a.aniVechime;
		return copie;
	}

	Angajat& operator-=(int ani) {
		this->aniVechime -= ani;
		return *this;
	}

	bool operator>(const Angajat& a)  {
		return aniVechime > a.aniVechime;
	}

	int operator()() {
		return this->varsta;
	}

};

int Angajat::varstaPensionare = 65;

class Fabrica {
	const int anInfiintare;
	char* locatie;
	float suprafata;
	int nrAngajati;
	string* numeAngajati;
	static int nrTotalFabrici;
public:


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

	Fabrica& operator+=( string numeAngajat) {
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

	bool operator==(const Fabrica& f)  {
		return (anInfiintare == f.anInfiintare && nrAngajati == f.nrAngajati);
	}

	bool operator!()  {
		return nrAngajati == 0;
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
	angajat.afisare();
	cout << endl;

	Angajat angajat2(234, 7);
	angajat2.afisare();
	cout << endl;


	Angajat angajat3("Viorel", 31);
	angajat3.afisare();
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
	angajat4.afisare();
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
	cout<<(fabrica1 == fabrica5) << endl;
	cout<<(fabrica3 == fabrica4) << endl;
	cout<<(fabrica5 == fabrica5) << endl;
	cout << endl;
	Fabrica fabrica6;
	cout << !fabrica6 << endl;
	cout << !fabrica2 << endl;
	cout << !fabrica3 << endl;
	cout << !fabrica4 << endl;
	cout << endl << endl << endl << endl;
	cout << "---------------------------------------" << endl;
	cout << endl << endl;

}
