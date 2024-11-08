# CRYPTOHACK 
## Modular Exponentiation
### Problem 
All operations in RSA involve modular exponentiation.

Modular exponentiation is an operation that is used extensively in cryptography and is normally written like: 210mod  17210mod17

You can think of this as raising some number to a certain power (210=1024210=1024), and then taking the remainder of the division by some other number (1024mod  17=41024mod17=4). In Python there's a built-in operator for performing this operation: pow(base, exponent, modulus).

In RSA, modular exponentiation, together with the problem of prime factorisation, helps us to build a "trapdoor function". This is a function that is easy to compute in one direction, but hard to do in reverse unless you have the right information. It allows us to encrypt a message, and only the person with the key can perform the inverse operation to decrypt it.

To grab the flag, find the solution to 10117mod  2266310117mod22663
### solution 
pow(101, 17, 22663)
flag: 19906

## Public Keys
### Problem 
15 pts · 10963 Solves
RSA encryption is modular exponentiation of a message with an exponent ee and a modulus NN which is normally a product of two primes: N=p⋅qN=p⋅q.

Together, the exponent and modulus form an RSA "public key" (N,e)(N,e). The most common value for ee is 0x10001 or 6553765537.

"Encrypt" the number 1212 using the exponent e=65537e=65537 and the primes p=17p=17 and q=23q=23. What number do you get as the ciphertext? 
### solution 
To encrypt the number 1212 using RSA with the given values, we will follow the same steps as before:

    Calculate nn:
    n=p×qn=p×q
    Here, p=17p=17 and q=23q=23:
    n=17×23=391
    n=17×23=391

    Use the public exponent ee:
    The public exponent is e=65537e=65537.

    Message mm:
    The message you want to encrypt is m=12.

    Encrypt the message:
    The ciphertext cc is calculated using the formula:
    c=memod  n
    c=memodn

    Since 12<39112<391, we can calculate:
    c=1265537mod  391
    c=1265537mod391

Calculation

Let's perform this calculation to find the ciphertext.

The ciphertext obtained by encrypting the number 1212 using the exponent e=65537e=65537 and the primes p=17p=17 and q=23q=23 is 301. ​
​
## Euler's Totient
### Problem 
RSA relies on the difficulty of the factorisation of the modulus N. If the prime factors can be deduced, then we can calculate the Euler totient of N and thus decrypt the ciphertext.

Given N=p⋅qN=p⋅q and two primes:

p = 857504083339712752489993810777
q = 1029224947942998075080348647219

What is Euler's totient ϕ(N)?
### Solution 
To calculate Euler's totient ϕ(N)ϕ(N) for two very long prime numbers pp and qq, you can follow these steps:
Steps to Calculate ϕ(N)ϕ(N)

    Identify the Primes: Ensure you have your two long prime numbers, pp and qq.

    Subtract One from Each Prime: Calculate p−1p−1 and q−1q−1.

    Multiply the Results: The totient ϕ(N)ϕ(N) is then calculated using the formula:
    ϕ(N)=(p−1)×(q−1)
thus
X-1=857504083339712752489993810776
and Y-1=1029224947942998075080348647218 hence the Euler Totient is 882,564,595,536,224,140,639,625,987,657,529,300,394,956,519,977,044,270,821,168
## Private Keys
### Problem 
The private key dd is used to decrypt ciphertexts created with the corresponding public key (it's also used to "sign" a message but we'll get to that later).

The private key is the secret piece of information, or "trapdoor", which allows us to quickly invert the encryption function. If RSA is implemented well, if you do not have the private key the fastest way to decrypt the ciphertext is to factorise the modulus which is very hard to do for large integers.

In RSA, the private key is the modular multiplicative inverse of the exponent ee modulo ϕ(N)ϕ(N), Euler's totient of NN.

Given the two primes:

p = 857504083339712752489993810777
q = 1029224947942998075080348647219

