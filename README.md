# UAV-Casero-Estilo-Casero-Mexicali

import RPi.GPIO as GPIO
import time
import math

# Pines GPIO para 8 motores (ducted fans) - ajusta según tu wiring
MOTORES = [17, 18, 27, 22, 23, 24, 25, 26]  # Ejemplo: pines 17-26

# Configuración PWM: frecuencia 50Hz, duty cycle 0-100%
PWM_FREQ = 50
MIN_DUTY = 10   # Idle (apagado suave)
MAX_DUTY = 90   # Full throttle (ajusta según motores)

# Inicializa GPIO
GPIO.setmode(GPIO.BCM)
for pin in MOTORES:
    GPIO.setup(pin, GPIO.OUT)
    pwm = GPIO.PWM(pin, PWM_FREQ)
    pwm.start(MIN_DUTY)  # Empieza en idle

def set_throttle(duty):
    """Aplica throttle uniforme a todos los motores"""
    for pin in MOTORES:
        pwm = GPIO.PWM(pin, PWM_FREQ)
        pwm.ChangeDutyCycle(duty)

def rotate(speed=50):
    """Giro suave: alterna potencia en pares opuestos"""
    for i in range(8):
        pwm = GPIO.PWM(MOTORES , PWM_FREQ)
        if i % 2 == 0:
            pwm.ChangeDutyCycle(speed)
        else:
            pwm.ChangeDutyCycle(speed * 0.8)  # Ligero offset para torque

def balance_kinestetico():
    """Ajuste manual: prueba y siente vibración"""
    print("Balance kinestésico: sube throttle hasta que levite.")
    print("Presiona Ctrl+C para parar.")
    try:
        duty = MIN_DUTY
        while True:
            set_throttle(duty)
            print(f"Throttle: {duty}% - siente el aire...")
            duty += 5
            time.sleep(2)
            if duty > MAX_DUTY:
                duty = MIN_DUTY
    except KeyboardInterrupt:
        print("Parando motores...")
        set_throttle(MIN_DUTY)

# Inicio
print("OVNI Mexicali - Levitación inicial. No lunar fuel.")
print("Motores listos. Sube throttle manualmente.")
balance_kinestetico()

# Limpieza
GPIO.cleanup()

git add ovni_levita.py
git commit -m "feat: levitación básica con 8 motores - kinestésico puro, no Área 51"
git remote add origin https://github.com/GioCorpus/OVNI_Mexicali.git
git push -u origin main
