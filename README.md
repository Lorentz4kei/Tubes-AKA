# Tubes-AKA
Tugas Besar Analisis Kompleksitas Algoritma

```python
import time
import matplotlib.pyplot as plt
import pandas as pd

# Input daftar produk
products = []
n = int(input("Masukkan jumlah produk: "))
for i in range(n):
    nama = input(f"Masukkan nama produk ke-{i+1}: ")
    profit = int(input(f"Masukkan keuntungan produk ke-{i+1}: "))
    berat = int(input(f"Masukkan berat produk ke-{i+1}: "))
    products.append((nama, profit, berat))

# Input berat maksimal
max_weight = int(input("Masukkan berat maksimal yang dapat ditampung: "))

# Rekursif dengan Backtracking
def knapsack_backtracking(index, remaining_weight):
    if index < 0 or remaining_weight <= 0:
        return 0

    if products[index][2] > remaining_weight:
        return knapsack_backtracking(index - 1, remaining_weight)

    include = products[index][1] + knapsack_backtracking(index - 1, remaining_weight - products[index][2])
    exclude = knapsack_backtracking(index - 1, remaining_weight)

    return max(include, exclude)

# Iteratif dengan Dynamic Programming
def knapsack_dp():
    n = len(products)
    dp = [[0] * (max_weight + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(1, max_weight + 1):
            if products[i - 1][2] <= w:
                dp[i][w] = max(products[i - 1][1] + dp[i - 1][w - products[i - 1][2]], dp[i - 1][w])
            else:
                dp[i][w] = dp[i - 1][w]

    return dp[n][max_weight]

# Analisis Kompleksitas Waktu
def measure_time():
    recursion_times = []
    dp_times = []
    weights = range(10, max_weight + 1, 5)

    for weight in weights:
        start = time.time()
        knapsack_backtracking(len(products) - 1, weight)
        end = time.time()
        recursion_times.append(end - start)

        start = time.time()
        knapsack_dp()
        end = time.time()
        dp_times.append(end - start)

    return weights, recursion_times, dp_times

# Visualisasi Grafik
def plot_graph(weights, recursion_times, dp_times):
    plt.figure(figsize=(10, 6))
    plt.plot(weights, recursion_times, label="Rekursif (Backtracking)", marker='o')
    plt.plot(weights, dp_times, label="Iteratif (Dynamic Programming)", marker='s')
    plt.xlabel("Berat Maksimal (Max Weight)")
    plt.ylabel("Waktu Eksekusi (Detik)")
    plt.title("Perbandingan Waktu Eksekusi: Rekursif vs Iteratif")
    plt.legend()
    plt.grid()
    plt.show()

# Menampilkan Tabel Waktu Eksekusi
def display_execution_times(weights, recursion_times, dp_times):
    # Buat DataFrame
    data = {
        'Max Weight (n)': weights,
        'Recursive Time (s)': recursion_times,
        'Iterative Time (s)': dp_times
    }
    df = pd.DataFrame(data)
    
    # Tampilkan tabel
    from IPython.display import display
    display(df)
    
    # Tampilkan dalam format rapi (opsional)
    print(df.to_markdown(index=False, tablefmt="grid"))

# Main Function
if __name__ == "__main__":
    print("Hasil Rekursif (Backtracking):", knapsack_backtracking(len(products) - 1, max_weight))
    print("Hasil Iteratif (Dynamic Programming):", knapsack_dp())

    weights, recursion_times, dp_times = measure_time()
    display_execution_times(weights, recursion_times, dp_times)
    plot_graph(weights, recursion_times, dp_times)
```