and the exponent e=65537e=65537, what is the private key d≡e−1mod  ϕ(N)d≡e^−1modϕ(N)? 
### Solution 
i asked an LLM to create a python code to calculate the inverse of e and mod it with Euler totient and then  i executed it to find the KEY=121832886702415731577073962957377780195510499965398469843281
### python code 
```
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
e = 65537
phi_N = (p - 1) * (q - 1)
def extended_gcd(a, b):
    if a == 0:
        return b, 0, 1
    gcd, x1, y1 = extended_gcd(b % a, a)
    x = y1 - (b // a) * x1
    y = x1
    return gcd, x, y
def mod_inverse(e, phi):
    gcd, x, y = extended_gcd(e, phi)
    if gcd != 1:
        raise Exception('Inverse does not exist')
    else:
        return x % phi
d = mod_inverse(e, phi_N)
print("The private key d is:", d)
```
## RSA Decryption
### Problem 
I've encrypted a secret number for your eyes only using your public key parameters:

N = 882564595536224140639625987659416029426239230804614613279163
e = 65537

Use the private key that you found for these parameters in the previous challenge to decrypt this ciphertext:

c = 77578995801157823671636298847186723593814843845525223303932 
### Solution 
``` # Given values
N = 882564595536224140639625987659416029426239230804614613279163
c = 77578995801157823671636298847186723593814843845525223303932

# Assuming `d` is the private key from previous calculations
# You need to set `d` to the value you obtained earlier

# Example d from previous challenge (replace with actual calculated value)
# d = [replace this with the actual private key calculated]
# For demonstration, let's assume we found it to be some value like below:
d = 121832886702415731577073962957377780195510499965398469843281  # Replace with actual d

# Decrypt the ciphertext
m = pow(c, d, N)

# Output the result
print("The decrypted plaintext (secret number) is:", m)
```
KEY=13371337
## RSA Signatures
### Problem 
How can you ensure that the person receiving your message knows that you wrote it?

You've been asked out on a date, and you want to send a message telling them that you'd love to go, however a jealous lover isn't so happy about this.

When you send your message saying yes, your jealous lover intercepts the message and corrupts it so it now says no!

We can protect against these attacks by cryptographically signing the message.

Imagine you write a message mm. You encrypt this message with your friend's public key: c=me0mod  N0c=me0​modN0​.

To sign this message, you calculate the hash of the message: H(m)H(m) and "encrypt" this with your private key: S=H(m)d1mod  N1S=H(m)d1​modN1​.

In real cryptosystems, it's best practice to use separate keys for encrypting and signing messages.

Your friend can decrypt the message using their private key: m=cd0mod  N0m=cd0​modN0​. Using your public key they calculate s=Se1mod  N1s=Se1​modN1​.

Now by computing H(m)H(m) and comparing it to ss: assert H(m) == s, they can ensure that the message you sent them, is the message that they received! As long as your private key is safe, no one else could have signed this message!

Sign the flag crypto{Immut4ble_m3ssag1ng} using your private key and the SHA256 hash function.

The output of the hash function needs to be converted into a number that can be used with RSA math. Remember the helpful bytes_to_long() function that can be imported from Crypto.Util.number.


## Square Eyes
### Problem:
It was taking forever to get a 2048 bit prime, so I just generated one and used it twice. it has a text file attached to it with n e and ct 
N - The modulus of the RSA encryption, given as a large number.
ee - The public exponent (65537), commonly used in RSA.
ct - The ciphertext, representing the encrypted message.
Step 1: Recognize N=p2N=p2

The hint suggests using the same prime twice to generate NN, so:
N=p×p=p2
N=p×p=p2

Thus, we can recover pp by taking the square root of NN.
Step 2: Compute pp

Using the integer square root, we calculate:
p=isqrt(N)
p=isqrt(N)

Now we have pp, a factor of NN.
Step 3: Calculate ϕ(N)ϕ(N)

In RSA, the totient ϕ(N)ϕ(N) is calculated differently based on the structure of NN:

    For distinct primes pp and qq, ϕ(N)=(p−1)(q−1)ϕ(N)=(p−1)(q−1).
    When N=p2N=p2, ϕ(N)=p×(p−1)ϕ(N)=p×(p−1).

Here, we calculate:
ϕ(N)=p×(p−1)
ϕ(N)=p×(p−1)
Step 4: Find dd

