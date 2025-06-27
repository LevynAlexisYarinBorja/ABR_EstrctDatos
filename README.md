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
int main() {
    Nodo* raiz = NULL;
    string nombre;
    int anio;

    cout << "Ingrese el nombre del primer miembro (raíz): ";
    cin >> nombre;
    cout << "Ingrese el año de nacimiento: ";
    cin >> anio;
    insertar(raiz, nombre, anio);

    cout << "Ingrese el nombre del segundo miembro: ";
    cin >> nombre;
    cout << "Ingrese el año de nacimiento: ";
    cin >> anio;
    insertar(raiz, nombre, anio);

    cout << "Ingrese el nombre del tercer miembro: ";
    cin >> nombre;
    cout << "Ingrese el año de nacimiento: ";
    cin >> anio;
    insertar(raiz, nombre, anio);

    cout << "\nRecorrido InOrden del árbol:\n";
    inOrden(raiz);

    return 0;
}
#include <iostream>
#include <string>
using namespace std;

struct Nodo {
    string nombre;
    int anio;
    Nodo* izquierda;
    Nodo* derecha;
};

Nodo* crearNodo(string nombre, int anio) {
    Nodo* nuevo = new Nodo;
    nuevo->nombre = nombre;
    nuevo->anio = anio;
    nuevo->izquierda = NULL;
    nuevo->derecha = NULL;
    return nuevo;
}

Nodo* insertar(Nodo* raiz, string nombre, int anio) {
    if (raiz == NULL) return crearNodo(nombre, anio);

    if (anio < raiz->anio)
        raiz->izquierda = insertar(raiz->izquierda, nombre, anio);
    else if (anio > raiz->anio)
        raiz->derecha = insertar(raiz->derecha, nombre, anio);
    else
        cout << "El miembro ya existe\n";

    return raiz;
}
