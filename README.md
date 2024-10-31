# Function to solve the system using Gaussian elimination and show steps
def gaussian_elimination(A, B):
    n = len(B)
    for i in range(n):
        # Find the row with the largest pivot element in the current column
        max_row = max(range(i, n), key=lambda r: abs(A[r][i]))
        if i != max_row:
            # Swap rows
            A[i], A[max_row] = A[max_row], A[i]
            B[i], B[max_row] = B[max_row], B[i]
            print(f"\nSwapping row {i + 1} with row {max_row + 1}:")
            print_matrix(A, B)

        # Make the pivot element equal to 1
   pivot = A[i][i]
        if pivot == 0:
            raise ValueError("Matrix is singular, cannot proceed with Gaussian elimination.")

   for j in range(i, n):
            A[i][j] /= pivot
        B[i] /= pivot
        print(f"\nMaking the pivot in row {i + 1} equal to 1:")
        print_matrix(A, B)

        # Eliminate elements below the pivot
   for j in range(i + 1, n):
            factor = A[j][i]
            for k in range(i, n):
                A[j][k] -= factor * A[i][k]
            B[j] -= factor * B[i]
            print(f"\nSubtracting {factor} * row {i + 1} from row {j + 1}:")
            print_matrix(A, B)

     # Back-substitution to find the values of unknowns
  x = [0 for _ in range(n)]
    for i in range(n - 1, -1, -1):
        x[i] = B[i] - sum(A[i][j] * x[j] for j in range(i + 1, n))
        print(f"\nCalculating unknown x{i + 1} = {x[i]}")
    return x


def print_matrix(A, B):
    for i in range(len(B)):
        row = " ".join(f"{coeff:.2f}" for coeff in A[i])
        print(f"{row} | {B[i]:.2f}")


# Input and validation
try:
    n = int(input("Enter the number of equations (and unknowns): "))
    A = []
    B = []

 print("\nEnter the coefficients for matrix A:")
    for i in range(n):
        row = list(map(float, input(f"Enter the coefficients for equation {i + 1}: ").split()))
        if len(row) != n:
            raise ValueError(f"Expected {n} coefficients in equation {i + 1}, but got {len(row)}.")
        A.append(row)

   print("\nEnter the values for matrix B:")
    B = [float(input(f"Enter the resulting value for equation {i + 1}: ")) for i in range(n)]

    # Solve the system using Gaussian elimination
 solution = gaussian_elimination(A, B)
    print("\nThe solution using Gaussian elimination is:")
    for i, x in enumerate(solution, 1):
        print(f"x{i} = {x}")

except ValueError as ve:
    print("ValueError:", ve)
except Exception as e:
    print("An error occurred:", e)
