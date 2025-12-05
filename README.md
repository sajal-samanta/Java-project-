cd FileEncryptionUtility

.\run.bat



javac -d bin src/com/filecipher/*.java src/com/filecipher/cipher/*.java src/com/filecipher/cipher/exceptions/*.java src/com/filecipher/ui/*.java



compile and run :

cd FileEncryptionUtility

javac -d bin src/com/fileencryption/exception/*.java src/com/fileencryption/core/*.java src/com/fileencryption/ui/*.java src/com/fileencryption/Main.java
java -cp bin com.fileencryption.Main




# 1. Go to your project directory
cd "C:\Users\Sajal Samanta\FileEncryptionUtility"

# 2. Check if Java is installed
java -version

# 3. Check if javac is available
javac -version

# 4. List all files to verify structure
dir /s

# 5. Run the batch file with .\
.\run.bat



# GUI Workflow Diagram (visual + explanation)

Here are three helpful views so you can present the GUI workflow clearly in your report or viva:

1. A compact **GUI screen mockup** (what the user sees)
2. A **workflow flowchart** (user actions → program steps)
3. A **component interaction diagram** (which parts talk to which)

---

## 1) GUI Screen Mockup (what the app window looks like)

```
+---------------------------------------------------------------+
|  File Encryptor - Core Java OOP Project                       |
+---------------------------------------------------------------+
| File:  [ C:\path\to\file.txt__________________________ ] [Browse] |
| Password: [ *************** ]                                   |
|                                                                |
| [ Encrypt ]    [ Decrypt ]                                      |
|                                                                |
| Progress: [##############..............]  48%                   |
| Status: Ready / Encrypting... / Decryption failed (wrong pwd)   |
+---------------------------------------------------------------+
| Tips:  • Use same password to decrypt  • Keep password safe     |
+---------------------------------------------------------------+
```

Short notes:

* File field + Browse: choose file using file chooser.
* Password: masked input.
* Encrypt / Decrypt: main actions.
* Progress bar + Status: feedback and error messages.

---

## 2) Workflow Flowchart (user actions → internal steps)

```
[User opens App UI]
         |
         v
[User clicks Browse] --> (FileChooser) --> [User selects file path] --> (UI shows path)
         |
         v
[User types password]  --------------------------+
         |                                       |
         v                                       v
[User clicks "Encrypt"]                   [User clicks "Decrypt"]
         |                                       |
         v                                       v
[UI validates inputs: file exists, pw non-empty]  [UI validates inputs: file exists, pw non-empty]
         |                                       |
         v                                       v
[Start SwingWorker background task] ------------> (Background)
         |                                       |
         v                                       v
[FileManager.encryptFile(in, out, password, cb)] [FileManager.decryptFile(in, out, password, cb)]
         |                                       |
         v                                       v
[CipherEngine.deriveKey(password)]               [CipherEngine.deriveKey(password)]
         |                                       |
         v                                       v
[Encrypt stream: XOR data with key]              [Read header, XOR data with key]
         |                                       |
         v                                       v
[Write header + encrypted bytes to outFile]      [Validate header checksum]
         |                                       |
         v                                       v
[On progress update -> call cb.onProgress()]     [If header invalid -> throw InvalidPasswordException]
         |                                       |
         v                                       v
[Background task completes successfully]         [If success -> write decrypted file]
         |                                       |
         v                                       v
[UI updates status + enable buttons]             [UI shows status or error dialog]
```

Key points:

* UI delegates heavy work to a background thread so the UI stays responsive.
* ProgressCallback lets the background task update the progress bar & status.
* Errors (IO or invalid password) are propagated to UI to show dialogs.

---

## 3) Component Interaction Diagram (logical)

```
  +--------+         +------------+         +-------------+
  |  App   |  ---->  | FileManager|  ---->  | CipherEngine|
  |  UI    | (calls) | (streams)  | (uses)  | (crypto ops)|
  +--------+         +------------+         +-------------+
     |                   |                      |
     |  ProgressCallback |                      |
     | <---------------- |                      |
     |                   |                      |
     |  shows status,    |                      |
     |  enables buttons  |                      |
     v                   v                      v
[User]                [File System]           [MessageDigest / CRC]
```

Explanation:

* **App UI**: collects input, shows progress & messages, starts the background task.
* **FileManager**: reads/writes file data in chunks, computes/validates header, calls CipherEngine to transform bytes.
* **CipherEngine**: derives key from password, XORs data, creates header and validates checksum.

---

## Error & Edge-case Handling (how UI should react)

* If file not found or unreadable → show error dialog: “File not found / permission denied.”
* If password empty → show warning: “Enter a password.”
* If decryption checksum fails → show error dialog: “Incorrect password or corrupted file.”
* If IO failure during write → show error: “Unable to save output — check disk space/permissions.”
* Buttons disabled during operation; re-enabled on finish/failure.

---

## Sequence Example (user encrypts then decrypts)

1. Open app → click **Browse** → choose `notes.txt`.
2. Type `myPass123` in Password.
3. Click **Encrypt** → UI disables buttons, status shows “Encrypting…”, progress updates.
4. On success: status shows `Done. Output: notes.txt.enc`.
5. To decrypt: Browse `notes.txt.enc`, type `myPass123`, click **Decrypt** → status shows “Decrypting…”.
6. If key matches, `notes.txt` restored (or `notes.dec` created). If not, error dialog shown.

---

If you want, I can:

* Convert the flowchart into a printable PNG or SVG (I can generate assets if you ask).
* Prepare a slide-ready diagram (title + diagram + bullet points) for your viva.
  Which one would you like next?
  
