Example of using:   
$ ansible-playbook -i ./hosts -Kk deployAD_IOC.yml --limit BMM --extra-vars ioc_name="SimTestFriday"

Anotther example with more redefined variables:
$ ansible-playbook -i ./hosts -Kk deployAD_IOC.yml --limit SRX --extra-vars ioc_name="SimDetBinariesDep IOC_TYPE=ADSimDetector PREFIX=XF:05ID1-ES{Test-Cam:1} generateEnvPaths=YES"

