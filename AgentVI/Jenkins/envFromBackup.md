# Create new env from backup for AgentVI

1. Run jenkins ansible to setup jenkins in VPC
2. deploy jenkins with ansible
3. go to jenkins master and extract latest backup to /var/lib/jenkins tar xvzf my-jenkins-backupfile.tar.gz -C /
4. update jenkins config.xml security realm with correct credentials if required for authentication. (for github security realm you will need clientid and secret key) - look up note "Encrypt value to use in jenkins configuration"
5. update slaves (remove old slaves from jenkins and connect to the newly created ones
6. update credentials private key for the slave users to the correct one (that was deployed for the jenkins vpc). update the key in JENKINS_HOME/.ssh/slave_id_rsa to the content of the private key created for the vpc keypair.
7. update jenkins location > jenkins url configuration to the new url in jenkins config page
8. slaves using amazon api (ansible) will need
   1. install boto (pip2 install boto)
   2. have configured .aws/credentials in the ubuntu user's home directory 
9. Install ansible version 2.2.1.0 on the slaves but for the python 2.7
    pip2 install ansible==2.2.1.0
10. Add slave SSH to known_hosts file on master for jenkins user`