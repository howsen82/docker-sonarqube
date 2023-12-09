# Securing credential in Vault

**Install Vault**

You will be using a tool called Vault from Hashicorp. Before you can use vault on any system, you must install it.

```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install vault
```

You can verify that it was installed properly by running the `vault` command in a terminal:

```
vault
```

**Setting up the Dev Server**

Now, you will start the Dev Server for Vault just to get an idea of what the functionalities look like. In reality, this is not very secure, but it is useful for exploring the tool locally. Nonetheless, all data is encrypted and stored in-memory for the Dev Server.

Use the `vault server` command with the `-dev` flag to run the vault server in development mode:

```
vault server -dev
```

**Set environment variable**

Next, you must open a new terminal using `Terminal > New Terminal `from the top menu, and run the following shell command to specify port 8200 for Vault:

```
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN="YOUR ROOT TOKEN HERE"
```

Use the `vault status` command to check that the server is running:

```
vault status
```

## Access the Vault UI

There are various ways to access Vault. One user-friendly way is to launch the web User Interface (UI) on your local host.

This will open a new tab in this IDE for the application. You will be presented with a logon page. From here, you can input the root token that you copied to the `VAULT_TOKEN` environment variable to login.

You can display your token from a terminal with:

```
echo $VAULT_TOKEN
```

**Vault web UI**

Once you are logged in, you can access secrets stored in secret/ by clicking the link:

# Installing HVAC

By default, Vault's dev server includes KV v2 Secrets Engines at the path `secret/`, storing secrets within its configured physical storage. Secrets are encrypted before writing to backend storage, so it can never decrypt these values without Vault.

As demonstrated, you will learn to read and write secrets to the server using Python with the help of the `hvac` library. Before we can do this, we must install the Python package `hvac` using the Python Package Manager (pip)

Use the `pip` command to install the `hvac` package with the following command:

```
python3 -m pip install hvac
```

> Note: You can also use the Vault command line instead of the Python API to manage secrets. Learn more about it here.

#### Write Secret to Vault

You are now ready to start using vault by writing secrets to it.

Before you can start, you must first, obtain the Python file that will be used to write, read, and delete secrets from Vault:

1. Use the following wget command to retrieve the program read_write_vault.py:
   
   ```
   wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-CD0267EN-SkillsNetwork/labs/module3/read_write_vault.py
   ```

2. Let's open the file in the IDE to take a look at it:

**Writing a secret**

Use `python3` to call the `read_write_vault.py` program passing in the function name `write_secret` with the parameters myapp alice mypassword:

```
python3 read_write_vault.py write_secret myapp alice mypassword
```

This will set the path as `secret/myapp` and write the secret key/value pair `alice=mypassword `in there.

#### Read Secret from Vault

You are going to use the same syntax in the terminal to read a secret from the path `secret/myapp`.

1. Use `python3` to run the `read_write_vault.py` program calling the `read_secret` function passing in the parameter `myapp`:

```
python3 read_write_vault.py read_secret myapp
```

#### Delete Secret from Vault

1. Use `python3` to run the `read_write_vault.py` program calling the `delete_secret` function passing in the parameter `myapp`:

```
python3 read_write_vault.py delete_secret myapp
```