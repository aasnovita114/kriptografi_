# kriptografi_
NAMA  :AAS NOVITASARI

NIM    :312210167

KELAS  :TI.22.A1

```
# Membuat matriks Playfair berdasarkan kunci yang diberikan
def generate_playfair_matrix(key):
    key = key.upper().replace("J", "I")  # Mengganti 'J' dengan 'I'
    matrix = []
    used = set()

    # Menghilangkan duplikasi huruf dalam kunci dan menambahkan ke matriks
    for char in key:
        if char not in used and char.isalpha():
            matrix.append(char)
            used.add(char)

    # Mengisi matriks dengan sisa huruf dari alfabet
    for char in "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if char not in used:
            matrix.append(char)

    # Mengembalikan matriks 5x5
    return [matrix[i:i+5] for i in range(0, 25, 5)]

# Menyiapkan teks dengan memisahkan menjadi pasangan huruf
def prepare_text(text):
    text = text.upper().replace("J", "I").replace(" ", "")  # Ubah menjadi huruf besar dan hilangkan spasi
    pairs = []
    i = 0
    while i < len(text):
        a = text[i]
        b = text[i + 1] if i + 1 < len(text) else 'X'  # Jika huruf tunggal, tambahkan 'X'
        if a == b:
            pairs.append((a, 'X'))  # Jika ada dua huruf yang sama, tambahkan 'X'
            i += 1
        else:
            pairs.append((a, b))
            i += 2
    return pairs

# Mencari posisi huruf dalam matriks Playfair
def find_position(matrix, char):
    for i, row in enumerate(matrix):
        if char in row:
            return i, row.index(char)
    return None

# Fungsi enkripsi dengan Playfair Cipher
def encrypt_playfair(plaintext, key):
    matrix = generate_playfair_matrix(key)
    pairs = prepare_text(plaintext)
    ciphertext = ""

    for a, b in pairs:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)

        if row1 == row2:  # Jika dalam baris yang sama, geser ke kanan
            ciphertext += matrix[row1][(col1 + 1) % 5] + matrix[row2][(col2 + 1) % 5]
        elif col1 == col2:  # Jika dalam kolom yang sama, geser ke bawah
            ciphertext += matrix[(row1 + 1) % 5][col1] + matrix[(row2 + 1) % 5][col2]
        else:  # Jika berbeda baris dan kolom, tukar berdasarkan posisi
            ciphertext += matrix[row1][col2] + matrix[row2][col1]

    return ciphertext

# Fungsi dekripsi dengan Playfair Cipher
def decrypt_playfair(ciphertext, key):
    matrix = generate_playfair_matrix(key)
    pairs = [ciphertext[i:i+2] for i in range(0, len(ciphertext), 2)]
    plaintext = ""

    for a, b in pairs:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)

        if row1 == row2:  # Jika dalam baris yang sama, geser ke kiri
            plaintext += matrix[row1][(col1 - 1) % 5] + matrix[row2][(col2 - 1) % 5]
        elif col1 == col2:  # Jika dalam kolom yang sama, geser ke atas
            plaintext += matrix[(row1 - 1) % 5][col1] + matrix[(row2 - 1) % 5][col2]
        else:  # Jika berbeda baris dan kolom, tukar berdasarkan posisi
            plaintext += matrix[row1][col2] + matrix[row2][col1]

    return plaintext

# Contoh penggunaan
key = "TEKNIK INFORMATIKA"
plaintext1 = "GOOD BROOM SWEEP CLEAN"
plaintext2 = "REDWOOD NATIONAL STATE PARK"
plaintext3 = "JUNK FOOD AND HEALTH PROBLEMS"

# Enkripsi
ciphertext1 = encrypt_playfair(plaintext1, key)
ciphertext2 = encrypt_playfair(plaintext2, key)
ciphertext3 = encrypt_playfair(plaintext3, key)

# Dekripsi
decrypted_text1 = decrypt_playfair(ciphertext1, key)
decrypted_text2 = decrypt_playfair(ciphertext2, key)
decrypted_text3 = decrypt_playfair(ciphertext3, key)

# Hasil enkripsi dan dekripsi
print("Plaintext 1:", plaintext1)
print("Ciphertext 1:", ciphertext1)
print("Decrypted 1:", decrypted_text1)

print("Plaintext 2:", plaintext2)
print("Ciphertext 2:", ciphertext2)![Screenshot 2024-10-15 092744](https://github.com/user-attachments/assets/d0cbd8c2-90d8-45b1-829f-ce30dc02799d)

print("Decrypted 2:", decrypted_text2)

print("Plaintext 3:", plaintext3)
print("Ciphertext 3:", ciphertext3)
print("Decrypted 3:", decrypted_text3)
```
```
Plaintext 1: GOOD BROOM SWEEP CLEAN
Ciphertext: CMRCDFRWR PYNVKLBPTBRN
Decrypted: GOODBROXOQXIWEBFHKDFXQ

Plaintext 2: REDWOOD NATIONAL STATE PARK
Ciphertext: FNCXRWRCTDETRKBHHIH KLD NW
Decrypted: REDWOXODNATIONALSTPTQBKX

Plaintext 3: JUNK FOOD AND HEALTH PROBLEMS
Ciphertext: TYINORRCDTAQTBHELQMRLVIFQY
Decrypted: IUNKFOODANDHEALTHPROBLEMSX
```

![Screenshot 2024-10-15 092744](https://github.com/user-attachments/assets/294576f6-8bdf-494f-8369-d1b292f61e65)

