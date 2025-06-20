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
