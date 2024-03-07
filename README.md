#include <iostream>
#include <iomanip>
#include <vector>

using namespace std;

const int Fileiras = 15;
const int Poltronas = 40;

struct Poltrona {
    bool ocupada;
};

class Teatro {
private:
    int lugaresOcupados;
    double faturamento;
    vector<vector<Poltrona>> mapa;

public:
    Teatro() : lugaresOcupados(0), faturamento(0) {
        for (int i = 0; i < Fileiras; i++) {
            vector<Poltrona> fileira(Poltronas);
            mapa.push_back(fileira);
        }
    }

    void reservarPoltrona(int fileira, int poltrona) {
        if (fileira < 1 || fileira > Fileiras || poltrona < 1 || poltrona > Poltronas) {
            cout << "Insira uma fileira e poltrona válidas." << endl;
            return;
        }
        if (mapa[fileira - 1][poltrona - 1].ocupada) {
            cout << "Esta poltrona já está ocupada!" << endl;
        } else {
            double preco = calcularPreco(fileira);
            cout << "Poltrona reservada com sucesso por R$ " << fixed << setprecision(2) << preco << endl;
            mapa[fileira - 1][poltrona - 1].ocupada = true;
            lugaresOcupados++;
            faturamento += preco;
        }
    }

    void mostrarMapaOcupacao() {
        cout << "Mapa de ocupação do teatro:" << endl;
        for (int i = 0; i < Fileiras; ++i) {
            for (int j = 0; j < Poltronas; ++j) {
                cout << (mapa[i][j].ocupada ? '#' : '.');
            }
            cout << endl;
        }
    }

    void mostrarFaturamento() {
        cout << "Quantidade de lugares ocupados: " << lugaresOcupados << endl;
        cout << "Valor da bilheteria: R$ " << fixed << setprecision(2) << faturamento << endl;
    }

private:
    double calcularPreco(int fileira) {
        if (fileira >= 1 && fileira <= 5) {
            return 50.00;
        } else if (fileira >= 6 && fileira <= 10) {
            return 30.00;
        } else {
            return 15.00;
        }
    }
};

int main() {
    Teatro teatro;

    while (true) {
        cout << "Selecione uma opção:" << endl;
        cout << "0. Finalizar" << endl;
        cout << "1. Reservar poltrona" << endl;
        cout << "2. Mapa de ocupação" << endl;
        cout << "3. Faturamento" << endl;

        int opcao;
        cin >> opcao;

        // Limpar o buffer de entrada
        cin.ignore();

        switch (opcao) {
            case 0:
                cout << "Finalizando..." << endl;
                return 0;
            case 1: {
                int fileira, poltrona;
                cout << "Insira o número da fileira e da poltrona: ";
                cin >> fileira >> poltrona;
                teatro.reservarPoltrona(fileira, poltrona);
                break;
            }
            case 2:
                teatro.mostrarMapaOcupacao();
                break;
            case 3:
                teatro.mostrarFaturamento();
                break;
            default:
                cout << "Opção inválida! Por favor, selecione uma opção válida." << endl;
        }
    }

    return 0;
}
