## BFlows

BFlows is a SDN-based traffic schduling application. It includes a set of Ryu applications collecting basic network information, such as topology and free bandwidth of links.
You can specify the mode of computing shortest paths when starting Ryu by adding "weight" argument. Moreover, you can set "k_paths" argument to support K-Shortest paths computing.
Fortunately, our application supports load balancing based on dynamic traffic information.

The detailed information of the modules is shown below:

* Fattree is topology modules;

* Network Awareness is a module for collecting network information;

* Network Monitor is a module for collecting traffic information;

* BFlows is the main module of the application;

* Setting is the module including common setting.

We make use of networkx's data structure to store topology. Meanwhile, we also utilize networkx's built-in algorithm to calculate shortest paths.


### Prerequisites

The following softwares should have been installed in your machine.
* Mininet: git clone git://github.com/mininet/mininet; mininet/util/install.sh -a
* Ryu: git clone git://github.com/osrg/ryu.git; cd ryu; pip install .
* Networkx: pip install networkx


### Download

Download files into Ryu directory, for instance, 'ryu/ryu/app/BFlows' is OK.


### Make some change

To register parsing parameters, you NEED to add the following code into the end of ryu/ryu/flags.py.

    CONF.register_cli_opts([
        # k_shortest_forwarding
        cfg.IntOpt('k_paths', default=4, help='number of candidate paths of KSP.'),
        cfg.StrOpt('weight', default='bw', help='weight type of computing shortest path.'),
        cfg.IntOpt('fanout', default=4, help='switch fanout number.')])


### Reinstall Ryu

You must reinstall Ryu, so that you can run the new code. In the top directory of Ryu project:

    sudo python setup.py install


### Start

Note: Before doing the experiment, you should change the controller's IP address from '192.168.56.101' to your own machine's eth0 IP address in the fattree.py module in each application, because '192.168.56.101' is my Ubuntu's eth0 IP address (Try 'ifconfig' in your Ubuntu to find out the eth0's IP address). Otherwise, the switches can't connect to the controller.

Firstly, start up the network. An example is shown below:

    $ sudo python ryu/ryu/app/BFlows/fattree.py --k 4

And then, start a  new terminal and go into the top directory of Ryu, then run the application. You are suggested to add arguments when starting Ryu. An example is shown below:

    $ cd ryu
    $ ryu-manager --observe-links ryu/app/BFlows/BFlows.py --k_paths=4 --weight=fnum --fanout=4

NOTE: After these, we should wait for the network to complete the initiation for several seconds, because LLDP needs some time to discovery the network topology. We can't operate the network until "[GET NETWORK TOPOLOGY]" is printed in the terminal of the Ryu controller, otherwise, some error will occur. It may be about 10 seconds for fattree4, and a little longer for fattree8.

After that, test the correctness of BFlows:

    mininet> pingall
    mininet> iperf

If you want to show the collected information, you can set the parameters in setting.py. Also, you can change the setting as you like.
After that, you can see the information shown in the terminal.


### Author

Brought to you by Huang MaChi (Chongqing University of Posts and Telecommunications, Chongqing, China).

If you have any question, you can email me at huangmachi@foxmail.com.

Enjoy it!