To decrypt, we need the private exponent dd, which is the modular inverse of ee with respect to ϕ(N)ϕ(N):
d=e−1mod  ϕ(N)
d=e−1modϕ(N)

Using the inverse function from Crypto.Util.number, we compute dd.
Step 5: Decrypt the Ciphertext

With dd known, we can decrypt the ciphertext:
m=ctdmod  N
m=ctdmodN

where mm is the decrypted message in integer form.
Step 6: Convert to Bytes

Finally, we convert mm back into a readable format using long_to_bytes(m). This gives us the plaintext flag, which may contain ASCII or UTF-8 encoded text.
for this I used an LLM to create a python code to decrypt the value of ct to obtain the flag:
```
from Crypto.Util.number import long_to_bytes, inverse
import math

# Given parameters
N = 535860808044009550029177135708168016201451343147313565371014459027743491739422885443084705720731409713775527993719682583669164873806842043288439828071789970694759080842162253955259590552283047728782812946845160334801782088068154453021936721710269050985805054692096738777321796153384024897615594493453068138341203673749514094546000253631902991617197847584519694152122765406982133526594928685232381934742152195861380221224370858128736975959176861651044370378539093990198336298572944512738570839396588590096813217791191895941380464803377602779240663133834952329316862399581950590588006371221334128215409197603236942597674756728212232134056562716399155080108881105952768189193728827484667349378091100068224404684701674782399200373192433062767622841264055426035349769018117299620554803902490432339600566432246795818167460916180647394169157647245603555692735630862148715428791242764799469896924753470539857080767170052783918273180304835318388177089674231640910337743789750979216202573226794240332797892868276309400253925932223895530714169648116569013581643192341931800785254715083294526325980247219218364118877864892068185905587410977152737936310734712276956663192182487672474651103240004173381041237906849437490609652395748868434296753449
e = 65537
ct = 222502885974182429500948389840563415291534726891354573907329512556439632810921927905220486727807436668035929302442754225952786602492250448020341217733646472982286222338860566076161977786095675944552232391481278782019346283900959677167026636830252067048759720251671811058647569724495547940966885025629807079171218371644528053562232396674283745310132242492367274184667845174514466834132589971388067076980563188513333661165819462428837210575342101036356974189393390097403614434491507672459254969638032776897417674577487775755539964915035731988499983726435005007850876000232292458554577437739427313453671492956668188219600633325930981748162455965093222648173134777571527681591366164711307355510889316052064146089646772869610726671696699221157985834325663661400034831442431209123478778078255846830522226390964119818784903330200488705212765569163495571851459355520398928214206285080883954881888668509262455490889283862560453598662919522224935145694435885396500780651530829377030371611921181207362217397805303962112100190783763061909945889717878397740711340114311597934724670601992737526668932871436226135393872881664511222789565256059138002651403875484920711316522536260604255269532161594824301047729082877262812899724246757871448545439896

# Step 1: Compute p (assuming N = p^2)
p = math.isqrt(N)

# Step 2: Compute phi(N) = p * (p - 1)
phi_N = p * (p - 1)

# Step 3: Calculate d (modular inverse of e mod phi(N))
d = inverse(e, phi_N)

# Step 4: Decrypt the ciphertext
m = pow(ct, d, N)

# Step 5: Convert to plaintext
plaintext = long_to_bytes(m)

print("Decrypted flag:", plaintext.decode(errors='ignore'))

```
FLAG=crypto{squar3_r00t_i5_f4st3r_th4n_f4ct0r1ng!}
## Working with Fields
### problem:
The set of integers modulo NN, together with the operations of both addition and multiplication forms a ring Z/NZZ/NZ. Fundamentally, this means that adding or multiplying any two elements in the set returns another element in the set.

When the modulus is prime: N=pN=p, we are additionally guaranteed a multiplicative inverse of every element in the set, and so the ring is promoted to a field. In particular, we refer to this field as a finite field denoted FpFp​.

The Diffie-Hellman protocol works with elements of some finite field FpFp​, where the prime modulus is typically very large (thousands of bits), but for the following challenges we will keep numbers smaller for compactness.

