## Load Balancer Solution With Nginx and SSL/TLS (Transport Layer Security)
 In this project , we will configure Nginx load balance solution.It is extremely important to ensure that conection to your Web solutions are secure and information is encrypted in transit, we will also cover connection over secured HTTP (HTTPS protocol), its purpose and what is required to implement it.
 When data is moving between a client (browser) and a Web Server over the Internet.it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-In-The-Middle (MIMT) attack.
 This threat is real – users that share sensitive information (bank details, social media access credentials, etc.) via non-secured channels, risk their data to be compromised and used by cybercriminals.

 SSL and its newer version, TSL – is a security technology that protects connection from MITM attacks by creating an encrypted session between browser and Web server. Here we will refer this family of cryptographic protocols as SSL/TLS – even though SSL was replaced by TLS, the term is still being widely used

