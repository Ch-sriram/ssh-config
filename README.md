# Faster `SSH` Configuration

- In case there's a wait time whenever you `ssh` into your server, the chances are that it stems primarily from authentication order. This behaviour can be fixed by adding some minor configuration lines to your `~/.ssh/config` file.

## `ssh` Server Configuration

1. In your server, create a `.ssh` directory at `$HOME`:
   ```sh
   mkdir -p $HOME/.ssh
   ```
2. In your client, copy the contents of `~/.ssh/id_rsa.pub` or `~/.ssh/id_ed25519.pub` or any file's contents which has `.pub` as the extension.
   - If `xclip` is installed, you can run the following command:
     ```sh
     # Change the file id_rsa.pub as per your convenience, to id_ed25519.pub or any other .pub file
     cat ~/.ssh/id_rsa.pub | xclip -selection clipboard
     ```
3. In your server, paste the public key content into `~/.ssh/authorized_keys` file, at the bottom.

## `ssh` Client Configuration

1. Create `~/.ssh/config` if it doesn't exist, and copy the file [`config.fast-login.dist`](./config.fast-login.dist):
   > **NOTE**: If `~/.ssh/config` already exists, then just copy the contents of [`config.fast-login.dist`](./config.fast-login.dist) to `~/.ssh/config`.
   ```sh
   cp ./config.fast-login.dist ~/.ssh/config
   ```
2. Open `~/.ssh/config` file, and make changes to the following fields:
   1. `Host`: By default set to `devbox`, means that there's a IP alias for `devbox` in `/etc/hosts` file. If not, please add there.
   1. `host_ip`: Actual IP of the host. Ex: `10.1.2.3`.
   1. `username_in_host`: Actual username in the host. `csriram`.
   1. `IdentityFile <PATH>`: Path can be changed to the actual private key path. By default, `~/.ssh/id_rsa` is configured.
3. Add the hostname for the IP in `/etc/hosts`:
   > **NOTE**: Take a backup of original `/etc/hosts` before doing any of the following, in case the config gets messed up somehow.
   > ```sh
   > # Might require superuser (sudo) permissions
   > cp /etc/hosts /etc/hosts.bak
   > ```
   ```sh
   # Replace <host_ip> with server's IP
   # Replace <Host> with the Host mentioned in ~/.ssh/config
   sudo echo "<host_ip>  <Host>" >> /etc/hosts
   ```
   Example: `sudo echo "10.1.2.3    devbox" >> /etc/hosts

