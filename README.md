# NSLS2 specific script for deploing camera IOCs against bundles
Script in this form is tested for ADProsilica only. Other rolles can be added. Let me know if you need one. 
Current location of the bundle is centraly defined by {root_ioc_dir}.
You need to have sudo rigts on the target host because script creates /epics/iocs/camxxN and changes the access rights.

# Quick start
go to ansible machine
make sure you can ssh to your target host
git clone https://github.com/oksanagit/deployIOC.git
create file ```host_vars/your_hostName.yml``` following the example
add your host to ```hosts``` file
run ```ansible-playbook -i ./hosts -Kk main.yml --limit BMM``` change BMM for your beamline_tag
-k may be omitted if you have keys configgured
-K will ask for the sudo access rights 


# General description
This Ansible utility automates deployment  of IOC instances in {root_ioc_dir} folder.  
To change the deployment folder, one needs to redefine {root_ioc_dir}.  
user:group should be defined in group_vars/all.yml.
These settings deploy all areaDetector IOCs in  /epics/iocs as softioc:controls user and xspress3 IOCs in /home/xspress3/epics/iocs/ as xspres3:xspres3.  
   
The following is supported:
- xspress3
- ADSimDetector
- ADProsilica
- ADPointGrey
- ADEiger

Some deployments types may require generating envPaths. This is supported for: ADSimDetector, ADProsilca.  
Please let me know if you need it for anything else.


# Script structure overview
main.yml calls deployAD_IOC.yml and loops over IOCs which are defined in {ioc_names}, with each IOC macros defined in _IOCs:{ioc_name}: dictionary. Different IOCs may require different macros. For example, Prosilica and Eiger use IP address, while Point Grey uses SN (Serial Number ID).   
   
Example of deployment on a beamline host xf05idd-ioc-det2 of all IOCs defined in xf05idd-ioc-det2.yml using ssh as xspress3 user:    
```ansible-playbook -i ./hosts -Kk main.yml --limit xf05idd-ioc-det2  --user xspress3```   
-k requires user login   
-K requires sudo password  

Example of deploying on BMM hosts with configuration details defined in each host.yml file:   
```ansible-playbook -i ./hosts -Kk main.yml --limit BMM```   

One can deploy a single IOC with all macros (variables) supplied in the command line:
$ ansible-playbook -i ./hosts -Kk deployAD_IOC.yml --limit SRX --extra-vars ioc_name="SimDetBinariesDep IOC_TYPE=ADSimDetector PREFIX=XF:05ID1-ES{Test-Cam:1} generateEnvPaths=YES"

