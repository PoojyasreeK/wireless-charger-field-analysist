# wireless-charger-field-analysis
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit


# MODEL
def magnetic_field_model(r, A):
    return A / r**3


# INPUT 
def get_data():
    n = int(input("Number of data points: "))
    
    r_vals = []
    B_vals = []

    print("\nEnter (r, B) values:")

    for i in range(n):
        r = float(input(f"r[{i+1}] = "))
        B = float(input(f"B[{i+1}] = "))

        if r <= 0:
            raise ValueError("Distance must be positive.")

        r_vals.append(r)
        B_vals.append(B)

    return np.array(r_vals), np.array(B_vals)


# FIT
def fit_data(r, B):
    params, _ = curve_fit(magnetic_field_model, r, B)
    return params[0]


# PLOT
def plot_data(r, B, A):
    r_fit = np.linspace(min(r), max(r), 200)
    B_fit = magnetic_field_model(r_fit, A)

    plt.figure()

    plt.scatter(r, B, label="Data")
    plt.plot(r_fit, B_fit, '--', label="Fit: A/r³")

    plt.xlabel("Distance (r)")
    plt.ylabel("Magnetic Field (B)")
    plt.title("Magnetic Field vs Distance")

    plt.legend()
    plt.grid()

    # annotation
    plt.text(
        0.95, 0.95,
        f"A ≈ {A:.3f}",
        transform=plt.gca().transAxes,
        ha='right',
        va='top'
    )

    plt.show()


#  MAIN 
def main():
    r, B = get_data()
    A = fit_data(r, B)

    print(f"\nFitted Model: B = {A:.4f} / r³")

    plot_data(r, B, A)


if __name__ == "__main__":
    main()
