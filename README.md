import numpy as np

# Function to solve the system using Gauss-Jordan elimination and show steps
def gauss_jordan_elimination(A, B):
    n = len(B)
    augmented_matrix = np.hstack([A, B.reshape(-1, 1)])

 for i in range(n):
        # Find the row with the largest pivot element in the current column
        max_row = i + np.argmax(np.abs(augmented_matrix[i:, i]))
        if i != max_row:
            # Swap rows
            augmented_matrix[[i, max_row]] = augmented_matrix[[max_row, i]]
            print(f"\nSwapping row {i + 1} with row {max_row + 1}:")
            print(augmented_matrix)

        # Make the pivot element equal to 1
   pivot = augmented_matrix[i, i]
        if pivot == 0:
            raise ValueError("Matrix is singular, cannot proceed with Gauss-Jordan elimination.")

 augmented_matrix[i] /= pivot
        print(f"\nMaking the pivot in row {i + 1} equal to 1:")
        print(augmented_matrix)

        # Eliminate all other elements in the current column
   for j in range(n):
            if j != i:
                factor = augmented_matrix[j, i]
                augmented_matrix[j] -= factor * augmented_matrix[i]
                print(f"\nSubtracting {factor} * row {i + 1} from row {j + 1}:")
                print(augmented_matrix)

    # Extract the solution (last column of the augmented matrix)
 x = augmented_matrix[:, -1]
    return x


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
    A = np.array(A)
    B = np.array(B)

    # Solve the system using Gauss-Jordan elimination    
solution = gauss_jordan_elimination(A, B)
 print("\nThe solution using Gauss-Jordan elimination is:")
    for i, x in enumerate(solution, 1):
        print(f"x{i} = {x}")

except ValueError as ve:
    print("ValueError:", ve)
except np.linalg.LinAlgError:
    print("LinAlgError: The matrix is singular and cannot be solved.")
except Exception as e:
    print("An error occurred:", e)
