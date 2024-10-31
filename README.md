import numpy as np

   # Function to solve the system using Gaussian elimination and show steps
def gaussian_elimination(A, B):
    n = len(B)
    augmented_matrix = np.hstack([A, B.reshape(-1, 1)])

 for i in range(n):
            # Find the row with the largest pivot element in the current column
        max_row = i + np.argmax(np.abs(augmented_matrix[i:, i]))
        if i != max_row:
            # Swap rows
            augmented_matrix[[i, max_row]] = augmented_matrix[[max_row, i]]
            print(f"\nSwapping row {i+1} with row {max_row+1}:")
            print(augmented_matrix)

     # Make the pivot element equal to 1
   pivot = augmented_matrix[i, i]
        if pivot != 0:
            augmented_matrix[i] /= pivot
            print(f"\nMaking the pivot in row {i+1} equal to 1:")
            print(augmented_matrix)

        # Eliminate elements below the pivot
  for j in range(i + 1, n):
            factor = augmented_matrix[j, i]
            augmented_matrix[j] -= factor * augmented_matrix[i]
            print(f"\nSubtracting {factor} * row {i+1} from row {j+1}:")
            print(augmented_matrix)

     # Back-substitution to find the values of unknowns
   x = np.zeros(n)
    for i in range(n - 1, -1, -1):
        x[i] = augmented_matrix[i, -1] - np.dot(augmented_matrix[i, i+1:n], x[i+1:n])
        print(f"\nCalculating unknown x{i+1} = {x[i]}")
    return x

# Input
n = int(input("Enter the number of equations (and unknowns): "))
A = []
B = []

print("\nEnter the coefficients for matrix A:")
for i in range(n):
    row = list(map(float, input(f"Enter the coefficients for equation {i+1}: ").split()))
    A.append(row)

print("\nEnter the values for matrix B:")
B = [float(input(f"Enter the resulting value for equation {i+1}: ")) for i in range(n)]

A = np.array(A)
B = np.array(B)

# Solve the system using Gaussian elimination
try:
    solution = gaussian_elimination(A, B)
    print("\nThe solution using Gaussian elimination is:")
    for i, x in enumerate(solution, 1):
        print(f"x{i} = {x}")
except np.linalg.LinAlgError:
    print("The matrix is singular and cannot be solved.")
