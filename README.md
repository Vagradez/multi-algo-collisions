# Birthday Attack: Cryptographic hash functions Collisions

> This was a team project with two others: Andrew Seitz and Tobias Muller in March 2014 for my cryptography class. Here's the writeup we did.

# On fork 
> Originaly suports sha1 function i forked it to support multiple Cryptographic hash functions, sha1|sha224|sha256|sha384|sha512|md5

We implemented the birthday attack by searching across iterations of the uppercase and lowercase ASCII characters, along with numbers.

## Design

The code originaly is written in Python 3.4 and uses the `sha` function from the `hexlib` library to search for collisions. It takes two arguments: the first is the maximum number of random bytes to use as input to the hash function, and the second is the number of bytes needed, starting at the beginning of the hash, for two inputs to be considered a collision.

The way the code works is this: random hashes are generated, and the results of each hash are stored as keys in a dictionary (Python's implementation of the hash table data structure). This allows lookup of collisons for already generated hashes to happen in constant time. When a collision is found, the results are printed to the screen.

## Results

Since the results only took a few seconds to run, we ran the program a few times to generate multiple collisions. We ran more than these, and they generally would have to try about 20 million attempts before a collision was found.

According to Wikipedia^[`https://en.wikipedia.org/wiki/Birthday_attack`] the number of trials required to reach 50% chance of finding a collision can be found by
$$n(0.5,2^{48}) \approx 1.1774 \sqrt{2^{48}} = 19.755 \times 10^6.$$
This is basically in line with the 20 million figure we saw from our test runs.


## Algo


### sha1 collision 

``` Bash
$ python3.4 sha.py 10 4
Collision found!

4 bytes collision found using 'sha1' algorithm in 1 seconds

Validate it

echo -n 'a' | 'sha1sum' | cut -c1-4  && echo -n '5Gf' | 'sha1sum' | cut -c1-4

a (bytes: b'61') hashes to 86f7, but 5Gf (bytes: b'354766') also hashes to 86f7

```

### sha224 collision

``` Bash
$ python3.4 sha.py 10 4 sha224
Collision found!

4 bytes collision found using 'sha224' algorithm in 5 seconds

Validate it

echo -n 'a' | 'sha224sum' | cut -c1-4  && echo -n 'leG' | 'sha224sum' | cut -c1-4

a (bytes: b'61') hashes to abd3, but leG (bytes: b'6c6547') also hashes to abd3

```

### sha256 collision

``` Bash
$ python3.4 sha.py 10 4 sha256
Collision found!

4 bytes collision found using 'sha256' algorithm in 2 seconds

Validate it

echo -n 'a' | 'sha256sum' | cut -c1-4  && echo -n '8uN' | 'sha256sum' | cut -c1-4

a (bytes: b'61') hashes to ca97, but 8uN (bytes: b'38754e') also hashes to ca97

```

### sha384 collision

``` Bash
$ python3.4 sha.py 10 4 sha384
Collision found!

4 bytes collision found using 'sha384' algorithm in 1 seconds

Validate it

echo -n 'a' | 'sha384sum' | cut -c1-4  && echo -n '3Lt' | 'sha384sum' | cut -c1-4

a (bytes: b'61') hashes to 54a5, but 3Lt (bytes: b'334c74') also hashes to 54a5

```

### sha512 collision

``` Bash
$ python3.4 sha.py 10 4 sha512
Collision found!

4 bytes collision found using 'sha512' algorithm in 7 seconds

Validate it

echo -n 'a' | 'sha512sum' | cut -c1-4  && echo -n 'CN0' | 'sha512sum' | cut -c1-4

a (bytes: b'61') hashes to 1f40, but CN0 (bytes: b'434e30') also hashes to 1f40


```

### md5 collision

``` Bash
$ python3.4 sha.py 10 4 md5
Collision found!

4 bytes collision found using 'md5' algorithm in 4 seconds

Validate it

echo -n 'a' | 'md5sum' | cut -c1-4  && echo -n 'qZ7' | 'md5sum' | cut -c1-4

a (bytes: b'61') hashes to 0cc1, but qZ7 (bytes: b'715a37') also hashes to 0cc1



```