Given the prime p=991p=991, and the element g=209g=209, find the inverse element d=g−1 d=g−1 such that g⋅d mod  991=1 g⋅d mod 991=1. 
### solution:
```
d = pow(209, -1, 991)
print(d)
```
Flag= 569
## Generators of Groups
Every element of a finite field FpFp​ can be used to make a subgroup HH under repeated action of multiplication. In other words, for an element gg the subgroup H=⟨g⟩={g,g2,g3,…}H=⟨g⟩={g,g2,g3,…}

A primitive element of FpFp​ is an element whose subgroup H=Fp∗H=Fp∗​, i.e., every non-zero element of FpFp​, can be written as gnmod  pgnmodp for some integer nn. Because of this, primitive elements are sometimes called generators of the finite field.

For the finite field with p=28151p=28151 find the smallest element gg which is a primitive element of Fp.
### Solution:
```
from sympy import isprime, divisors, mod_inverse

# Given prime field
p = 28151
order = p - 1

# Factor of p-1
factors = [2, 5, 563]  # these are the distinct prime factors of 28150

# Function to check if g is a primitive root
def is_primitive_root(g, p, order, factors):
    return all(pow(g, order // q, p) != 1 for q in factors)

# Find the smallest primitive root
for g in range(2, p):
    if is_primitive_root(g, p, order, factors):
        print(f"The smallest primitive root of F_{p} is: {g}")
        break
```
FLAG=7
## Computing Public Values
### problem:
The Diffie-Hellman protocol is used because the discrete logarithm is assumed to be a "hard" computation for carefully chosen groups.

The first step of the protocol is to establish a prime pp and some generator of the finite field gg. These must be carefully chosen to avoid special cases where the discrete log can be solved with efficient algorithms. For example, a safe prime p=2⋅q+1p=2⋅q+1 is usually picked such that the only factors of p−1p−1 are {2,q}{2,q} where qq is some other large prime. This protects DH from the Pohlig–Hellman algorithm.

The user then picks a secret integer a<p−1a<p−1 and calculates gamod  pgamodp. This can be transmitted over an insecure network and due to the assumed difficulty of the discrete logarithm, the secret integer should be infeasible to compute. The value aa is known as the secret value, while A=gamod  pA=gamodp is the public value.

Given the NIST parameters:

g: 2
p: 2410312426921032588552076022197566074856950548502459942654116941958108831682612228890093858261341614673227141477904012196503648957050582631942730706805009223062734745341073406696246014589361659774041027169249453200378729434170325843778659198143763193776859869524088940195577346119843545301547043747207749969763750084308926339295559968882457872412993810129130294592999947926365264059284647209730384947211681434464714438488520940127459844288859336526896320919633919

Calculate the value of gamod  p.g^a mod p for:

a: 972107443837033796245864316200458246846904598488981605856765890478853088246897345487328491037710219222038930943365848626194109830309179393018216763327572120124760140018038673999837643377590434413866611132403979547150659053897355593394492586978400044375465657296027592948349589216415363722668361328689588996541370097559090335137676411595949335857341797148926151694299575970292809805314431447043469447485957669949989090202320234337890323293401862304986599884732815 
### solution:
```# Given parameters
g = 2
p = 2410312426921032588552076022197566074856950548502459942654116941958108831682612228890093858261341614673227141477904012196503648957050582631942730706805009223062734745341073406696246014589361659774041027169249453200378729434170325843778659198143763193776859869524088940195577346119843545301547043747207749969763750084308926339295559968882457872412993810129130294592999947926365264059284647209730384947211681434464714438488520940127459844288859336526896320919633919
a = 972107443837033796245864316200458246846904598488981605856765890478853088246897345487328491037710219222038930943365848626194109830309179393018216763327572120124760140018038673999837643377590434413866611132403979547150659053897355593394492586978400044375465657296027592948349589216415363722668361328689588996541370097559090335137676411595949335857341797148926151694299575970292809805314431447043469447485957669949989090202320234337890323293401862304986599884732815

# Calculate g^a mod p
A = pow(g, a, p)
print("The computed value of g^a mod p is:", A)
```
FLAG=1806857697840726523322586721820911358489420128129248078673933653533930681676181753849411715714173604352323556558783759252661061186320274214883104886050164368129191719707402291577330485499513522368289395359523901406138025022522412429238971591272160519144672389532393673832265070057319485399793101182682177465364396277424717543434017666343807276970864475830391776403957550678362368319776566025118492062196941451265638054400177248572271342548616103967411990437357924
## Computing Shared Secrets
### Problem 
Now it's time to calculate a shared secret using data received from your friend Alice. Like before, we will be using the NIST parameters:

