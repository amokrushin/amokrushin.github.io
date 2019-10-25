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
