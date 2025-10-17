from automata.fa.dfa import DFA
from automata.base.exceptions import RejectionException

# Definición del AFD 1: Acepta cadenas de longitud exactamente 4
dfa1 = DFA(
    # Q: Conjunto de estados
    states={'q0', 'q1', 'q2', 'q3', 'q4'},
    # Σ: Alfabeto
    input_symbols={'0', '1'},
    # δ: Función de transición (diccionario anidado)
    transitions={
        'q0': {'0': 'q1', '1': 'q1'},
        'q1': {'0': 'q2', '1': 'q2'},
        'q2': {'0': 'q3', '1': 'q3'},
        'q3': {'0': 'q4', '1': 'q4'},
        'q4': {'0': 'q4', '1': 'q4'} # El estado q4 es el estado de "trampa" o "muerto" después de la longitud 4
    },
    # q0: Estado inicial
    initial_state='q0',
    # F: Conjunto de estados de aceptación
    final_states={'q4'}
)

# Cadenas para probar
cadenas_validas_1 = ['0000', '1111', '0101', '1000', '0110']
cadenas_invalidas_1 = ['0', '11', '000', '11110', '010101'] # Longitud < 4 o Longitud > 4

print("--- AFD 1: Cadenas de longitud 4 ---")
print("\n--- Cadenas Válidas ---")
for cadena in cadenas_validas_1:
    try:
        # accepts_input devuelve True si la cadena es aceptada
        aceptada = dfa1.accepts_input(cadena)
        print(f"Cadena '{cadena}': {'Aceptada' if aceptada else 'Rechazada'}")
    except RejectionException:
        print(f"Cadena '{cadena}': Rechazada (RejectionException)")

print("\n--- Cadenas Inválidas ---")
for cadena in cadenas_invalidas_1:
    try:
        aceptada = dfa1.accepts_input(cadena)
        print(f"Cadena '{cadena}': {'Aceptada' if aceptada else 'Rechazada'}")
    except RejectionException:
        # En este AFD, cadenas más largas de 4 o con símbolos incorrectos
        # pueden causar RejectionException si no hay transición definida,
        # pero la implementación anterior maneja las transiciones indefinidas
        # al caer a un estado "muerto" si la longitud es > 4 o por
        # llegar a un estado no final si la longitud es < 4.
        # accepts_input ya maneja el resultado booleano.
        print(f"Cadena '{cadena}': Rechazada (No es final o longitud incorrecta)")
