import random
from itertools import combinations

# Feste Spinnenpositionen: genau 10 pro Feld
SPINNEN = [
    (1, 1), (3, 1), (5, 1), (7, 1), (9, 1),
    (1, 5), (3, 5), (5, 5), (7, 5), (9, 5)
]

# Feste Kopfausrichtungen pro Position
# N = Norden, O = Osten, S = Süden, W = Westen
KOEPFE = {
    (1, 1): "S", (3, 1): "S", (5, 1): "S",
    (7, 1): "S", (9, 1): "S",
    (1, 5): "N", (3, 5): "N", (5, 5): "N",
    (7, 5): "N", (9, 5): "N"
}

# Beispielwege – durch deine echten Wegkoordinaten ersetzen
ROT = [(x, 3) for x in range(11)]
GRUEN = [(5, y) for y in range(7)]

SCHWIERIGKEIT = 4  # Zulässig: 2, 4, 6 oder 8


def abstand(a, b):
    """Rasterabstand, diagonale Nachbarn zählen ebenfalls."""
    return max(abs(a[0] - b[0]), abs(a[1] - b[1]))


def weg_sicher(weg, magnetpositionen):
    """Mindestens ein vollständiges freies Feld Abstand."""
    for spinne in magnetpositionen:
        for feld in weg:
            if abstand(spinne, feld) < 2:
                return False
    return True


def verteilung_gueltig(magnetpositionen):
    rot_sicher = weg_sicher(ROT, magnetpositionen)
    gruen_sicher = weg_sicher(GRUEN, magnetpositionen)

    return rot_sicher or gruen_sicher


def neues_spiel():
    # Alle möglichen Kombinationen prüfen, damit eine Lösung
    # nicht durch eine begrenzte Zahl zufälliger Versuche scheitert.
    moegliche = list(combinations(SPINNEN, SCHWIERIGKEIT))
    random.shuffle(moegliche)

    for kombination in moegliche:
        if verteilung_gueltig(kombination):
            magnetisch = set(kombination)

            return {
                "magnetisch": magnetisch,
                "nicht_magnetisch": set(SPINNEN) - magnetisch,
                "rot_sicher": weg_sicher(ROT, magnetisch),
                "gruen_sicher": weg_sicher(GRUEN, magnetisch),
                "kopfpositionen": KOEPFE.copy()
            }

    raise ValueError(
        "Keine gültige Verteilung für diese Wege "
        "und diesen Schwierigkeitsgrad gefunden."
    )


if __name__ == "__main__":
    ergebnis = neues_spiel()

    print("Magnetische Spinnen:", ergebnis["magnetisch"])
    print("Roter Weg sicher:", ergebnis["rot_sicher"])
    print("Grüner Weg sicher:", ergebnis["gruen_sicher"])
