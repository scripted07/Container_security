ALl steps and screenshots are attached on pdf. 
I demonstrated this on the kali virtual macchine. 


# Container_security
What is container security? As security threats and opportunities to tamper with organizations escalates, it’s increasingly important for organization to assess their system’s attack surface to identify all possible points of vulnerability. Container Security is a critical part of a comprehensive security assessment. 

#Security Hardening (Process to secure containers) 
Hardening is the process of strengthening the security of a system or application by reducing its vulnerabilities and minimizing the attack surface. Here are a few best practices for hardening container security: 
1.	Keep Host and Docker up to date:- 
To prevent from known, container escapes vulnerabilities, which typically end in escalating to root/administrator privileges, patching Docker Engine and Docker Machine is crucial. 
In addition, containers (unlike in virtual machines) share the kernel with the host, therefore kernel exploits executed inside the container will directly hit host kernel. For example, kernel privilege escalation exploit (like Dirty COW) executed inside a well-insulated container will result in root access in a host.
 
Command: - docker ps –a 

2.	Set a user:- 
Configuring the container to use an unprivileged user is the best way to prevent privilege escalation attacks. This can be accomplished in three different ways as follows: 
  
3.	Limit capabilities 
Linux kernel capabilities are a set of privileges that can be used by privileged. Docker, by default, runs with only a subset of capabilities. You can change it and drop some capabilities (using --cap-drop) to harden your docker containers, or add some capabilities (using --capadd) if needed. Remember not to run containers with the --privileged flag - this will add ALL Linux kernel capabilities to the container. 
  
4.	Add –no-new-privileges flag:- 
Always run your docker images with --security-opt=no-new-privileges in order to prevent escalate privileges using setuid or setgid binaries. 
Command: docker container_name --security-opt=no-new-privileges 
5.	Disable inter-container communication (--icc=false):- 
By default inter-container communication (icc) is enabled - it means that all containers can talk with each other (using docker0 bridged network). This can be disabled by running docker daemon with flag. If icc is disabled (icc=false) it is required to tell which containers can communicate using --link=CONTAINER_NAME_or_ID:ALIAS option. See more in Docker documentation - container communication 
 
Command:-  run with this command --icc=false 
6.	Set filesystem and volumes to read-only 
Run containers with a read-only filesystem using --read-only flag. For example: 
Command: docker run --read-only alpine sh -c 'echo "whatever" > /tmp' 
7.	- Use static analysis tools 
To detect containers with known vulnerabilities - scan images using static analysis tools. 
-Free Tools 
Clair 
ThreatMapper 
Trivy 
-Commercial 
Snyk (open source and free option available) anchore (open source and free option available) 
Aqua Security's MicroScanner (free option available for rate-limited number of scans) 
JFrog XRay 
Qualys 
8. Set the logging level to at least INFO 
By default, the Docker daemon is configured to have a base logging level of 'info', and if this is not the case: set the Docker daemon log level to 'info'. Rationale: Setting up an appropriate log level, configures the Docker daemon to log events that you would want to review later. A base log level of 'info' and above would capture all logs except the debug logs. Until and unless required, you should not run docker daemon at the 'debug' log level. 
To configure the log level in docker-compose: 
Command: docker-compose --log-level info up 
