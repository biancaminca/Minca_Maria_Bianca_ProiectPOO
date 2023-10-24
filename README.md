#define _CRT_SECURE_NO_WARNINGS
# include <iostream>
using namespace std;

class Mobila {
public:
	const int id;
	int nrSertare;
	char* culoare;
	string material;
	static int garantie;

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



};
int Mobila::garantie = 5;


class Angajat {
public:
	const int id;
	char* nume;
	int varsta;
	int aniVechime;
	bool eBarbat;
	static int varstaPensionare;
	Angajat() :id(223)
	{
		this->nume = new char[strlen("Popescu") + 1];
		strcpy_s(this->nume, strlen("Popescu") + 1, "Popescu");
		this->varsta = 38;
		this->aniVechime = 15;
		this->eBarbat = false;
	}

	Angajat(const char* nume, int varsta) : id(278)
	{
		this->nume = new char[strlen(nume) + 1];
		strcpy(this->nume, nume);
		this->varsta = varsta;
		this->aniVechime = 10;
		this->eBarbat = false;

	}
		Angajat(int id, int aniVechime) :id(id)
	{
		this->nume = new char[strlen("Giuclea") + 1];
		strcpy_s(this->nume, strlen("Giuclea") + 1, "Giuclea");
		this->varsta = 55;
		this->aniVechime = aniVechime;
		this->eBarbat = true;

	}

	void afisare2() {
		cout << "Angajatul cu numele de " << nume << " in varsta de " << varsta << ", are " << aniVechime << " ani vechime si  " << (this->eBarbat ? "e Barbat" : "e Femeie") << endl;
		cout << "Angajatul mai are  de munca inca " << (this->eBarbat ? 65-varsta : 60-varsta) << " pana la pensionare!" << endl;
	}
};


class Fabrica {
public:
	const int anInfiintare;
	char* locatie;
	float suprafata;
	int nrAngajati;
	string* numeAngajati;
	static int nrTotalFabrici;

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

	Fabrica(int anInfiintare, const char* locatie, float suprafata, int nrAngajati, string* numeAngajati): anInfiintare(anInfiintare) {
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

	 static int getNrTotalFabrici() {
		return nrTotalFabrici;
	}

};
int Fabrica::nrTotalFabrici = 1;







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
	angajat.afisare2();
	cout << endl;

	Angajat angajat2(234, 7);
	angajat2.afisare2();
	cout << endl;


	Angajat angajat3("Viorel",31);
	angajat3.afisare2();
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
	Fabrica fabrica3(25, "Buzau", 257, 3, angajati);
	fabrica3.afisare();
	cout << endl;

}
