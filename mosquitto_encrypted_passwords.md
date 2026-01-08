## Create an encrypted password file

Mosquitto uses the `mosquitto_passwd` utility to create and manage encrypted password files.

Open Command Prompt or PowerShell and navigate to the Mosquitto installation directory:

```bash
cd "C:\Program Files\mosquitto"
```

Create an encrypted password file with a user:

```bash
mosquitto_passwd -c pwfile.txt mqtt_user
```

You'll be prompted to enter a password (e.g., `mqtt_pass`). Enter it twice.

**Important:**
- The `-c` flag **creates** a new file. Use it only for the first user.
- To add more users later, omit `-c`:

```bash
mosquitto_passwd pwfile.txt another_user
```

Copy the encrypted password file to the project:

```bash
copy "C:\Program Files\mosquitto\pwfile.txt" "C:\Users\capta\OneDrive\Documents\mqtt-basics-windows-tutorial\mosquitto\pwfile.txt"
```

**Note:** Mosquitto 2.0+ requires encrypted passwords for security. Plaintext password files are not supported.