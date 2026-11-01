ROOTGRID
Home Lab Project — Summary and Motivation

1. Project Overview and Motivation
RootGrid is a self-hosted home-lab project built to demonstrate how individuals can host and manage their own digital services rather than relying entirely on large third-party technology companies. Its central motivation is the belief that people should have greater ownership, control and responsibility over their digital data.
Personal information, documents, photographs and other digital content are commonly stored on infrastructure owned and operated by external providers. These services are convenient, but they require users to place trust in third parties to store, manage and protect their information. RootGrid explores an alternative: a personal infrastructure where applications and data can be hosted on hardware controlled by the user.
The project is guided by five principles:
•	Data ownership: Users should retain control over where their information is stored and how it is managed.
•	Privacy: Personal information should not have to be entrusted to an external cloud provider simply to use cloud-storage functionality.
•	Security: Data should be protected through appropriate access controls, secure remote connections, backups and responsible administration.
•	Independence: Individuals should have the option to operate digital services without depending entirely on large technology companies.
•	Transparency: Users should understand the infrastructure and processes responsible for storing and managing their information.
RootGrid does not assume that third-party providers are inherently unsafe or that self-hosting automatically guarantees complete security. Instead, it demonstrates that individuals can take a more active role in managing their privacy and infrastructure. By repurposing an ordinary laptop as a home server, the project also shows that a personal cloud environment can be explored without expensive enterprise hardware.
Ultimately, RootGrid is about bringing control of personal data closer to the people who own it.

2. Project Implementation and Achievements
2.1 Physical Server and Operating System
An older Dell Inspiron 15 3000 laptop was repurposed as the main server. It runs Ubuntu Server 26.04.1 LTS and has an Intel Core i7-8550U processor, 8 GB RAM and a 128 GB SSD. Additional storage is provided through two ext4 partitions mounted at /storage0 and /storage1. SSH and Tailscale were configured for remote administration and private connectivity.
2.2 Virtualisation
QEMU/KVM and libvirt were used to create and manage a separate Ubuntu Server 24.04.5 virtual machine named web-vm. This separates the application environment from the physical host and provides a dedicated place to run the project's web and cloud services.
2.3 RootGrid Website
A project website was developed using HTML, CSS and JavaScript and stored in GitHub. It is served by an Nginx Docker container and is publicly available at https://rootgrid.au. The container maps host port 8081 to container port 80. Repository: https://github.com/utsavniroula-boop/rootgrid.
2.4 Personal Cloud Storage
A self-hosted cloud environment was deployed using Docker Compose. It consists of Nextcloud Apache for the cloud application, PostgreSQL 16 for database services and Redis 7 for caching. Nextcloud maps host port 8080 to container port 80 and is published at https://cloud.rootgrid.au. A separate virtual disk was created, formatted with ext4 and mounted at /srv/cloud-data for cloud data.
2.5 Public Access Through Cloudflare
Cloudflare Tunnel was configured to publish the RootGrid website and personal cloud without relying on direct inbound port forwarding for these public services. The intended request path is browser → Cloudflare edge → Cloudflare Tunnel → configured origin service.
2.6 Private Administrative Services
Administrative access was separated from public-facing services. Tailscale provides private network connectivity, while SSH is used for remote server and virtual-machine administration. Jenkins runs in a Docker container with host port 127.0.0.1:8082 mapped to container port 8080. Jenkins is accessed through an SSH tunnel rather than being exposed as a public service.
2.7 Automated Deployment
Jenkins retrieves the website source from GitHub, checks for the required HTML, CSS and JavaScript files, and deploys them to /srv/rootgrid-site. Temporary files and rename operations are used during deployment to reduce the risk of partially updated files. This creates a repeatable CI/CD workflow and reduces manual file copying.

3. Problems Encountered and Resolutions
•	Proxmox installation did not boot successfully from the USB installer. The project moved to Ubuntu Server and established virtualisation using QEMU/KVM and libvirt.
•	Initial virtualisation setup and package configuration required troubleshooting. Hardware virtualisation support and the required QEMU/KVM and libvirt components were checked and configured.
•	Nextcloud initially returned HTTP errors during access. The application access path was investigated and the service was tested locally through an SSH tunnel.
•	Remote access to services required additional configuration. Tailscale and SSH were configured to provide private remote administration.
•	The public website continued to display an older stylesheet after deployment. The deployed files and HTTP responses were checked; cache-busting was used to confirm that the updated stylesheet could be retrieved.
•	Tailscale temporarily failed to connect on the TAFE Wi-Fi network. DNS and TLS connectivity were investigated. The issue appeared related to the network environment and later resolved without server configuration changes.
•	Cloud storage needed to be separated from the virtual machine's operating-system disk. An additional virtual disk was created, formatted with ext4 and configured to mount at /srv/cloud-data.

4. Skills and Knowledge Gained
•	Linux server installation and administration
•	Virtualisation using QEMU/KVM and libvirt
•	Docker and Docker Compose
•	Web hosting using Nginx
•	Self-hosted cloud services using Nextcloud
•	PostgreSQL database services and Redis caching
•	Network configuration and remote access
•	Tailscale and Cloudflare Tunnel
•	Git and GitHub version control
•	Jenkins CI/CD pipeline configuration
•	Deployment automation and technical troubleshooting
•	Infrastructure documentation and system architecture design

5. Future Improvements
•	Implement automated backups and regularly test restoration procedures.
•	Introduce server monitoring and resource-utilisation dashboards.
•	Configure separate cloud user accounts and storage quotas.
•	Strengthen access controls and security monitoring.
•	Add automated website and application tests to the Jenkins pipeline.
•	Develop a centralised administration dashboard.
•	Expand the range of self-hosted services available through RootGrid.

6. Overall Conclusion
RootGrid demonstrates how an ordinary laptop can be transformed into a functional home-lab infrastructure capable of hosting public-facing and private services. The project brought together Linux server administration, virtualisation, web hosting, self-hosted cloud storage, secure remote access, version control and automated deployment in one working environment.
Its most important achievement is the integration of these technologies into a practical platform that gives the operator greater visibility and control over where applications run and where data is stored. RootGrid provides a foundation for further work in monitoring, backups, security and additional self-hosted services.
RootGrid is a working foundation for personal self-hosted infrastructure, built around the principle that people should have a meaningful choice to keep their digital services and data under their own control.
