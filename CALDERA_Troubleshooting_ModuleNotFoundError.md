# CALDERA Troubleshooting: ModuleNotFoundError

## Symptoms

Running:

``` bash
python3 server.py --insecure --build
```

produced:

``` text
ModuleNotFoundError: No module named 'aiohttp_apispec'
```

or

``` text
ModuleNotFoundError: No module named 'rich'
```

## Root Cause

The virtual environment was not activated, so CALDERA used:

``` text
/usr/bin/python
/usr/bin/pip
```

instead of:

``` text
/root/CalderaVENV/caldera/venv/bin/python
/root/CalderaVENV/caldera/venv/bin/pip
```

## Resolution

``` bash
python3 -m venv venv
source venv/bin/activate
which python
which pip
pip install --upgrade pip
pip install -r requirements.txt
python server.py --insecure --build
```

## Verification

``` bash
which python
which pip
```

Expected:

``` text
/root/CalderaVENV/caldera/venv/bin/python
/root/CalderaVENV/caldera/venv/bin/pip
```

## Conclusion

The issue was resolved by creating and activating the Python virtual
environment and reinstalling all dependencies into that environment.
