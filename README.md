##CYBERSPARK ctf 
I solved couple of crypto tasks and here are the writeups

#BIGRSA -495pts-
![Screenshot 2024-05-11 185721](https://github.com/STORTNY/ctfs/assets/78549378/18a04eaf-2fb3-418b-a6ab-f94a754e6c73)


```python
flag = open('flag.txt', 'rb').read()

p = getPrime(512)
q = getPrime(512)
n = p*q
phi = (p-1)*(q-1)

e = 65537
e1 = pow(e,e)
e2 = pow(e,e1)

c = pow(bytes_to_long(flag), e2, n)

print(f"n = {n}")
print(f"c = {c}")
print(f"phi = {phi}")
```

so given n, c and phi we still need to calculate the private key d , the trick here is that the public exponent is loo large to be computed as it is, but what we do know that e=d^-1 mod phi, so obv it can't be larger than phi.
And for our computers to calculate properly we have to calculate the main exponent in one pow() function like so 
```python
e = 65537
e2 = pow(e,pow(e,e),phi)
d=pow(e2,-1,phi)
```
solver : 
```python
from Crypto.Util.number import long_to_bytes

n = 55339911839568605434033119987234278623062104594980067237791202130538703553598242587186156138509099454400445619635644594061470675797718659928407400269551978058243845422309215484245910720668443757162324020828699580386175041359988444335094233126606419429252632754027267957490682824524591446083809482067641933921
c = 28394648362572195405154050926901886104871039494850568080337805873579271922974681293349820441677917300181035352920902658615721186023552174517928833214102936527195030009209318356079439857753022042964213177166879477425682131551361877531408060772055765524824337125480439496758501935767584364707361966390976763090
phi = 55339911839568605434033119987234278623062104594980067237791202130538703553598242587186156138509099454400445619635644594061470675797718659928407400269551963179351785922840972886056626020934304203063841142648828205963538281527472772041765227434937080954519833137577263904112829279761219086708895910252445976772
e = 65537
e2 = pow(e,pow(e,e),phi)
d=pow(e2,-1,phi)
m=pow(c,d,n)
print(long_to_bytes(m).decode())
```
flag: `Spark{RSA_Genius?!!!}`

#Kinda EZ RSA -436pts-
![Screenshot 2024-05-11 185638](https://github.com/STORTNY/ctfs/assets/78549378/3908a699-7489-4424-a59f-7f5072f0974b)

``
N = 0x9857550a7d4fad45a2a1b354bfd33c2d5912749e7f396ab36f891b7506db41e61f70a216e6421315987f8cc0b5bf6d5f81785aa908690ea3c2671001dc23eb0bca59afaee798ab4e7feda4df474f372e9927bd5d7b855af3a209908d2baad72b8c5c7fde465d155844cffd38585ad9aeb7e430561992ff543d55d3839901c0d713cce88e8880700428261e10e1cd90f5694a6ef16a3a1f61aa73f1666b60e4e83ab605c11234e429090e4ae4c5f0c761115710305648fe5890d88e0e50ff73993fce9165005c7d07e9d7446b1fa6b28acc55047f5b403f5318aa7b21bc1cd6a668d4455a473861e8f693518d1fbf36dfb2e2466c4cc194a6cded7d173f4ad073
e = 0x693ff9dd4a34f6a3cc80403f777882e73716db11684dea651be2e31c71ff480cdc30e0e6a14b74aef85f1afb4fe20d953806ca7c588818d94b47a16441d512c6ba1f922b3569d799fc98fa641a244f4960eeac517f993152a6da7ca3b9226466437e5bd1e94d56b6ce7216f09719b76cef341e5b0da9f6b35cc171404e575d6811a4bd90f8b250f75ec7411c6f7576f2fc9a4060554330abc307540e15cdea3b7416f185dee4b7359cee7d970059357d47c2718987804f41fd8507646fd510a1a2ef3e1adf7e7bdd7ed8a2a923dee116d6a20d589a716b8ad7cd2bd3171c6e2f0448e09b02fbdc3e25957b8d84805b95ba131468e6774baa1e0b7522f05d42d3
c = 0xb1cb17d7e9dd7b4f9c31392201d0da9586fccc6b033f918e2defe36082fb7c4d205614f3e7fc1eed03a950b04a74a958198e19f91f377c1bdce8f01e0dcc1d120e4d9f7ef9c7e329ca87a26364ee58a019ce4f42836e8ebe86fe05e1704de8f016c5322fd991bda5975e10828f531fa0ce0520a9c4539cf4626c6a19326d9d7bcbb179deb6f8c10f74a7e21fa0c1f044c9fc6144f3e46e3bda23f0385bf24e460ad43c5f7d14976ab75f98b1091c48d072c1129db50232d562e0e82b6373cceeb3a7eb895a87d120a1c2c5bd71420a77d0c86b2ac7364ee1e3702ea784409cf820008544dba21b1f967d6fda1ca3fc1885e7f24852d837b698ed17ad8db2888
``

looking at how big those integers are, the first thing I thought of is the wiener attack 
solver : 

```python
import owiener
from Crypto.Util.number import long_to_bytes

N = 0x9857550a7d4fad45a2a1b354bfd33c2d5912749e7f396ab36f891b7506db41e61f70a216e6421315987f8cc0b5bf6d5f81785aa908690ea3c2671001dc23eb0bca59afaee798ab4e7feda4df474f372e9927bd5d7b855af3a209908d2baad72b8c5c7fde465d155844cffd38585ad9aeb7e430561992ff543d55d3839901c0d713cce88e8880700428261e10e1cd90f5694a6ef16a3a1f61aa73f1666b60e4e83ab605c11234e429090e4ae4c5f0c761115710305648fe5890d88e0e50ff73993fce9165005c7d07e9d7446b1fa6b28acc55047f5b403f5318aa7b21bc1cd6a668d4455a473861e8f693518d1fbf36dfb2e2466c4cc194a6cded7d173f4ad073
e = 0x693ff9dd4a34f6a3cc80403f777882e73716db11684dea651be2e31c71ff480cdc30e0e6a14b74aef85f1afb4fe20d953806ca7c588818d94b47a16441d512c6ba1f922b3569d799fc98fa641a244f4960eeac517f993152a6da7ca3b9226466437e5bd1e94d56b6ce7216f09719b76cef341e5b0da9f6b35cc171404e575d6811a4bd90f8b250f75ec7411c6f7576f2fc9a4060554330abc307540e15cdea3b7416f185dee4b7359cee7d970059357d47c2718987804f41fd8507646fd510a1a2ef3e1adf7e7bdd7ed8a2a923dee116d6a20d589a716b8ad7cd2bd3171c6e2f0448e09b02fbdc3e25957b8d84805b95ba131468e6774baa1e0b7522f05d42d3
c = 0xb1cb17d7e9dd7b4f9c31392201d0da9586fccc6b033f918e2defe36082fb7c4d205614f3e7fc1eed03a950b04a74a958198e19f91f377c1bdce8f01e0dcc1d120e4d9f7ef9c7e329ca87a26364ee58a019ce4f42836e8ebe86fe05e1704de8f016c5322fd991bda5975e10828f531fa0ce0520a9c4539cf4626c6a19326d9d7bcbb179deb6f8c10f74a7e21fa0c1f044c9fc6144f3e46e3bda23f0385bf24e460ad43c5f7d14976ab75f98b1091c48d072c1129db50232d562e0e82b6373cceeb3a7eb895a87d120a1c2c5bd71420a77d0c86b2ac7364ee1e3702ea784409cf820008544dba21b1f967d6fda1ca3fc1885e7f24852d837b698ed17ad8db2888


d = owiener.attack(e, N)
if d:
    m = pow(c, d, N)
    flag = long_to_bytes(m)

    print(flag)
else:
  print('attack failed')
```
flag=`Spark{ab4b5f8a879411e556590886f28090fa990aaa0f}`

#RSA101 -449pts-
![Screenshot 2024-05-11 185659](https://github.com/STORTNY/ctfs/assets/78549378/430a7cbf-5571-4460-bd4a-4f7785d79e92)


task file : 
```python
from Crypto.Util.number import *

flag = open("flag.txt","rb").read()
flag = bytes_to_long(flag)
e = 65537
p = getPrime(512)
n = p*p*p
c = pow(flag, e, n)

print (f"n = {n}")
print (f"c = {c}")
```
so n here is p cube 
so the  Euler's totient function phi for a prime power argument is :
![Screenshot 2024-05-11 215241](https://github.com/STORTNY/ctfs/assets/78549378/ff7c698c-e03b-4f08-b094-0feb9fd5ac79)


in this case a=3
so phi(n)=phi(p**3)=p^2*(p-1)

solver : 
```python
from gmpy2 import iroot
from Crypto.Util.number import long_to_bytes

n = 880674765938292988253202733068999441370225980493082250293770108839238525173140672028586926059569234956086334098934256319958427223211981609900174353433233635531420938608337844907638761397741872915869057101536297717494270382023262734939463053245168089144322878569577839083545388805481696553406917669778617558497888334679076374709271281550795111553114600031758193437040624986352761831630272858688309123381388470862086985921176162048792009987228215795441625521616137
c = 645056485211620346978535666785604975280754872647179594109641305091423921165155722104664444088687235839128248600148143315460009492054409130436797788265699866177932125389944332649532462834332178279205964361079082767369331533906053438874273146377191231020779328010810371540314926752311621146795928267762979331803528053448822028978261770189358788240873445590240544604410075899620636633697878839846303267286114971103304095344337085090641058311308066880525320223837207
p=iroot(n,3)[0]
e = 65537

phi = p**2*(p-1)
d=pow(e,-1,phi)
m=pow(c,d,n)
print(long_to_bytes(m).decode())
```
 flag=`Spark{N0t_Y0Ur_R3GUL4R_T0Ti3Nt}`

#SUDO -500pts- 
![image](https://github.com/STORTNY/ctfs/assets/78549378/988776c4-2769-4fd7-a6d6-0a4ec8fdb61d)

I was surprised this task had only 2 solves, would've been so easy if u just knew how rsa works
so the cipher text istelf is equal to m^e mod n
if the e is too small like 3 in our case, we can take the e'th root of c , or if m^e > N, we can keep adding the modulus n until we get to the correct value of m which is the cube.root(c+k*n)

for this chall I did brute force it in a range of 20 andi got the flag 
solver : 
```python
from Crypto.Util.number import  long_to_bytes
from gmpy2 import iroot


c = 21219812398146774982012671975172698983037477798626924660438494599504149529737723511870861195181900330890516887624967642878410986845215343561744768345349889381245089393834161962270484578831648076878935348353324755624
n = 21219812398146774982012671975172698983037477798626924660438494599504149529737723511870861195181900330890516887624967642878410994945215343561744849345324532423485089393834161962468778767831648076897857676678645723678
e=3
for i in range(20):

    m = iroot(c+(i*n), e)

    print(long_to_bytes(m[0]))

```
output  :
``
<--*snip*-->
b'L\xbb\xe3\xe4\x1a.M\n~,\x91\xd8\x10\x17&\xe8Y\x9d\x80N\xe5\xb4\xd8\xdb\xb4\xeeAW$\xdb'
b'P9\xff\xf8\x03+\xdc\xb9\xad%\xc3\x8ep\xa4{\xe0\xaf\x92|\x03\xa0b\x10\x85\xeb9X\xaf\xae\xa5'
b'Spark{L00piN6_4R0uND_M0DuLU5}\t'
b'Vk\xe3\xc3k\xe4\x8d\xc9g\xa0\xfe\xce\x1f\x8c&b\x99"\xd4\x8c1\x02V\xd6\xf2\xcdv7\x8d\''
<--*snip*-->
``
flag=`Spark{L00piN6_4R0uND_M0DuLU5}`



That's it for now :=)
There were other fun tasks that I enjoyed solving but I'll probably focus on crypto writeups ftm.
Thanks for reading cya!






