"""
Transformer Turns Ratio Calculator
----------------------------------
A menu-driven program for engineering students to calculate the
turns ratio and related quantities of an ideal single-phase transformer.

Key relations (ideal transformer):
    a = N1 / N2 = V1 / V2 = I2 / I1

where
    a  = turns ratio
    N1 = primary turns,      N2 = secondary turns
    V1 = primary voltage,    V2 = secondary voltage
    I1 = primary current,    I2 = secondary current

    a > 1  -> Step-down transformer
    a < 1  -> Step-up transformer
    a = 1  -> Isolation transformer

Impedance referred to the primary:  Z1 = a^2 * Z2
EMF equation:                        E = 4.44 * f * N * Phi_m
"""


def get_positive_float(prompt):
    """Keep asking until the user enters a valid positive number."""
    while True:
        try:
            value = float(input(prompt))
            if value <= 0:
                print("  Please enter a value greater than zero.")
                continue
            return value
        except ValueError:
            print("  Invalid input. Please enter a number.")


def turns_ratio_from_turns(n1, n2):
    """a = N1 / N2"""
    return n1 / n2


def turns_ratio_from_voltages(v1, v2):
    """a = V1 / V2"""
    return v1 / v2


def turns_ratio_from_currents(i1, i2):
    """a = I2 / I1"""
    return i2 / i1


def secondary_voltage(v1, n1, n2):
    """V2 = V1 * (N2 / N1)"""
    return v1 * n2 / n1


def secondary_turns(v1, v2, n1):
    """N2 = N1 * (V2 / V1)"""
    return n1 * v2 / v1


def secondary_current(i1, a):
    """I2 = a * I1"""
    return a * i1


def referred_impedance(z2, a):
    """Secondary impedance referred to primary: Z1 = a^2 * Z2"""
    return (a ** 2) * z2


def turns_from_emf(emf, freq, flux_max):
    """N = E / (4.44 * f * Phi_m)"""
    return emf / (4.44 * freq * flux_max)


def transformer_type(a):
    """Classify the transformer from its turns ratio."""
    if a > 1:
        return "Step-down transformer"
    if a < 1:
        return "Step-up transformer"
    return "Isolation transformer (1:1)"


def show_ratio(a):
    """Print the turns ratio nicely."""
    print(f"\n  Turns ratio a = {a:.4f}")
    print(f"  Ratio N1 : N2 = {a:.4f} : 1")
    print(f"  Type          = {transformer_type(a)}")


def menu():
    print("\n" + "=" * 46)
    print("     TRANSFORMER TURNS RATIO CALCULATOR")
    print("=" * 46)
    print(" 1. Turns ratio from number of turns (N1, N2)")
    print(" 2. Turns ratio from voltages (V1, V2)")
    print(" 3. Turns ratio from currents (I1, I2)")
    print(" 4. Find secondary voltage V2 (V1, N1, N2)")
    print(" 5. Find secondary turns N2 (V1, V2, N1)")
    print(" 6. Find secondary current I2 (I1, a)")
    print(" 7. Refer secondary impedance to primary")
    print(" 8. Find number of turns from EMF equation")
    print(" 0. Exit")
    print("-" * 46)


def main():
    while True:
        menu()
        choice = input("Enter your choice: ").strip()

        if choice == "1":
            n1 = get_positive_float("Primary turns N1: ")
            n2 = get_positive_float("Secondary turns N2: ")
            show_ratio(turns_ratio_from_turns(n1, n2))

        elif choice == "2":
            v1 = get_positive_float("Primary voltage V1 (V): ")
            v2 = get_positive_float("Secondary voltage V2 (V): ")
            show_ratio(turns_ratio_from_voltages(v1, v2))

        elif choice == "3":
            i1 = get_positive_float("Primary current I1 (A): ")
            i2 = get_positive_float("Secondary current I2 (A): ")
            show_ratio(turns_ratio_from_currents(i1, i2))

        elif choice == "4":
            v1 = get_positive_float("Primary voltage V1 (V): ")
            n1 = get_positive_float("Primary turns N1: ")
            n2 = get_positive_float("Secondary turns N2: ")
            print(f"\n  Secondary voltage V2 = {secondary_voltage(v1, n1, n2):.2f} V")

        elif choice == "5":
            v1 = get_positive_float("Primary voltage V1 (V): ")
            v2 = get_positive_float("Secondary voltage V2 (V): ")
            n1 = get_positive_float("Primary turns N1: ")
            print(f"\n  Secondary turns N2 = {secondary_turns(v1, v2, n1):.2f}")

        elif choice == "6":
            i1 = get_positive_float("Primary current I1 (A): ")
            a = get_positive_float("Turns ratio a (N1/N2): ")
            print(f"\n  Secondary current I2 = {secondary_current(i1, a):.4f} A")

        elif choice == "7":
            z2 = get_positive_float("Secondary impedance Z2 (ohm): ")
            a = get_positive_float("Turns ratio a (N1/N2): ")
            print(f"\n  Impedance referred to primary Z1 = {referred_impedance(z2, a):.4f} ohm")

        elif choice == "8":
            emf = get_positive_float("Induced EMF E (V): ")
            freq = get_positive_float("Frequency f (Hz): ")
            flux = get_positive_float("Maximum flux Phi_m (Wb): ")
            print(f"\n  Number of turns N = {turns_from_emf(emf, freq, flux):.2f}")

        elif choice == "0":
            print("\nThank you for using the calculator. Goodbye!")
            break

        else:
            print("  Invalid choice. Please select from the menu.")


if __name__ == "__main__":
    main()
