---
layout: post
title:  "Welcome to Crypto!"
date:   2016-06-06 12:12:00 +0200
categories: jekyll update
---

<h1>Working with Matasano Cryptopals</h1>

I always wanted to start writing my first blog post about something that i always cared and appreciate on the field of Computer Science. Cryptography has been for more than thousand years a science that was used on hiding or making the information unreadable in times of war and peace.

I would like to put out of the scope history for a moment, there are a plenty of usefull books and articles that everyone can read about the role that Cryptography played in history. Today we are going to write a simple script to encrypt and decrypt data on ruby.

When i first start learning cryptography, i was used to write simple scripts with ruby, python or perl, because of the many features they have with regular expressions. Basically they make things simpler. Many of books that talk about cryptography explain in theory the right implementation of how to use the algorithm on real applications. Some of the books that i would suggest everyone to read are books like <a href="https://www.amazon.de/Applied-Cryptography-Protocols-Algorithms-Computer/dp/0471117099/278-6334511-5114103?ie=UTF8&*Version*=1&*entries*=0">Applied Cryptography: Protocols, Algorithms and Source Code in C</a> from <a href="https://en.wikipedia.org/wiki/Bruce_Schneier">Bruce Schneier</a>, <a href="https://www.amazon.de/Code-Book-Secret-History-Code-breaking/dp/1857028899/ref=sr_1_5?s=books-intl-de&ie=UTF8&qid=1465246499&sr=1-5&keywords=cryptography">The Code Book: The Secret History of Codes and Code-breaking</a> by Simon Singh, <a href="https://www.amazon.de/Cryptography-Engineering-Principles-Practical-Applications/dp/0470474246/ref=sr_1_4?s=books-intl-de&ie=UTF8&qid=1465246499&sr=1-4&keywords=cryptography">Cryptography Engineering: Design Principles and Practical Applications</a> by Niels Ferguson and more others.

After googling, i found some usefull material on crypto challenges from matasano called <a href="http://cryptopals">Cryptopals</a>. I would suggest everyone that wants to learn even programming to start with the challenges, you will not regret it! There are some other crypto challenges too, like those from <a href="https://www.guardsupport.com/crypto/index.asp">NSA</a> but the Matasano Challenges are focused more in real-time applications.

In the following example i use the Ruby programming language. Lets take the first set and take a look to the challenge 5, Repeating XOR. I picked this challenge because is simple and it can be easy to implement to an application. `The example below doesn't make  an applications secure!!!`. This challenge will only help us just to learn how to use the most simple crypto method to encrypt some text.

So, let's start! First we create a simple file, which will have our plain data stored.

{% highlight text %}
iMario:cryptopals mgrabova$ ls -l
total 0
-rw-r--r--   1 mgrabova  staff    0  6 Jun 17:03 plaintext.txt
drwxr-xr-x  10 mgrabova  staff  340  5 Jun 21:40 set1
iMario:cryptopals mgrabova$
{% endhighlight %}

Now lets put some text on the new file.

{% highlight text %}
iMario:cryptopals mgrabova$ vi plaintext.txt
iMario:cryptopals mgrabova$ cat plaintext.txt
The study of elliptic curves by algebraists, algebraic geometers and number theorists dates back
to the middle of the nineteenth century. There now exists an extensive liter- ature that describes
the beautiful and elegant properties of these marvelous objects. In 1984, Hendrik Lenstra described
an ingenious algorithm for factoring integers that re- lies on properties of elliptic curves. This
discovery prompted researchers to investigate other applications of elliptic curves in cryptography
and computational number theory.Public-key cryptography was conceived in 1976 by Whitfield Diffie and
Martin Hell- man. The first practical realization followed in 1977 when Ron Rivest, Adi Shamir and Len
Adleman proposed their now well-known RSA cryptosystem, in which security is based on the intractability of
the integer factorization problem.
iMario:cryptopals mgrabova$
{% endhighlight %}

Now, basically we have the plaintext that it will be encrypted using repeating XOR, but what that means? First of all Repeating XOR is just a very small piece of the puzzle on encrypting data. To encrypt the plaintext we will need a key, which it will be the same for encrypting and decrypting the data. That means we have to do with a <a href="https://en.wikipedia.org/wiki/Symmetric-key_algorithm">Symmetric-key algorithm</a>.

From programming aproach what we need to do is:

<h4>Encrypt</h4>
<ul>
  <li>Read the file</li>
  <li>Encrypt data</li>
  <li>Output data to another file</li>
</ul>
<h4>Decrypt</h4>
<ul>
  <li>Read the output file</li>
  <li>Decrypt data</li>
  <li>Output data to another file</li>
