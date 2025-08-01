# ODK Password Registration, Hashing & Verification (No Plaintext Stored)

## What it does
This setup lets you:

Register a password for a user in ODK without ever storing it in clear text.
Hash it locally on the device using SHA‑256 before it’s stored in the users entity list.
Verify it later in a data‑collection form by hashing the entered password again and comparing to the stored hash.
Wipe the raw password entry before submission so it never goes to the database.

## How it works

### 1. Registration
Enumerator chooses a password.
Form calculates:
digest(${pw_input}, 'sha256')
Only the Base64 SHA‑256 hash is saved to the users entity.
The raw ${pw_input} field is not included in submission output.

### 2. Verification
Enumerator enters password when logging in.
Form hashes this input in the same way:
digest(${pw_input}, 'sha256')
Looks up the stored hash from the users entity:
pulldata('users', 'pw_hash', 'username', ${username})
Compares entered hash to stored hash.


### 3. One‑shot transfer to final field
If hashes match:
The entered hash is copied once into a final_pw_hash field:

`once(if(${entered_hash} = ${correct_hash}, ${entered_hash}, ''))`

This field is included in the submissio
The user then deletes the original text in the password field
Only when the system detects a verified final hash and an empty password box can the data entry proceed.

## Key benefit
Because the password is hashed on device and the raw password field is never submitted, the database — and anyone who gains access to it — will never see the clear‑text password.

https://github.com/user-attachments/assets/42a8e224-286f-4817-b40f-f1efdfe33916



