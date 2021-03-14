### Вычисление хеша файла

```
# 1.2.643.2.2.9 for GOST R 34.11-94
# 1.2.643.7.1.1.2.2 for GOST R 34.11-2012 256 bit
# 1.2.643.7.1.1.2.3 for GOST R 34.11-2012 512 bit

$ cryptcp -hash -provtype 75 -hashAlg '1.2.643.7.1.1.2.2' -hex Lorem\ Ipsum.txt
$ cat Lorem\ Ipsum.txt.hsh
F3C932C069E28BEDD1E260B20B4130260C1769DEF07C2E6095F466B02C6E608B

$ cpverify -mk -alg GR3411_2012_256 Lorem\ Ipsum.txt
F3C932C069E28BEDD1E260B20B4130260C1769DEF07C2E6095F466B02C6E608B
```

## Установка openssl с поддержкой алгоритмов ГОСТ

```
apt install openssl
apt install libengine-gost-openssl1.1
```

`/etc/ssl/openssl.cnf`
```
# перед [ new_oids ]
openssl_conf=openssl_def
...
# в конец файла
[openssl_def]
engines = engine_section

[engine_section]
gost = gost_section

[gost_section]
engine_id = gost
dynamic_path = /usr/lib/x86_64-linux-gnu/engines-1.1/gost.so
default_algorithms = ALL
```

Пример использования
https://github.com/gost-engine/engine/blob/master/README.gost

```
openssl dgst -md_gost12_256 test.txt
```
