# Python Virtual Environments

Virtual environments are isolated environments that allow you to install and manage dependencies without interfering with system-wide packages.

They are useful for isolating dependencies and sharing or moving projects between different systems.

## Creating a Virtual Environment

Built-in module `venv` can be used to create a new virtual environment.

```shell
$ python3 -m venv MyProject
```

It creates a *MyProject* directories with items.

```shell
$ ls MyProject/
bin  include  lib  lib64  pyvenv.cfg
```

## Activating and Deactivating a Virtual Environment

Virtual environment can be activated by using `source` command on macOS and Linux.

```shell
$ source MyProject/bin/activate
(MyProject) $
```

On Windows, we can execute the following.

```shell
$ .\MyProject\Scripts\activate
(MyProject) $
```

`(MyProject)` before the prompt indicates that the virtual environment has been activated.

Once it has been activated, `deactivate` command can be used to deactivate the environment.

```shell
(MyProject) $ deactivate
$
```

## Checking and Installing Packages

When a virtual environment is first created, there are only built-in and standard library modules.

We can list all install packages using `pip freeze` command. Currently, the output will be empty.

```shell
(MyProject) $ pip freeze
(MyProject) $
```

Let's create a simple python AES encryption program inside MyProject using [PyCryptodome](https://pycryptodome.readthedocs.io/en/latest/).

```python
# program.py

from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad

def encrypt_message(message, key):
    iv = get_random_bytes(AES.block_size)
    cipher = AES.new(key, AES.MODE_CBC, iv)

    padded_message = pad(message.encode(), AES.block_size)
    ciphertext = cipher.encrypt(padded_message)

    return iv, ciphertext

message = "Hello World"
key = get_random_bytes(16)
iv, ciphertext = encrypt_message(message, key)

# Printing the IV and the ciphertext
print("IV:", iv)
print("Ciphertext:", ciphertext)
```

Since, `MyProject` is a new virtual environment, `PyCryptodome` will not be there.

```shell
(MyProject) $ python3 program.py
Traceback (most recent call last):
  File "/mnt/c/Users/Dell/Desktop/Test/MyProject/program.py", line 1, in <module>
    from Crypto.Cipher import AES
ModuleNotFoundError: No module named 'Crypto'
```

It can be installed using `pip` just like in global Python environment.

```shell
(MyProject) $ pip install pycryptodome
[...]
(MyProject) $ pip freeze
pycryptodome==3.20.0
(MyProject) $ python3 program.py
IV: b'a\xd2\xd9kU\xd0\xbf\xf8\xc9\x04\xe7)\xe9\x8c\x10\xa7'
Ciphertext: b'\xc9iZ\xaa\xe0\xc6\xa4\x0e\x14\xfc\x0fOy h)'
```

Now, `PyCryptodome` package is in the environment and the program is now working.

## Requirements

`requirements.txt` file can be used to specify the dependencies for the project. You can generate a `requirements.txt` file containing the currently installed packages in the environment using following command.

```shell
(MyProject) $ pip freeze > requirements.txt
(MyProject) $ cat requirements.txt
pycryptodome==3.20.0
```

Now, the project can be shared and the others can install the dependencies listed in the `requirements.txt` using following command.

```shell
$ pip install -r requirements.txt
```

## Links for learning more

- [Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)

- [What does 'source' do?](https://superuser.com/questions/46139/what-does-source-do)

- [pip](https://pip.pypa.io/en/stable/)

- [pip freeze](https://pip.pypa.io/en/stable/cli/pip_freeze/)