# Install the IdM Server software

FreeIPA server provides centralized authentication, authorization and account information by storing data about user, groups, hosts and other objects necessary to manage the security aspects of a network of computers. 

On the *control* node, use ansible to install the IPA Server on *host02* - 

`ansible-playbook -v -i ~/hosts /usr/share/doc/ansible-freeipa/playbooks/install-server.yml`{{execute T2}}

> __NOTE__ : In this step, we have only installed a single IdM server. However, in production, IdM replicas are typically deployed for scale and high-availability reasons. An IdM replica server is a full read/write copy of the first installation, and can be easily created using the *ipa-replica-prepare* command. 

The target host (*host02*) and other IPA server variables are picked up from the ansible inventory file (*/root/hosts*).

A number of different services are installed together with an IdM server, including Directory Server, Certificate Authority (CA), DNS, Kerberos, and others. 

# Stop and start the IPA Server 

Because IdM has several different services working together, *ipactl* is a single utility to stop, start, or restart the entire IdM server along with all other installed services.

Using the *host02* terminal of the lab, stop the IdM server and all installed services -

`ipactl stop`{{execute T3}}

Using the *host02* terminal of the lab, start the entire IdM server and all installed services -

`ipactl start`{{execute T3}}

If you only want to stop, start, or restart an individual IdM service, use the *systemctl* utility.

# Configure the DNS name resolution 

On most Linux operating systems, the DNS servers that the system uses for name resolution is defined in the */etc/resolv.conf* file.

Using *host03* terminal of the lab, make a DNS entry with the IP address of the IdM server in the DNS resolution file */etc/resolv.conf* -

`IP_Server=$(grep -Ri "host02" /etc/hosts | cut -d" " -f1)`{{execute T4}}

`sed -i '2 i nameserver '"$IP_Server" /etc/resolv.conf`{{execute T4}}

Inspect the */etc/resolv.conf* file to ensure that there is a nameserver entry with the IdM server's IP -

`cat /etc/resolv.conf`{{execute T4}}