g: 2
p: 2410312426921032588552076022197566074856950548502459942654116941958108831682612228890093858261341614673227141477904012196503648957050582631942730706805009223062734745341073406696246014589361659774041027169249453200378729434170325843778659198143763193776859869524088940195577346119843545301547043747207749969763750084308926339295559968882457872412993810129130294592999947926365264059284647209730384947211681434464714438488520940127459844288859336526896320919633919

You have received the following integer from Alice:

A: 70249943217595468278554541264975482909289174351516133994495821400710625291840101960595720462672604202133493023241393916394629829526272643847352371534839862030410331485087487331809285533195024369287293217083414424096866925845838641840923193480821332056735592483730921055532222505605661664236182285229504265881752580410194731633895345823963910901731715743835775619780738974844840425579683385344491015955892106904647602049559477279345982530488299847663103078045601

You generate your secret integer bb and calculate your public value B=gbmod  pB=gbmodp, which you send to Alice.

b: 12019233252903990344598522535774963020395770409445296724034378433497976840167805970589960962221948290951873387728102115996831454482299243226839490999713763440412177965861508773420532266484619126710566414914227560103715336696193210379850575047730388378348266180934946139100479831339835896583443691529372703954589071507717917136906770122077739814262298488662138085608736103418601750861698417340264213867753834679359191427098195887112064503104510489610448294420720
B: 518386956790041579928056815914221837599234551655144585133414727838977145777213383018096662516814302583841858901021822273505120728451788412967971809038854090670743265187138208169355155411883063541881209288967735684152473260687799664130956969450297407027926009182761627800181901721840557870828019840218548188487260441829333603432714023447029942863076979487889569452186257333512355724725941390498966546682790608125613166744820307691068563387354936732643569654017172

Try and compute BB yourself and verify things are working using the values above.

You and Alice are now able to calculate a shared secret by using your secret values a,ba,b with each others public values B,AB,A. Note that computing this shared value is infeasible knowing only {g,p,A,B}{g,p,A,B}, it's the knowledge of a,ba,b which allows the generation of the shared value.

What is your shared secret? 
### Solution:
```
# Given parameters
p = 2410312426921032588552076022197566074856950548502459942654116941958108831682612228890093858261341614673227141477904012196503648957050582631942730706805009223062734745341073406696246014589361659774041027169249453200378729434170325843778659198143763193776859869524088940195577346119843545301547043747207749969763750084308926339295559968882457872412993810129130294592999947926365264059284647209730384947211681434464714438488520940127459844288859336526896320919633919
A = 70249943217595468278554541264975482909289174351516133994495821400710625291840101960595720462672604202133493023241393916394629829526272643847352371534839862030410331485087487331809285533195024369287293217083414424096866925845838641840923193480821332056735592483730921055532222505605661664236182285229504265881752580410194731633895345823963910901731715743835775619780738974844840425579683385344491015955892106904647602049559477279345982530488299847663103078045601
b = 12019233252903990344598522535774963020395770409445296724034378433497976840167805970589960962221948290951873387728102115996831454482299243226839490999713763440412177965861508773420532266484619126710566414914227560103715336696193210379850575047730388378348266180934946139100479831339835896583443691529372703954589071507717917136906770122077739814262298488662138085608736103418601750861698417340264213867753834679359191427098195887112064503104510489610448294420720

# Calculate the shared secret
shared_secret = pow(A, b, p)
print("The shared secret is:", shared_secret)
```
FLAG=1174130740413820656533832746034841985877302086316388380165984436672307692443711310285014138545204369495478725102882673427892104539120952393788961051992901649694063179853598311473820341215879965343136351436410522850717408445802043003164658348006577408558693502220285700893404674592567626297571222027902631157072143330043118418467094237965591198440803970726604537807146703763571606861448354607502654664700390453794493176794678917352634029713320615865940720837909466




