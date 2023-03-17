#Funkcja rozkładu normalnego (Rozkład Gaussa).
Spis treści: 
[Opis programu] #(opis programu)
[Technologie] #(technoligie)
[Kod programu] #(kod programu)

#Opis programu
Program generuje wykres funkcji gęstości prawdopodobieństwa rozkładu normalnego na podstawie podanych argumentów z linii poleceń.

Program przyjmuje trzy argumenty wymagane:
*u - wartość oczekiwana (średnia) rozkładu normalnego,
*s - odchylenie standardowe rozkładu normalnego,
*w - wariację rozkładu normalnego.

Możliwe jest podanie tylko jednego z argumentów - s lub w. Jeśli podano s, program oblicza wartość wariacji i wykorzystuje ją do obliczenia funkcji gęstości prawdopodobieństwa. Jeśli podano w, program oblicza odchylenie standardowe i na jego podstawie oblicza funkcję gęstości prawdopodobieństwa.

Program przyjmuje również dwa argumenty opcjonalne - min i max - które określają dolną i górną granicę przedziału, na którym wykres będzie generowany.
Na końcu program wyświetla wykres funkcji gęstości prawdopodobieństwa rozkładu normalnego z zaznaczonymi osiami x i y, tytułem "Rozkład normalny".

#Technologie 
W programie wykorzystane zostały dwie biblioteki: NumPy, Matplotlib.
NumPy jest biblioteką do obliczeń numerycznych w języku Python, która umożliwia operacje na dużych tablicach i macierzach danych, w tym także na danych wielowymiarowych. 

Matplotlib natomiast służy do tworzenia wykresów i wizualizacji danych w Pythonie, umożliwiając m.in. rysowanie wykresów liniowych, słupkowych, punktowych czy histogramów. 

Program wykorzystuje te biblioteki do generowania wykresu rozkładu normalnego na podstawie podanych parametrów średniej, odchylenia standardowego lub wariacji oraz przedziału wartości x. Dodatkowo, program przyjmuje argumenty z linii poleceń za pomocą modułu argparse, co umożliwia użytkownikowi podawanie wartości parametrów bezpośrednio przy uruchamianiu programu.

#Kod programu
```
from numpy import linspace, exp, pi, sqrt
from matplotlib.pyplot import ylabel, plot, grid, show, xlabel, title
import argparse

class RozkladNormalny:
    def __init__(self, s, w, u, min_val=-5, max_val=5):
        self.sigma = None
        self.var = None
        self.x = None
        self.y = None
        self.parse_args(s, w, u, min_val, max_val)

    def parse_args(self, s, w, u, min_val, max_val):
        parser = argparse.ArgumentParser()
        parser.add_argument('-s', nargs='+', type=float, help='odchylenie standardowe')
        parser.add_argument('-w', nargs='+', type=float, help='wariacja')
        parser.add_argument('-u', nargs='+', type=float, required=True, help='średnia')
        parser.add_argument('--min', type=int, default=min_val, help='dolna granica przedziału')
        parser.add_argument('--max', type=int, default=max_val, help='górna granica przedziału')
        args = parser.parse_args([f"-s {s}", f"-w {w}", f"-u {u}"])

        if args.s:
            self.sigma = args.s if len(args.s) > 1 else args.s[0]
            self.var = self.sigma ** 2
        elif args.w:
            self.var = args.w if len(args.w) > 1 else args.w[0]
            self.sigma = sqrt(self.var)
        else:
            raise ValueError('Podaj argumenty: odchylenie standardowe (-s) lub wariację (-w)')

        self.x = linspace(args.min, args.max, 100)
        self.y = ((2 * pi * self.var) ** -0.5) * exp(-0.5 * ((self.x - u) ** 2) / self.var)

    def plot(self):
        plot(self.x, self.y)
        xlabel('x')
        ylabel('y')
        title('Rozkład normalny')
        grid(True)
        show()
        
\\wywołanie klasy 
rozklad = RozkladNormalny(s=2, u=0, w=None)
rozklad.plot()

```
