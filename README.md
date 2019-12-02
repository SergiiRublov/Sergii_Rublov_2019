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