# PICOCTF
## Sum-O-Primes
We have so much faith in RSA we give you not just the product of the primes, but their sum as well!
Hint:I love squares :)
### solution:
```
from sympy import isprime, lcm, mod_inverse, sqrt

# Given hexadecimal values
x_hex = "1429cf99b5dd5dde9f016095be650d5b0a9a73e648aa72324cb8eb05bd14c1b913539a97f5417474f6014de830ad6dee028dd8908e593b1e99c4cc88f400127214036e71112292e58a2ccffc48f57524aee90f9858d935c7a86297a90c7fe48b80f6c4e8df9eaae501ef40da7984502457255fbc8a9e1574ec6ba210be134f192"
n_hex = "64fc90f5ca6db24f7bfc6419de407596d29a9ecda526101b8d0eff2181e9b8ed1538a1cbabe4dfc5bcd899976e7739f8b448815b50db36a994c5b1df97981d562c663113fc5ee84f3206aecd18248fb4e9bddf205c8119e8437f7d6522e63d05bc357ae4969a4b3000b8226f8d142c23c4e38cdb0c385bf9564e8a115e4c52b7a2e3a9073a5d99d7bec3bca6452cf0c1b8d8b6b123cc6a6980cf14088d6a2bbb5ed36b85cb0003e535bd16d79ad54ff5b26e62f57de074654493d3a26a149786d5fbf61b42c9305092eb018aa3db3cb18b24f188ae520bd18acf9ffced09a2ba302a520f6e2bfd8eea9adc01eb8ee941181694a3ab493e1aa53fbbbf2851a591"
c_hex = "56ed81bbc149701110f0a15e2e6078ab926d74ee2c11b804ae4fad4333a25c247f38bb74867922438d10ce529b75f5ee5e29ce71d6f704cc0644f7e78d60a2af8921fbc49326280e3f0c00f2769e837363cbb05dc3f30bda8fdc94111fb025008eae562ae57029d5cfde6bdd09893a738187578d22f82a5f8769f093681662329f05b262c2054f91696a24f631ba8132f3d92ae7758c91fa9b5657e4944c5d5f93afb4af68908d004ae5f97071bcaceb7d0034297eeb897f972b44b0d7def52f46ee45d386a5e24ed613bf7e5177c6e10f69a3d3de0f0c30de0b15d360ee81da3d277a4acf47b6df389c24615884b692e604eba711fc28c34bc56227b8455705"

# Convert hex to integer
x = int(x_hex, 16)
n = int(n_hex, 16)
c = int(c_hex, 16)

# Calculate the discriminant
discriminant = x**2 - 4*n

# Check if the discriminant is a perfect square
if discriminant >= 0:
    sqrt_discriminant = sqrt(discriminant)
    if sqrt_discriminant.is_integer:
        sqrt_discriminant = int(sqrt_discriminant)

        # Calculate p and q
        p = (x + sqrt_discriminant) // 2
        q = (x - sqrt_discriminant) // 2

        # Check if p and q are prime
        if isprime(p) and isprime(q):
            print(f"p: {p}, q: {q}")

            # Compute d and decrypt the flag
            m = lcm(p - 1, q - 1)
            e = 65537
            d = mod_inverse(e, m)

            # Decrypt the flag
            FLAG = pow(c, d, n)
            # Convert the integer back to bytes
            decrypted_flag = hex(FLAG)[2:]  # Remove '0x' prefix
            # Convert the hex back to string
            flag_bytes = bytes.fromhex(decrypted_flag)
            print(f"Decrypted FLAG: {flag_bytes.decode()}")
        else:
            print("p and q are not primes.")
    else:
        print("The discriminant is not a perfect square.")
else:
    print("The discriminant is negative.")
```
FLAG=picoCTF{pl33z_n0_g1v3_c0ngru3nc3_0f_5qu4r35_24929c45}

