# -SSH-Login-with-Password-and-Key-Generation_linux

# SSH Login with Password and Key Generation

## Key Information

- **Package Name:** openssh
- **Service Name:** sshd
- **Port Number:** 22
- **Server Config Path:** /etc/ssh/sshd_config
- **Client Config Path:** /etc/ssh/ssh_config
- **Login Types:** Password & Key Generation
- **Key Gen Path:** /userhomedir/.ssh/

## Key Commands

- **Generate SSH Key Pair:**
    ```bash
    ssh-keygen -t rsa -b 2048
    ```
- **Copy Public Key to Server:**
    ```bash
    ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.100.2
    ```
- **Log in Using SSH Key:**
    ```bash
    ssh -i /root/.ssh/id_rsa root@192.168.100.2
    ```
- **Edit SSH Server Config to Disable Password Authentication:**
    ```bash
    vi /etc/ssh/sshd_config
    ```
    Add or update the following lines:
    ```text
    PubkeyAuthentication yes
    PasswordAuthentication no
    ```
- **Restart SSH Service:**
    ```bash
    systemctl restart sshd
    ```
- **Check SSH Listening Port:**
    ```bash
    netstat -ant | grep 22
    ```

## Step-by-Step Procedure

### 1. Generate SSH Key Pair on the CentOS 7 Client

1. **Log in to the CentOS 7 client machine.**
2. **Generate an SSH key pair:**
    ```bash
    ssh-keygen -t rsa -b 2048
    ```
    - Press `Enter` to save the key in the default location (`/root/.ssh/id_rsa`).
    - Optionally, enter a passphrase (or leave it empty for no passphrase).

### 2. Copy Public Key to the Ubuntu Server

1. **Copy the public key to the Ubuntu server:**
    ```bash
    ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.100.2
    ```
    - Accept the server's fingerprint if prompted.
    - Enter the password for the `root` user on the Ubuntu server when prompted.

### 3. Verify SSH Key-Based Login

1. **Log in to the Ubuntu server using the private key:**
    ```bash
    ssh -i /root/.ssh/id_rsa root@192.168.100.2
    ```
    - This should log you in without asking for a password.

### 4. Restrict User to Key-Based Login Only on CentOS 7

1. **Edit the SSH server configuration file:**
    ```bash
    vi /etc/ssh/sshd_config
    ```
2. **Update or add the following lines:**
    ```text
    PubkeyAuthentication yes
    PasswordAuthentication no
    ```
3. **Save and close the file.**

4. **Restart the SSH service to apply changes:**
    ```bash
    systemctl restart sshd
    ```
    
5. **Verify SSH server is listening on port 22:**
    ```bash
    netstat -ant | grep 22
    ```
    - You should see an output similar to:
      ```text
      tcp        0      0 0.0.0.0:22            0.0.0.0:*               LISTEN
      ```

## Troubleshooting

- **Host Key Verification Failed:**
    - Remove the existing entry from `~/.ssh/known_hosts` and try again:
      ```bash
      ssh-keygen -R 192.168.100.2
      ```
- **Permission Denied (publickey):**
    - Ensure the permissions of `.ssh` directory and files are correct:
      ```bash
      chmod 700 ~/.ssh
      chmod 600 ~/.ssh/id_rsa
      chmod 644 ~/.ssh/id_rsa.pub
      ```

By following these steps, you will set up SSH key-based authentication and restrict a user to only use key-based login on CentOS 7.
