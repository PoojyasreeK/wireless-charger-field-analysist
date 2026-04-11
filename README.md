# wireless-charger-field-analysis

#this part of project plots plots B vs distance data at the the primary coil of the wireless charger that my team and I built using IC 555 chip for AC voltage generation
#verifies the dipole behaviour of magnetic field at close distances
#this indicates the proper operation of all the chosen components in circuit.
#aquired data points Dist vs B
(1,72)
(7,68)
(13,62)
(18,58)
(23,56)
(28,55)
(33,53)
(46,52)
(64,51)




#how the code works


# it involves numpy, matplotlib, scipy.optimise - for numerical computation, data visualisation of B vs D while scipy.optimise ideally, is for finding a curve that fits the data best.

#then we deliberately define a model which follows 1 /r **3 to see if the aquired data follows this.

#.append is used to build the dataset step by step it adds elements to the list, in this case (B,r)

#return params[0] returns the value of A (after adjusting the constant) 

#plt.legend makes the graph interpretable

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
