<p align="center">
  <h2 align="center">Primo Homework</h2>
  <p align="center">Il mio primo homework!</p>
</p>

## Traccia 🐾
Abbiamo una stringa int_seq contenente una sequenza di interi non-negativi
    separati da virgole ed un intero positivo subtotal.

Progettare una funzione ex1(int_seq, subtotal) che
    riceve come argomenti la stringa int_seq e l'intero subtotal e
    restituisce il numero di sottostringhe di int_seq
    la somma dei cui valori è subtotal.

Ad esempio, per int_seq='3,0,4,0,3,1,0,1,0,1,0,0,5,0,4,2' e subtotal=9,
    la funzione deve restituire 7.

Infatti:
```h
'3,0,4,0,3,1,0,1,0,1,0,0,5,0,4,2'
 _'0,4,0,3,1,0,1,0'_____________
 _'0,4,0,3,1,0,1'_______________
 ___'4,0,3,1,0,1,0'_____________
____'4,0,3,1,0,1'_______________
____________________'0,0,5,0,4'_
______________________'0,5,0,4'_
 _______________________'5,0,4'_
```
NOTA: è VIETATO usare/importare ogni altra libreria a parte quelle già presenti

NOTA: il timeout previsto per questo esercizio è di 1 secondo per ciascun test (sulla VM)

ATTENZIONE: quando caricate il file assicuratevi che sia nella codifica UTF8
    (ad esempio editatelo dentro Spyder)

Il mio codice:
```python
def ex1(int_seq, subtotal):
    subsets,count_zero = 0, 0
    seq = [int(number) for number in int_seq.split(',')]
    length = len(seq)
    if seq.count(seq[0]) != length:
        for x in range(length):
            amount = 0
            if seq[x] == 0:
                count_zero += 1
                continue
            for y in range(x, length):
                amount += seq[y]
                if amount > subtotal: break
                elif amount == subtotal: subsets += 1 + count_zero
            count_zero = 0
    else:
        return lenght - subtotal + 1
    return subsets


if __name__ == '__main__':
    print(ex1('3,0,4,0,3,1,0,1,0,1,0,0,5,0,4,2', 9))
```