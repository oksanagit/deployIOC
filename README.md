# What script does and supports 
This Ansible utility automaticaly deploys/creates instances of IOCs in {root_ioc_dir} folder.  
To change the deployment folder, one needs to redefine {root_ioc_dir}.  
user:group should be defined in group_vars/all.yml.
These settings delploy all areaDetector IOCs in  /epics/iocs as softioc:controls user and xspress3 IOCs in /home/xspress3/epics/iocs/ as xspres3:xspres3.  
   
The following is supported:
- xspress3
- ADSimDetector
- ADProsilica
- ADPointGrey
- ADEiger

Some deployments types may require gennerating envPaths. This is supported for: ADSimDetector, ADProsilca.
Please let me know if you need it for anything else.


# Script structure overview
main.yml calls deployAD_IOC.yml and loops over IOCs which are defined in {ioc_names}, with each IOC macros defined in _IOCs:{ioc_name}: dictionary. Differnt IOCs may requre different macros. For example, Prosilica and Eiger use IP address, while Point Grey uses SN (Serial Number ID).   
   
Example of deployment on a beamile host xf05idd-ioc-det2 of all IOCs defined in xf05idd-ioc-det2.yml using ssh as xspress3 user:    
```ansible-playbook -i ./hosts -Kk main.yml --limit xf05idd-ioc-det2  --user xspress3```   
-k requires user login   
-K requires sudo password  

Example of delploying on BMM hosts with configuration detatils defined in each host.yml file:   
```ansible-playbook -i ./hosts -Kk main.yml --limit BMM```   

One can deploy a single IOC with all macros (variables) supplied in the command line:
$ ansible-playbook -i ./hosts -Kk deployAD_IOC.yml --limit SRX --extra-vars ioc_name="SimDetBinariesDep IOC_TYPE=ADSimDetector PREFIX=XF:05ID1-ES{Test-Cam:1} generateEnvPaths=YES"

