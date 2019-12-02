# Sergii_Rublov_2019
Code in  "C" for PrPr
Projekt č. 1 Práca s jednorozmerným poľom Programa, ktora bude pracovať so záznamami zapísanými v súbore autobazar.txt obsahujúci záznamy o predaji áut. Program bude vykonávať príkazy načítané zo štandardného vstupu.

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
