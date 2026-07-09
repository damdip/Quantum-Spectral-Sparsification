# Spectral Graph Drawing in a Quantum Key

> Algoritmo quantistico per la *quantum spectral sparsification*: individuazione di sottoinsiemi di archi la cui rimozione abbassa la connettività algebrica $\lambda_2$ di un grafo al di sotto di una soglia di disegnabilità $\tau_N$, tramite una combinazione di Quantum Phase Estimation (QPE), simulazione hamiltoniana Trotterizzata per-arco e algoritmo di Grover.

**Autore:** Damiano Di Padova
**Tesi:** *Spectral Graph Drawing in a Quantum Key* (Quantum Spectral Sparsification)

---

## Panoramica

Questo repository raccoglie l'implementazione Qiskit dell'algoritmo descritto nella tesi. L'idea di fondo è che la disegnabilità spettrale di un grafo dipende dalla sua connettività algebrica $\lambda_2$ (il secondo autovalore della laplaciana $L$): se $\lambda_2 > \gamma$, con $\gamma = 10/\sqrt{N}$, il grafo può risultare troppo "connesso" per un disegno spettrale leggibile. L'algoritmo cerca, con un vantaggio quadratico rispetto alla ricerca esaustiva classica oltre una soglia di crossover $k^*$, un sottoinsieme di archi $S$ tale che la laplaciana ridotta $L(S) = L - \sum_{e \in S} L_e$ abbia $\lambda_2(L(S)) \le \gamma$.

L'architettura combina tre ingredienti quantistici:

- **Simulazione hamiltoniana per arco**, tramite decomposizione di Trotter-Suzuki della laplaciana nei contributi dei singoli archi;
- **QPE** come oracolo di verifica della disegnabilità (non di stima diretta di $\lambda_2$), condizionata coerentemente su un registro indice in sovrapposizione su tutti i sottoinsiemi di archi candidati;
- **Grover**, per amplificare gli stati del registro indice che superano l'oracolo di soglia, su uno spazio di ricerca di dimensione $N \cdot M^k$.

## Struttura del repository

| Notebook | Contenuto |
|---|---|
| `01_spectral_graph_drawing.ipynb` | Disegno spettrale dei grafi: calcolo degli autovettori della laplaciana e loro utilizzo come coordinate di embedding, con esempi su diverse famiglie di grafi (cammini, cicli, grafi espansori). |
| `02_hamiltonian_simulation_per_edge.ipynb` | Simulazione hamiltoniana Trotterizzata a livello di singolo arco: costruzione dei circuiti per $e^{-iL_e t}$, verifica della convergenza del prodotto di Trotter all'aumentare del numero di passi $r$, confronto con l'esponenziale esatto della laplaciana completa. |
| `03_qpe_edge_removal.ipynb` | QPE applicata all'operatore parametrico $\sum_S \lvert S\rangle\langle S\rvert \otimes e^{iL(S)t}$: costruzione del registro indice in sovrapposizione, oracolo di soglia OR/NOR su $\lambda_2$, gestione dei casi limite (falsi positivi/negativi) dell'oracolo. |
| `04_spectral_sparsification_circuit.ipynb` | Circuito completo: ricerca di Grover sul registro indice con QPE come sotto-routine, decomposizione MCX ricorsiva (Barenco et al., Lemma 7.3) per l'operatore di diffusione, conteggio ottimale delle iterazioni di Grover ed esperimenti end-to-end su grafi di piccole dimensioni. |

## Requisiti

```bash
python >= 3.12
qiskit
qiskit-aer
numpy
scipy
matplotlib
networkx
```

Ambiente consigliato:

```bash
python -m venv venv
source venv/bin/activate      # su Windows: venv\Scripts\activate
pip install -r requirements.txt
```


## Licenza

Distribuito a scopo accademico nell'ambito della tesi di laurea magistrale in Ingegneria Informatica. 
