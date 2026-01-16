
extras_default={
    "lenguaje": "english",
    "level": 1,
    "pokeballs": 3
}

def configurar_juego(nombre, dificultad="normal", **extras):
    print(f"Nombre del jugador: {nombre}")
    print(f"dificultad: {dificultad}")
    for clave, valor in extras.items():
        print(f"{clave.capitalize()}: {valor}")

configurar_juego("Ang11", "dificil", extras_default)
