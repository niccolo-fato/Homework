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
"""
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
"""

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
    print(list1)
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
    num = xkcd_to_list_of_weights(xkcd)
    length = len(num)
    return list_of_weights_to_number(num) if num.count(num[0]) != length else num[0]*length

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
    newstring = ''.join([i for i in xkcd if i.isdigit()])
    length, list1, tmp = len(newstring), [], ''
    count = 0
    for x in range(length):
        count = count+1
        tmp = ''.join([tmp,newstring[x]]) 
        if x == (length - 1) or newstring[x + 1] != '0' :
            list1.append(tmp)
            tmp = '' 
    return list(map(int, list1))
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
    length = len(weigths)
    return sum([weigths[x] if x == length - 1 or weigths[x] >= weigths[x+1] else weigths[x]*(-1) for x in range(length)])
    

if __name__ == '__main__':
    # inserisci qui i tuoi test
    print(decode_XKCD_tuple([ "150",  "1050110" , "100100010100110", "11000", "1500", "10050010100110"],6))
```

Il mio file txt:
```html
Per svolgere questo homework, per comodità sono partito dalla terza funzione.
Nella terza funzione chiamata "xkcd_to_list_of_weights" mi chiedeva di inserire una 
stringa di numeri in una lista. Inizialmente controllo se nella stringa ci siano solo numeri
e in caso ci fossero delle lettere le elimino, successivamente ho utilizzato una variabile
"temporanea", perché ad ogni ciclo del for conterrà temporaneamente il numero che dovrà 
essere inserito nella lista. Nel ciclo poi ho utilizzato un if per controllare se il
numero seguente a quello puntato sia uno 0 o meno.Nel caso fosse uno 0 non inserisco
subito il numero contenuto nella variabile temporanea ma aspetto fino a che il numero 
seguente a quello puntato non sia uno 0. Questo lo faccio per evitare che magari da una
stringa tipo '10015100' non mi esca una lista di questo tipo = [1, 0, 0, 1, 5, 1, 0, 0](
quando il risultato dovrebbe essere questo [100, 1, 5, 100]).
Successivamente sono passato alla seconda funzione chiamata "decode_value", qui mi viene
chiesto di convertire una stringa di numeri in un numero utilizzando "la legge" dei numeri 
romani(cioè che se un numero è seguito da un numero maggiore in quel caso si sottraggono).
Molto semplicemente ho richiamato la funzione precedentemente descritta per poter creare una 
lista con la stringa data e successivamente controllo se nella stringa tutti i numeri sono uguali o meno 
e in caso non lo fossero richiamo la funzione list_of_weights_to_number, mentre se non lo sono moltiplico il primo 
numero della lista per la lunghezza di tutta la lista.
Nella funzione "list_of_weights_to_number" ho uno messo i valori in una lista dove controllo prima di farlo
se il numero successivo sia maggiore o minore, in caso fosse maggiore lo trasformo in negativo mentre se è minore lo lascio così com'è
e alla fine tramite sum() faccio la somma di tutti i numeri contenuti nella lista.
Infine nell'ultima funzione ossia "decode_XKCD_tuple" mi viene chiesto di decodificare i numeri forniti e farne
tornare i k maggiori. Per far questo ho usato map, che restituisce un oggetto mappa (che è un iteratore) 
dei risultati dopo aver applicato la funzione data a ciascun elemento di un dato iterabile, dove al suo interno richiamo 
la funzione "decode_value" e alla fine sistemo i numeri in ordine decrescente e poi faccio tornare i primi k numeri 
maggiori.
```