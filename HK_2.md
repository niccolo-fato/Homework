<p align="center">
  <h2 align="center">Secondo Homework(obbligatorio)</h2>
  <p align="center">Il mio secondo homework!</p>
</p>

## Traccia 🐾
Consideriamo la codifica dei numeri romani e la modifica suggerita da 
Randall Munroe nel suo blog XKCD.
Nella codifica dei numeri romani:
- non esiste lo zero
- si usano le lettere 'IVXLCDM' che corrispondono ai valori decimali
  'I' = 1, 'V' = 5, 'X' = 10, 'L' = 50, 'C' = 100, 'D' = 500, 'M' = 1000
- i numeri si scrivono da sinistra a destra cominciando con i valori più alti 
  (migliaia, centinaia, decine, unità)
- i valori delle lettere si sommano, tranne quando sono seguiti da lettere di peso maggiore, 
  nel qual caso si sottraggono
- si possono usare al massimo 3 lettere consecutive uguali per le lettere 'IXCM'
  ('III' = 3, 'XXX' = 30, 'CCC' = 300 , 'MMM' = 3000)
- per rappresentare i valori che hanno cifra decimale 4 o 9 si usa la sottrazione 
  dalla lettera seguente
  Es. 4 = 'IV'   9 = 'IX',    40 = 'XL'    39 = 'IXL'   499 = 'ID'

Nel suo blog XKCD, invece, Randall Munroe codifica i numeri romani con i corrispondenti numeri arabi: 
si concatenano i numeri arabi ottenuti sostituendo a ciascuna lettera il valore corrispondente.  
Es.    397 =>  'CCCXCVII' => 100 100 100 10 100 5 1 1 => '10010010010100511'
Chiamiamo questa codifica "formato XKCD".

Obiettivo dello homework è decodificare una lista di stringhe che rappresentano
numeri romani nel formato XKCD, e tornare i K valori maggiori in ordine decrescente.
Implementate quindi le seguenti funzioni:

Il mio codice:
```python
def decode_XKCD_tuple(xkcd_values : tuple[str, ...], k : int) -> list[int]:
    '''
    Riceve una lista di stringhe che rappresentano numeri nel formato XKCD
    ed un intero k positivo.
    Decodifica i numeri forniti e ne ritorna i k maggiori.

    Parameters
    valori_xkcd : list[str]     lista di stringhe in formato XKCD
    k : int                     numero di valori da tornare
    Returns
    list[int]                   i k massimi valori ottenuti in ordine decrescente
    '''
    list1 = list(map(decode_value, xkcd_values))
    list1.sort(reverse = True)
    return list1[:(k)]
    
def decode_value(xkcd : str ) -> int:
    '''
    Decodifica un valore nel formato XKCD e torna l'intero corrispondente.

    Parameters
    xkcd : str                  stringa nel formato XKCD
    Returns
    int                         intero corrispondente
    
    Esempio: '10010010010100511' -> 397
    '''
    xkcd1 = xkcd_to_list_of_weights(xkcd)
    return list_of_weights_to_number(xkcd1)

def xkcd_to_list_of_weights(xkcd : str) -> list[int]:
    '''
    Spezza una stringa in codifica XKCD nella corrispondente
    lista di interi, ciascuno corrispondente al peso di una lettera romana

    Parameters
    xkcd : str              stringa nel formato XKCD
    Returns
    list[int]               lista di 'pesi' corrispondenti alle lettere romane

    Esempio: '10010010010100511' -> [100, 100, 100, 10, 100, 5, 1, 1,]
    '''
    length = len(xkcd)
    list1 = []
    tmp = ''
    for x in range(length):
        tmp = ''.join([tmp,xkcd[x]])
        if x == length - 1 or xkcd[x + 1] != '0' :
            list1.append(int(tmp))
            tmp = ''
    return list1
def list_of_weights_to_number(weigths : list[int] ) -> int:
    '''
    Trasforma una lista di 'pesi' nel corrispondente valore arabo
    tenendo conto della regola di sottrazione

    Parameters
    lista_valori : list[int]    lista di 'pesi' di lettere romane
    Returns
    int                         numero arabo risultante
    
    Esempio: [100, 100, 100, 10, 100, 5, 1, 1,] -> 397
    '''
    amount = 0
    subtract = 0
    length = len(weigths)
    for x in range(length):
        if x == length - 1 or weigths[x] >= weigths[x+1]: 
            amount += weigths[x]
        else: subtract += weigths[x]
    return amount - subtract
    

if __name__ == '__main__':
    # inserisci qui i tuoi test
    print(decode_XKCD_tuple([ "150",  "1050110" , "100100010100110", "11000", "1500", "10050010100110"],6))
```

Il mio file txt:
```html
Per svolgere questo homework per comodità sono partito dalla terza funzione.
Nella terza funzione chiamata "xkcd_to_list_of_weights" mi chiedeva di inserire una 
stringa di numeri in una lista e per far questo ho utilizzato una variabile diciamo 
temporanea, perché ad ogni ciclo del for conterrà temporaneamente il numero che dovrà 
essere inserito nella lista. Nel ciclo ho utilizzato anche un if per controllare se il
numero seguente a quello puntato sia uno 0 o meno. In caso fosse uno 0 non inserisco
subito il numero contenuto nella variabile temporanea ma aspetto fino a che il numero 
seguente a quello puntato non sia uno 0. Questo lo faccio per evitare che magari in una
stringa tipo '10015100' non mi esca una lista di questo tipo = [1, 0, 0, 1, 5, 1, 0, 0],
quando il risultato dovrebbe essere [100, 1, 5, 100].
Successivamente sono passato alla seconda funzione chiamata "decode_value", qui mi viene
chiesto di convertire una stringa di numeri in un numero utilizzando "la legge" dei numeri 
romani(cioè che se un numero è seguito da un numero maggiore in quel caso si sottraggono).
Molto semplicemente ho richiamato la funzione precedentemente descritta per poter creare una 
lista con la stringa data e successivamente ho utilizzato 2 contatori uno con la somma dei 
numeri non avente di seguito un numero più grande e un altro contatore con la somma dei numeri 
avente un numero più grande dopo e infine li ho sottratti.
Nella funzione "list_of_weights_to_number" ho fatto la stessa cosa senza però richiamare 
un'altra funzione.
Infine nell'ultima funzione cioè "decode_XKCD_tuple" devo decodificare i numeri forniti e farne
tornare i k maggiori. Per far questo ho fatto un for dove al suo interno richiamo ad ogni giro 
la funzione "decode_value" e la inserivo in una lista e alla fine ho sistemato i numeri in 
ordine decrescente e poi ho fatto tornare i primi k numeri maggiori.
```