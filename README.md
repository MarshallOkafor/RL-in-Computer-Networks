# Applications of Reinforcement Learning to Computer Networks
This repo outlines the steps on how to use the _**OWL**_ application in a VM running ```Linux``` operating system or on a ```Linux``` standalone computer. The original repository that contains the implementation of  _**OWL**_ can be found here _**[tcp-owl](https://github.com/alessiosac/tcp-owl)**_.

For simplicity and ease of setup, we provisioned and packaged a VM with _**OWL**_ already installed and setup. Below are detailed step on how to retrieve the VM and test out the _**OWL**_ application.

# Get the VM
Visit this **[Download image](https://google.com)** to get the VM image that contains a preinstalled _**OWL**_ application. The image format is in Open Virtual Appliance (OVA) and can be imported into any virtualization platform of your choice. We conducted our test of the image using Oracle virtualbox, hence we recommend that you use Oracle virtualbox.

# Load kernel module

The `kernel_module` folder includes files for the kernel implementation of the _**OWL**_ algorithm.
After importimg and starting the virtual appliance, run the following commands below to load the _**OWL**_ Linux kernel module congestion control algorithm:

```bash
$ cd kernel_module   # change into the kernel module dir
$ sudo insmod tcp_owl.ko    # load the module
```

After the module has been loaded, check to ensure that _**OWL**_ is now listed among the available congestion control algorithms by using the command below:  

```bash
$ cat /proc/sys/net/ipv4/tcp_available_congestion_control
reno cubic owl  # output
```

Then config "owl" as default congestion control for TCP
```bash
$ sudo su    # switch to root user
$ echo owl > /proc/sys/net/ipv4/tcp_congestion_control
```

or
```bash
$ sudo sysctl -w net.ipv4.tcp_congestion_control=owl
```

To unload the module from the kernel, run:
```bash
$ sudo rmmod owl
```

# Test _**OWL**_

## iperf
The behavior of _**OWL**_ can be easily experienced starting a communication, e.g., using [iperf](https://iperf.fr/iperf-download.php). To test the application using iperf, follow the steps below:  

1. Open two different terminals on the Linux VM that you downloaded earlier and import into your virtual environment.  
2. In the ```home``` directories of the terminals, you will find two bash scripts ```server.sh```and ```client.sh```.
3. Start the iperf server by running the server script in one of the terminals by entering the command ```./server.sh```.
4. In the other terminal, start the client script by entering the command ```./client.sh```.
5. Observe the performance of _**OWL**_ congestion control algorithm.

## mahimahi
To reproduce many different link conditions we use [Mahimahi](http://mahimahi.mit.edu/) network emulation tool.

