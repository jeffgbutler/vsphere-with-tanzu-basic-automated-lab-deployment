| Setting                              | Value                              |
|--------------------------------------|------------------------------------|
| **Load Balancer**                    |                                    |
| Name                                 | tanzu-basic-haproxy                |
| Data Plane API                       | 192.168.136.116:5556               |
| User Name                            | wcp                                |
| Password                             | VMware1!                           |
| Load balancer workload address range | 192.168.137.64-192.168.137.127     |
| **Management Network**               |                                    |
| Network                              | DVPG-Supervisor-Management-Network |
| Management Network Starting IP       | 192.168.136.120                    |
| Subnet                               | 255.255.255.0                      |
| Gateway                              | 192.168.136.1                      |
| DNS                                  | 192.168.128.1                      |
| Search Domains                       | tanzubasic.tanzuathome.net         |
| NTP Server                           | pool.ntp.org                       |
| **Workload Network**                 |                                    |
| DNS                                  | 192.168.128.1                      |
| Port Group                           | DVPG-Workload-Network              |
| Gateway                              | 192.168.137.1                      |
| Subnet                               | 255.255.255.0                      |
| IP Address Ranges                    | 192.168.137.130-192.168.137.150    |

Network Notes:

- Management Network - The IP space for the supervisor cluster. (Supervisor nodes also grab IPs in the workload network)
- Workload Network - The IP space for K8S nodes
- Load Balancer (Frontend) Network - IP space for service type load balancer
