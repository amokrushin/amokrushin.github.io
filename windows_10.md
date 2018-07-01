## FAQ

- Open *.pem with `Crypto Shell Extensions`
  > Write the following to a `pem-as-cert.reg` file:
  > ```
  > Windows Registry Editor Version 5.00
  > 
  > [HKEY_CLASSES_ROOT\.pem]
  > @="CERFile"
  > "Content Type"="application/x-x509-ca-cert"
  > ```
  > Double click on it and then you have .pem linked to Crypto Shell extension.
  > [Source](https://social.technet.microsoft.com/Forums/ie/en-US/b83125dd-5152-4044-9bf2-a1847d4e7d8a/default-program-crypto-shell-extensions?forum=w7itprosecurity)
