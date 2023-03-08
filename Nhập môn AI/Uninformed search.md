##### Bài 2
**BFS**
Fringe: ($\theta$, Lugoj, 0)
Closed: ($\theta$, Lugoj, 0)

Fringe: (L->T, Timiseara, 111), (L->M, Mehadia, 70)
Closed: (L->T, Timiseara, 111), ($\theta$, Lugoj, 0)

Fringe: (L->M, Mehadia, 70), (T->A, Arad, 229)
Closed: (L->M, Mehadia, 70), (L->T, Timiseara, 111), ($\theta$, Lugoj, 0)

Fringe: (T->A, Arad, 229), (M->D, Druleta, 145)
Closed: (T->A, Arad, 229), (L->M, Mehadia, 70), (L->T, Timiseara, 111), ($\theta$, Lugoj, 0)

Fringe: (M->D, Druleta, 145), (A->Z, Zerind, 304), (A->S, Sihiu, 369)
Closed: (M->D, Druleta, 145), (L->M, Mehadia, 70), (L->T, Timiseara, 111), ($\theta$, Lugoj, 0)

Fringe: (A->Z, Zerind, 304), (A->S, Sihiu, 369), (D->C, Craiova, 265)
Closed: (A->Z, Zerind, 304), (M->D, Druleta, 145), (L->M, Mehadia, 70), (L->T, Timiseara, 111), ($\theta$, Lugoj, 0)

Fringe: (A->S, Sihiu, 369), (D->C, Craiova, 265)
(...)

**DFS**
Fringe: ($\theta$, Lugoj, 0)
Closed: ($\theta$, Lugoj, 0)

Fringe: (L->T, Timiseara, 111), (L->M, Mehadia, 70)
Closed: ($\theta$, Lugoj, 0), (L->M, Mehadia, 70)

Fringe: (L->T, Timiseara, 111), (M->D, Druleta, 145)
Closed: ($\theta$, Lugoj, 0), (L->M, Mehadia, 70), (M->D, Druleta, 145)

Fringe: (L->T, Timiseara, 111), (D->C, Craiova, 265)
Closed: ($\theta$, Lugoj, 0), (L->M, Mehadia, 70), (M->D, Druleta, 145), (D->C, Craiova, 265)

Fringe: (L->T, Timiseara, 111), (C->R, Rimnicu Vilea, 411), (C->P, Pitesti, 403)
Closed: ($\theta$, Lugoj, 0), (L->M, Mehadia, 70), (M->D, Druleta, 145), (D->C, Craiova, 265), (C->P, Pitesti, 403)

Fringe: (L->T, Timiseara, 111), (C->R, Rimnicu Vilea, 411), (P->R, Rimnicu Vilea, 500)
(...)
