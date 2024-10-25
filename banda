import RPi.GPIO as GPIO
import time

# Configuración inicial
GPIO.setmode(GPIO.BOARD)

# Pines para los sensores
pins_sensores = {
    'S1': 30,
    'S2': 31,
    'S3': 32,
    'S4': 33
}

# Pines para los motores
pin_derecha = 22
pin_izquierda = 23

# Configuración de pines
for pin in pins_sensores.values():
    GPIO.setup(pin, GPIO.IN)

GPIO.setup(pin_derecha, GPIO.OUT)
GPIO.setup(pin_izquierda, GPIO.OUT)

# Configuración del pin para la tecla #
pin_tecla = 24  # Asegúrate de conectar correctamente el pin de la tecla #

# Configuración del pin de la tecla #
GPIO.setup(pin_tecla, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)  # Esto asume que la tecla está conectada con un pull-down resistor

def mover_banda_derecha():
    GPIO.output(pin_derecha, GPIO.HIGH)
    GPIO.output(pin_izquierda, GPIO.LOW)
    print("Moviendo banda hacia la derecha...")

def mover_banda_izquierda():
    GPIO.output(pin_izquierda, GPIO.HIGH)
    GPIO.output(pin_derecha, GPIO.LOW)
    print("Moviendo banda hacia la izquierda...")

def detener_banda():
    GPIO.output(pin_derecha, GPIO.LOW)
    GPIO.output(pin_izquierda, GPIO.LOW)
    print("Banda detenida.")

# Función principal
def control_banda():
    try:
        # Solicitar clave de 4 dígitos
        clave = input("Ingrese la clave de 4 dígitos: ")

        # Definir topes según la clave
        tope_derecha = int(clave[0])  # S1, S2, S3
        tope_izquierda = int(clave[1])  # S2, S3, S4

        print(f"Tope derecha: S{tope_derecha}")
        print(f"Tope izquierda: S{tope_izquierda}")

        # Esperar que se presione la tecla '#'
        print("Presione la tecla '#' para iniciar el movimiento de la banda.")
        while True:
            if GPIO.input(pin_tecla) == GPIO.HIGH:
                print("Tecla '#' presionada, iniciando movimiento de la banda.")
                break
            time.sleep(0.1)

        # Bucle para controlar el movimiento de la banda
        while True:
            # Detectar si la pieza está en algún sensor
            pieza_en_sensor = None
            for sensor, pin in pins_sensores.items():
                if GPIO.input(pin) == GPIO.HIGH:
                    pieza_en_sensor = sensor
                    break

            if pieza_en_sensor:
                print(f"Pieza detectada en {pieza_en_sensor}")

                if pieza_en_sensor == f'S{tope_derecha}':
                    detener_banda()
                    print(f"Tope derecho alcanzado: S{tope_derecha}")
                    break

                elif pieza_en_sensor == f'S{tope_izquierda}':
                    detener_banda()
                    print(f"Tope izquierdo alcanzado: S{tope_izquierda}")
                    break

                else:
                    # Mover la banda dependiendo de la dirección
                    if int(pieza_en_sensor[-1]) < tope_derecha:
                        mover_banda_derecha()
                    elif int(pieza_en_sensor[-1]) > tope_izquierda:
                        mover_banda_izquierda()
                    else:
                        detener_banda()

            time.sleep(1)

    except KeyboardInterrupt:
        GPIO.cleanup()

# Iniciar el control de la banda
control_banda()
