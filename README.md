# Sergii_Rublov_2019
Code in  "C" for PrPr


#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

time_t get_time(char *date)                            // funkciu pre prevod retazca s datumom do formatu time_t, zapiste do pamate hodnotu „rrrr“ - vypiste rok z formatu „rrrrymdd“
{
	struct tm t;
	char substr1[5], substr2[3], substr3[3];           
	strncpy(substr1, date, 4);
	substr1 [4] = '\0';                               // zapiste hodnotu „rrrr“ do pamate
	strncpy(substr2, date+4, 2);
	substr2 [2] = '\0';                               // zapiste hodnotu „mm“ do pamate
	strncpy(substr3, date+6, 2);
	substr3 [2] = '\0';                               // zapiste hodnotu „dd“ do pamate
	t.tm_year = atoi(substr1) - 1900;                 
	t.tm_mon = atoi(substr2) - 1;                     
	t.tm_mday = atoi(substr3);                        
	t.tm_hour = 0;
    t.tm_min = 0;
    t.tm_sec = 1;
    t.tm_isdst = -1;
	time_t t1 = mktime(&t);                          // skonvertujte format „rrrrmdd“ na „rrrr mm dd“
	return t1;                                       // vrati hodnotu t1
}

int v (char *meno, char *SPZ, int *typ, double *cena, char *datum, FILE *fv)   //po aktivovani program otvori subor a vypise jednotlive zaznamy zo suboru na obrazovku.
{
	fv = fopen("autobazar.txt", "r");                // otvorte textovy subor na citanie
	if (fv != NULL)                                  // subor nie je 0
	{
		int i = 0;
		char c;
		for (; ; i++)
		{
			int j = i * 50;
			while((c = fgetc(fv)) != '\n')                    // funkcie na citanie suborov znak po znaku a na koniec riadku
			{
				if (c != EOF)
					meno[j++] = c;                            // zapisovanie precitaneho znaku do pola Meno s prechodom na dalsi prvok pola
				else
					return i;
			}
			meno[j] = '\0';                                   // napiste na meno riadku na koniec aktualneho mena 
				
			j = i * 8;                                        // nastavenie posunu v poli SPZ pre aktualny prvok
			while((SPZ[j++] = fgetc(fv)) != '\n');            // citanie SPZ z pamate
			SPZ[--j] = '\0';
				
			j = 0;
			char typeChar[2];
			while((typeChar[j++] = fgetc(fv)) != '\n');       // citanie z pamate - auta 0 alebo 1
			typ[i] = atoi(typeChar);                          // previest subor z char na int
				
			j = 0;
			char cenaChar[10];
			while((cenaChar[j++] = fgetc(fv)) != '\n');       // nacitanie ceny automobilu z pamate
			cena[i] = atof(cenaChar);                         // previest subor z char na int
				
			j = i * 9;
			while((datum[j++] = fgetc(fv)) != '\n');         // citanie datumu
			datum[--j] = '\0';
						
			if (fgetc(fv) == EOF)                            // po dokonceni citania je subor ukonceny
				break;
		}
		
		fclose(fv);                                          // zatvorte textovy subor
		return i+1;
	}
	else
		printf("Neotvoreny subor\n");                         // zobrazit informacie, ak subor nie je otvoreny
	return 0;
}


int main (void)
{
	char meno[500], SPZ[80], datum[90];
	double cena[10];
	int  typ[10];
	char c;
	int fread = 0;
	FILE *fv = NULL;
	int number = 0;
	while((c = getchar()) != 'k')	
	{
		switch (c)
		{
			case 'v':                                                          // zavolajte funkciu 'v'
			{
				number = v(meno, SPZ, typ, cena, datum, fv);
				fread = 1;
				for (int i = 0; i < number; i++)
				{
					printf("meno priezvisko: %s\n", &meno[i*50]);	
					printf("SPZ: %s\n", &SPZ[i*8]);
					printf("typ auta: %d\n", typ[i]);
					printf("cena: %.2lf\n", cena[i]);
					printf("datum: %s\n", &datum[i*9]);
					printf("\n");
				}
				break;		
			}
			case 'o':
			{
				if (fread == 1)
				{
					char rmd[9];
					scanf("%s", rmd);
					o(meno, SPZ, typ, cena, datum, rmd, fv);                            // zavolajte funkciu 'î'	
				}
				break;
			}
			case 'n':                                                     
			{
				if (fread == 1)
				{
					number = n(SPZ, fv);                                                // zavolajte funkciu 'n'
				}
				break;
			}
			case 's':
			{
				s(SPZ, number);	                                                       // zavolajte funkciu 's'
				break;
			}
			case 'm':
			{ 
				m(SPZ, number);                                                       // zavolajte funkciu 'm'
				break;
			}
			case 'p':
			{
				p(SPZ, number);                                                      // zavolajte funkciu 'p'
				break;
			}
			case 'z':
			{
				z(SPZ, number);                                                      // zavolajte funkciu 'z'
				break;
			}
		}
	}
	return 0;	
}