</ul>

<h2>Ruby!!</h2>

Ruby is an amazing programming language, you can do a lot of stuff, save a lot of time, and has great features and modules for basically everything. I would suggest even Perl programming language for the challenges, because of the amazing features of regular expressions.

To make the script more helpfull for the user we need to create a usage method.

{% highlight ruby linenos=table %}
# Usage Method.
def usage
  if ARGV.length < 2
    puts "usage: ./repeating_xor.rb key <enc/dec> plaintext ciphertext";
    exit;
  end
end
{% endhighlight %}

Next we need to write a method to read the content of the file and an other one to write the encrypted or decrypted data to an output file. So here they are.

{% highlight ruby linenos=table %}
# Method to read the content of the file.
def readFile
  content = File.read(ARGV[2]);
  return content;
end

# Method to write the data to the output file.
def writeFile(data)
  File.open(ARGV[3], 'w') { |file|
    file.write(data);
  }
end
{% endhighlight %}

We use four arguments.
<ul>
  <li>key</li>
  <li>encrypt / decrypt</li>
  <li>file to read from</li>
  <li>file to write to</li>
</ul>

That means that a command like this below will give us the encrypted content of the plaintext file.

{% highlight text %}
iMario:cryptopals mgrabova$ ruby repeating_xor.rb password123 enc plaintext.txt ciphertext.txt
JAkWUwQbBwBIElwWQRYfGwYCEFhRExMUAQUSHFIGSBJSHAYWEQUOGxdFQR9Q
AB8UEg0ABVhRExcEHB4SGxcWQhJSHgVTHQICEAFDEkcYBBwBHhwGFxFWUgQE
AFMVDhEPO0ZcUBUbFlcCGwBVXlZQDhVTAwcXRF9bXRUVFhYZGxpEUlddBBQB
CllPJgxUQFZQDxwEVwoKDUJGQFAAHVMSFwYBX0FaBgRTHx4bFxYcElIEFAEW
VxsaBUUSVxUSEAEeDRcXO0ZbFUERFhYaBg1XR19QAB0XVwoeAVZTXQRBAwEY
HxcWRVtWA0EcFVcbGgFCVxMdAAEFEgMdEUISXBILFhADHFxEeFwTQVhLR1tP
OgFfVkEZClM/EgEBEENTExQEABAFBhABVThSHkEaHRAKHA1eR0BQAB8UGB0b
EFlfExYOAVMRDhEQXkBaHgZTGhkbFwNUQEBQFRsSA08AARwSXxkEAFMYAVIU
Q11DFRMHGhIcUgtXElYcDRoDAwYRRFJHQQYEAF1XOxoNQjhXGRIQHAEKAB0R
QkEfDAMHEgtSFlRBVhETEBsSHQFERV0TGQ8FFgQbGwNQRlZQDgcbEh1SBUFC
XxkCEgceABwXEV1VUAQfHx4fBg1SElAFEwUWBE8bChFRQQkRBxwQHRMUWUs5
EQ8XUxQAHxRERlIECBwdFgNSCkRfURUTUwcfCh0WSBxjBQMfGhRCGQFIElAC
GAMHGAgABUFaSlAWEgBXDB0KUldaBgQXUx4BUlUIBQVQAwpTIAcbEFdbVhwF
UzceCRQNVBJSHgV5PhYdBg1fEnsVDR9eVwITCh8SZxgEUxUeHQEQEUJBEQIH
GhQOHkRDV1IcCAkSAwYdChFUXBwNHAQSC1INXxICSVZEUwAHFwoRYFweQSEa
AQoBEB0SchQIUyAfDh8NQxJSHgVTPxIBeCVVXlYdAB1TBx0dFF5BVhRBBxsS
BgBEX11EUBYWHxtCGQpeRV1QMyAyVwwAHUFGXAMYAAcSAl5EWFwTBwkaEB9P
AQFSR0EZFQpTHhxSBlBBVhRBHB1XGxoBEVtdBBMSEAMOEA1dW0cJQRwVfRsa
ARFbXQQEFBYFTxQFUkZcAggJEgMGHQoRQkEfAx8WGkFSbg==
iMario:cryptopals mgrabova$
{% endhighlight %}

Let's check the function that makes the encryption. The repeating xor does not have any special algorithm but is using some basic principles of the <a href="https://en.wikipedia.org/wiki/Vigenère_cipher">vigenere cipher</a>.

