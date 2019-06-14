---
layout: post
title: PYODC face smash
---
Just had a hell of a time installing pyodbc on AWS ECS Linux AMI. And since I think I smashed it... here's what i did.

First go though everything on this post. An awesome start 

https://datalere.com/tips-guides/ec2-aws-linux-ami-pyodbc/

However, the challenege I had next 

```
(env) [ec2-user@ip-172-30-1-231 eurion]$ pip install pyodbc
Collecting pyodbc
Installing collected packages: pyodbc
```

Yet, `pip freeze` **still** had no pyodbc! FACESMASH!

So, after running through it 100 more times slightly differently, it occured, let's go manually, shall we??

```
(env) $ cd ~/src
(env) $ curl -O -L https://github.com/mkleehammer/pyodbc/archive/4.0.26.tar.gz
(env) $ tar xvzf 4.0.26.tar.gz
(env) $ cd pyodbc-4.0.25
(env) $ python setup.py build
(env) $ python setup.py install
```

not there... one more error.

```
TEST FAILED: env/lib64/python3.6/dist-packages/ does NOT support .pth files
error: bad install directory or PYTHONPATH

You are attempting to install a package to a directory that is not
on PYTHONPATH and which Python does not read ".pth" files from.  The
installation directory you specified (via --install-dir, --prefix, or
the distutils default setting) was:

    env/lib64/python3.6/dist-packages/

and your PYTHONPATH environment variable currently contains:

    ''

```

So I guess I'm setting `$PYTHONPATH`

After trying a few try's, I needed to knwo not just my virtural environment directory but also where my 
`site-packages` and `dist-packages`. Which ended up being

```
export PYTHONPATH=$PROJROOT/env/:$PROJROOT/env/lib64/python3.6/dist-packages/:$PROJROOT/env/lib64/python3.6/site-packages/
```

Any now give `python setup.py install` a go.

And we're good to go.
