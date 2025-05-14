# Table of Contents

1.  [Connecting](#orgb0675fe)
2.  [Exploit](#org657e302)
3.  [Implementation](#org93ee44a)
4.  [Create Payload](#org84d0a85)
5.  [Connect and send payload](#org6972142)



<a id="orgb0675fe"></a>

# Connecting

First ssh into and look at code via cat


<a id="org657e302"></a>

# Exploit

The hash we are trying to map is not cryptographically safe and we can easily insert 4 byte integers to match the hash


<a id="org93ee44a"></a>

# Implementation

So here we want the sum of all 5, 4 byte integers to be equal to 0x21DD09EC. We can simply take this number and divide by 5 take the floor and then make the last integer 0x21DD09EC - 4 \* result

    total = 0x21DD09EC
    regular_four = total // 5
    regular_four

    irregular_five = total - regular_four * 4
    irregular_five


<a id="org84d0a85"></a>

# Create Payload

Now we create the payload by making 4 4 byte integers of the regular<sub>four</sub> and a 4 byte integer of the regular 5

    from pwn import *
    values = [regular_four] * 4 + [irregular_five]
    payload = b''.join(p32(v) for v in values)
    payload


<a id="org6972142"></a>

# Connect and send payload

    ssh_conn = ssh(host='pwnable.kr', user='col', port=2222, password='guest')
    p = ssh_conn.process(argv=['./col', payload])
    print(p.recvall(timeout=30).decode(errors='ignore'))