{% highlight ruby linenos=table %}
# Method to encrypt and decrypt the data.
def dataEncDec(data, key)
  data.length.times { |e|
    # If key => 123 and text => "this is a message"
    # 't', 'h', 'i', 's', ' ', 'i', 's', ' ', 'a', ' ', 'm', 'e', 's', 's', 'a', 'g', 'e'
    # '1', '2', '3', '1', '2', '3', '1', '2', '3', '1', '2', '3', '1', '2', '3', '1', '2'
    keychar = key[e % key.length].ord;
    inchar = data[e].ord;
    data[e] = (keychar ^ inchar).chr;
  }
  return data;
end
{% endhighlight %}

We try to reproduce the key to be the same length with the plain text length, so we can xor every letter with each other, which are turned to number and then after xor-ing are converted to characters again. Unfortunately the output will be just binary data, which will look really awfull, so for clear output let's use the Base64 module to have a pretty output on console and the ciphertext file.

{% highlight ruby linenos=table %}
# Display usage information.
require "base64"

...

usage();

data = readFile;
if ARGV[1] == "enc"
  # Encrypt.
  data = dataEncDec(data, ARGV[0]);
  data = Base64.encode64(data);
  puts "#{data}";
elsif ARGV[1] == "dec"
  # Decrypt.
  data = Base64.decode64(data);
  data = dataEncDec(data, ARGV[0]);
  puts "#{data}";
end
writeFile(data);
{% endhighlight %}

And when we want to decrypt the encrypted data we have the next command.

{% highlight text %}
iMario:cryptopals mgrabova$ ruby repeating_xor.rb password123 dec ciphertext.txt plaintext.txt
The study of elliptic curves by algebraists, algebraic geometers and number theorists dates back
to the middle of the nineteenth century. There now exists an extensive liter- ature that describes
the beautiful and elegant properties of these marvelous objects. In 1984, Hendrik Lenstra described
an ingenious algorithm for factoring integers that re- lies on properties of elliptic curves. This
discovery prompted researchers to investigate other applications of elliptic curves in cryptography
and computational number theory.Public-key cryptography was conceived in 1976 by Whitfield Diffie and
Martin Hell- man. The first practical realization followed in 1977 when Ron Rivest, Adi Shamir and Len
Adleman proposed their now well-known RSA cryptosystem, in which security is based on the intractability of
the integer factorization problem.
iMario:cryptopals mgrabova$
{% endhighlight %}

This is one of the more simple examples of encrypting data. But let's check what happens when someone use a different password, so instead of `password123` we add `password1234`.

{% highlight text %}
iMario:cryptopals mgrabova$ ruby repeating_xor.rb password1234 dec ciphertext.txt plaintext.txt
The study o"1wlhqmb<`!  qdao%i:vc.5"ad}h`*3{a2, blsv/>as5.-&kve1e=lab/16``0a0wvx0,/2gp%!skr7$<";K'/#btd33)3a.7#}b<qc&vlo-tfeavi~u`ditury. Thered}w$}e&wrc4m2adqn-%k47gohdas`65&2"'ziv2}%q#"2*}vdH14g% $%.vl$4(?ry&e9*q7 .s.qe)-$"5:%p6sg3*(2v'rnsvj`g,#q&,sxegln8uJo'1984, Hendr-z2Lavnb'b!'0pqvugn'\c,r.mqyoz15$a2, h|hc.0ar?3`%~pc-75lbb(*/gb'37?g#1|4q}a,nkr7)3a 3<0fdhhv-`8cb7o~mlqb vas1gws*8I~<p
ciscovery pr+|bta|=d0pd"'`zanv+79"+<1fehht?42a<4/b|!v6--}3 4*p}db*:"`.--+vl!a'jaa'6|/zp"2~~ux!/  )*Jbxx!p1-'p63w{krdgc8wk!t` ppxy'z/Wublic-key c6hbtkow%kxc"ba$je 3k47##r!"gwaa19gPfhc 4$x4a*yu~'e=laH
                                        %)vl,a
                                              z{oe1'z~aok!q//2$a#2buhhp?,ww'3o{~}qb,8"`,}~os}y6<m!6977 when Ro*1@ir}nby#@'<#Al}hb1vc,6gOsr
 R:,2,2.gw|ng).$pp5(&va7,*+"r'-(vik-6*?ADe?4m 5/twrc#0mp(=`t~ub{~32f7 jf}<lxc4cu&u2oj8i~0#hitractabilit=1}flusujo70dwv<cj "m0;=bbun}~0%.1,"j !
iMario:cryptopals mgrabova$
{% endhighlight %}

All what we get is the encrypted data which is encrypted once again with a different password. Repeating XOR can be a really good example on how someone can encrypt some data just for fun. As from the security prespective repeating XOR in this example is not ``secure`` to be used.
