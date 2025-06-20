#include <iostream>
#include <string>

using namespace std;

struct Nodo {
    string nombre;
    int anio;
    Nodo* izquierda;
    Nodo* derecha;
};

// Función para crear un nuevo nodo
Nodo* crearNodo(string nombre, int anio) {
    Nodo* nuevo = new Nodo;
    nuevo->nombre = nombre;
    nuevo->anio = anio;
    nuevo->izquierda = NULL;
    nuevo->derecha = NULL;
    return nuevo;
}
// Inserción en Árbol Binario de Búsqueda (ABB)
void insertar(Nodo*& raiz, string nombre, int anio) {
    if (raiz == NULL) {
        raiz = crearNodo(nombre, anio);
    } else {
        if (anio < raiz->anio) {
            insertar(raiz->izquierda, nombre, anio);
        } else {
            insertar(raiz->derecha, nombre, anio);
        }
    }a
}

// Recorrido InOrden (izquierda - nodo - derecha)
void inOrden(Nodo* raiz) {
    if (raiz != NULL) {
        inOrden(raiz->izquierda);
        cout << raiz->nombre << " (" << raiz->anio << ")" << endl;
        inOrden(raiz->derecha);
    }
}
