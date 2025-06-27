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

Nodo* buscar(Nodo* raiz, string nombre) {
    if (raiz == NULL) return NULL;
    if (raiz->nombre == nombre) return raiz;

    Nodo* izq = buscar(raiz->izquierda, nombre);
    if (izq != NULL) return izq;
    return buscar(raiz->derecha, nombre);
}

Nodo* buscarPorAnio(Nodo* raiz, int anio) {
    if (raiz == NULL) return NULL;
    if (raiz->anio == anio) return raiz;
    if (anio < raiz->anio) return buscarPorAnio(raiz->izquierda, anio);
    return buscarPorAnio(raiz->derecha, anio);
}

Nodo* minimo(Nodo* raiz) {
    while (raiz && raiz->izquierda != NULL) raiz = raiz->izquierda;
    return raiz;
}

Nodo* eliminar(Nodo* raiz, int anio) {
    if (raiz == NULL) return NULL;

    if (anio < raiz->anio)
        raiz->izquierda = eliminar(raiz->izquierda, anio);
    else if (anio > raiz->anio)
        raiz->derecha = eliminar(raiz->derecha, anio);
    else {
        if (raiz->izquierda == NULL) {
            Nodo* temp = raiz->derecha;
            delete raiz;
            return temp;
        } else if (raiz->derecha == NULL) {
            Nodo* temp = raiz->izquierda;
            delete raiz;
            return temp;
        }
        Nodo* temp = minimo(raiz->derecha);
        raiz->anio = temp->anio;
        raiz->nombre = temp->nombre;
        raiz->derecha = eliminar(raiz->derecha, temp->anio);
    }
    return raiz;
}

void inorden(Nodo* raiz) {
    if (raiz != NULL) {
        inorden(raiz->izquierda);
        cout << raiz->nombre << " (" << raiz->anio << ")\n";
        inorden(raiz->derecha);
    }
}

bool ancestros(Nodo* raiz, string nombre) {
    if (raiz == NULL) return false;
    if (raiz->nombre == nombre) return true;

    if (ancestros(raiz->izquierda, nombre) || ancestros(raiz->derecha, nombre)) {
        cout << raiz->nombre << " -> ";
        return true;
    }
    return false;
}

void descendientes(Nodo* raiz, string nombre) {
    Nodo* nodo = buscar(raiz, nombre);
    if (nodo == NULL) {
        cout << "Miembro no encontrado.\n";
        return;
    }

    if (nodo->izquierda != NULL) {
        cout << "Hijo izquierdo: " << nodo->izquierda->nombre << "\n";
        descendientes(nodo->izquierda, nodo->izquierda->nombre);
    }

    if (nodo->derecha != NULL) {
        cout << "Hijo derecho: " << nodo->derecha->nombre << "\n";
        descendientes(nodo->derecha, nodo->derecha->nombre);
    }
}

bool esParteDeRama(Nodo* raiz, string ancestro, string posibleDesc) {
    Nodo* nodoAncestro = buscar(raiz, ancestro);
    if (nodoAncestro == NULL) return false;
    Nodo* nodoDesc = buscar(nodoAncestro, posibleDesc);
    return nodoDesc != NULL;
}

int main() {
    Nodo* raiz = NULL;
    int opcion, anio, cantidad;
    string nombre, nombre2;

    do {
        cout << "\n--- Menú Árbol Genealógico ---\n";
        cout << "1. Insertar miembro\n";
        cout << "2. Eliminar miembro\n";
        cout << "3. Buscar miembro\n";
        cout << "4. Recorrer InOrden\n";
        cout << "5. Mostrar ancestros\n";
        cout << "6. Mostrar descendientes\n";
        cout << "7. Verificar si pertenece a una rama\n";
        cout << "0. Salir\n";
        cout << "Seleccione una opción: ";
        cin >> opcion;

        switch(opcion) {
            case 1:
                cout << "¿Cuántos miembros deseas insertar?: ";
                cin >> cantidad;
                for (int i = 0; i < cantidad; i++) {
                    cout << "Nombre: ";
                    cin >> nombre;
                    cout << "Año de nacimiento: ";
                    cin >> anio;
                    raiz = insertar(raiz, nombre, anio);
                }
                break;
            case 2:
                cout << "Ingrese el año de nacimiento del miembro a eliminar: ";
                cin >> anio;
                raiz = eliminar(raiz, anio);
                break;
            case 3:
                cout << "Ingrese el nombre del miembro a buscar: ";
                cin >> nombre;
                {
                    Nodo* resultado = buscar(raiz, nombre);
                    if (resultado != NULL)
                        cout << "Encontrado: " << resultado->nombre << " (" << resultado->anio << ")\n";
                    else
                        cout << "No encontrado.\n";
                }
                break;
            case 4:
                cout << "--- Recorrido InOrden ---\n";
                inorden(raiz);
                break;
            case 5:
                cout << "Ingrese el nombre del miembro: ";
                cin >> nombre;
                cout << "Ancestros: ";
                if (!ancestros(raiz, nombre))
                    cout << "No se encontraron ancestros.\n";
                else
                    cout << nombre << "\n";
                break;
            case 6:
                cout << "Ingrese el nombre del miembro: ";
                cin >> nombre;
                descendientes(raiz, nombre);
                break;
            case 7:
                cout << "Nombre del posible ancestro: ";
                cin >> nombre;
                cout << "Nombre del posible descendiente: ";
                cin >> nombre2;
                if (esParteDeRama(raiz, nombre, nombre2))
                    cout << nombre2 << " es descendiente de " << nombre << "\n";
                else
                    cout << "No pertenece a la misma rama\n";
                break;
        }
    } while(opcion != 0);

    return 0;
}
