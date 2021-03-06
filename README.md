# Docker-Freeling

Dockerfile in this folder is a modified version of the original one with only support for English and Spanish languages. Freeling can run in different modes: server mode and standalone mode. You can create application that consumes services from freeling running standalone or as a server (check references for ways of execute shell commands from python)  

## 1. Create the docker image

```
$ docker build -t freeling .
```

## 2. Configuration file (Optional)
If configuration file is not in your folder it is searched inside the container in the path _/usr/local/share/freeling/config/_ . You can check the configuration files creating a temporary
docker container

```
$ docker run -it --rm freeling /bin/bash
# cd /usr/local/share/freeling/config/
# ls
```

You can also use this for checking other files or debugging purposes

## 3. Modes

## A. Standalone mode
As a standalone mode you run the analyzer one time per input file

### Create an input file

```
$ echo "El gato come pescado. Pero a Don Jaime no le gustan los gatos." > input.txt
```

### Create a container
You have to specify the configuration file according with the input language

```
$ docker run -i --rm  freeling analyze -f es.cfg < input.txt > output.txt
```

### Check the output

```
$ cat ouput.txt
El el DA0MS0 1
gato gato NCMS000 1
come comer VMIP3S0 0.978902
pescado pescado NCMS000 0.822581
. . Fp 1

Pero pero CC 0.999902
a a SP 0.998775
Don_Jaime don_jaime NP00000 1
no no RN 0.999297
le le PP3CSD0 1
gustan gustar VMIP3P0 1
los el DA0MP0 0.992728
gatos gato NCMP000 1
. . Fp 1
```

## B. Server mode
As a server mode you have to start a container in background mode and use a client to request an analysis

### Create an input file

```
$ echo "El gato come pescado. Pero a Don Jaime no le gustan los gatos." > input.txt
```

### Create a server container

```
 $ docker run -it --rm -p 50005:50005 freeling analyze -es.cfg --server -p 50005
```

### Make a request

You can make an analysis request to the server container from localhost (127.0.0.1) or from a remote machine.

```
$ chmod +x analyzer_client
$ ./analyzer_client localhost:50005 < input.txt > output.txt
```

### Check the output

```
$ cat output.txt
El el DA0MS0 1
gato gato NCMS000 1
come comer VMIP3S0 0.978902
pescado pescado NCMS000 0.822581
. . Fp 1

Pero pero CC 0.999902
a a SP 0.998775
Don_Jaime don_jaime NP00000 1
no no RN 0.999297
le le PP3CSD0 1
gustan gustar VMIP3P0 1
los el DA0MP0 0.992728
gatos gato NCMP000 1
. . Fp 1
```

### 4. Python
You have several options for using freeling with python. You can create a python script or python webservice that uses docker analyzer_client to make request to a freeling server container.

```
Here code that uses POPEN for executing the analyzer_client and parsing output, check ICESI-Training/microservices2016b for more info
```

Other option is to use pyfreeling.

```
$ docker run -it --rm freeling /bin/bash
# cd/tmp
# python3 pyfreeling_sample.py
```

And the last one is to use the Python API.

### 5. References
https://github.com/TALP-UPC/FreeLing/tree/master/APIs/docker  
https://talp-upc.gitbooks.io/freeling-user-manual/content/analyzer.html  
https://github.com/ICESI-Training/microservices2016b
https://github.com/malev/pyfreeling
