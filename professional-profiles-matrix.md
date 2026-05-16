# 📊 Professional Profiles & Self-Hosting Services Matrix

> **Comprehensive analysis mapping 14 professional profiles to self-hosted services**  
> Based on awesome-selfhosted data (100+ categories, 1000+ projects)

---

## Table of Contents

1. [Professional Profiles Matrix](#1-professional-profiles-matrix)
   - [DevOps Engineers](#devops-engineers)
   - [Security/Privacy Professionals](#securityprivacy-professionals)
   - [Web Developers](#web-developers)
   - [System Administrators](#system-administrators)
   - [Network Engineers](#network-engineers)
   - [Data Engineers](#data-engineers)
   - [Data Scientists/ML Engineers](#datascientistsml-engineers)
   - [IoT/Home Automation Enthusiasts](#iothome-automation-enthusiasts)
   - [Privacy Advocates](#privacy-advocates)
   - [Media Enthusiasts](#media-enthusiasts)
   - [Small Business Owners](#small-business-owners)
   - [Educators/Researchers](#educatorsresearchers)
   - [Students](#students)
   - [Career Shifters](#career-shifters)

2. [Comprehensive Service Catalog](#2-comprehensive-service-catalog)
   - [Development & DevOps](#development--devops)
   - [Security & Privacy](#security--privacy)
   - [Communication](#communication)
   - [Storage & File Management](#storage--file-management)
   - [Media](#media)
   - [Productivity](#productivity)
   - [IoT & Home Automation](#iot--home-automation)
   - [Database & Analytics](#database--analytics)
   - [AI/ML & GenAI](#aiml--genai)
   - [Web & CMS](#web--cms)
   - [Network Utilities](#network-utilities)

---

## 1. Professional Profiles Matrix

### DevOps Engineers

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Production infrastructure experience without cloud costs; CI/CD mastery; Infrastructure as Code practice |

#### Primary Use Cases
- **CI/CD Pipeline Development**: Build and test deployment pipelines before enterprise adoption
- **Kubernetes Orchestration**: Multi-node HA clusters with service mesh, ingress controllers
- **Monitoring & Observability**: Full-stack Prometheus/Grafana/Loki/Tempo observability
- **Infrastructure as Code**: Ansible, Terraform, Pulumi practice with real hardware
- **Service Mesh Testing**: Istio, Linkerd, Traefik service mesh configurations
- **GitOps Workflows**: ArgoCD, FluxCD continuous deployment patterns

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **CI/CD** | Jenkins, GitLab CI, Tekton, ArgoCD, Flux, GitHub Actions self-hosted runners | "Production-grade CI/CD pipeline design with GitOps patterns" |
| **Container Registry** | Harbor, Docker Registry, Quay, Nexus | "Private container registry with vulnerability scanning" |
| **Configuration Management** | Ansible, SaltStack, Puppet, Chef | "Infrastructure as Code with idempotent configuration" |
| **Monitoring** | Prometheus, Grafana, VictoriaMetrics, Thanos, Alertmanager | "Multi-metric time-series aggregation with custom alerting" |
| **Log Management** | Loki, ELK Stack, Graylog, Fluentd, Vector | "Centralized log aggregation with structured querying" |
| **Service Mesh** | Istio, Linkerd, Traefik, Caddy | "mTLS service-to-service communication with traffic splitting" |
| **Secrets Management** | Vault, Sealed Secrets, External Secrets Operator | "HashiCorp Vault integration with dynamic secrets" |
| **API Gateway** | Kong, Traefik, APISIX, Gloo | "API rate limiting, authentication, and routing" |

#### Skill Development Benefits
- Real-world experience with enterprise tools before job applications
- Understanding of failure modes through intentional chaos engineering
- Cost optimization skills (cloud vs. bare-metal TCO analysis)
- Documentation and runbook creation practice

#### Career Relevance
> **Interview Talking Point**: *"I designed a 3-node Kubernetes cluster with GitOps deployment, serving 20+ microservices with zero-downtime updates. The cluster uses Istio for service mesh, Prometheus for metrics collection (50GB/day), and ArgoCD for continuous deployment. I can demonstrate this architecture and walk through any incident response scenarios."*

---

### Security/Privacy Professionals

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Data sovereignty; security tool testing; privacy-first infrastructure; penetration testing sandbox |

#### Primary Use Cases
- **Security Tooling**: Self-hosted vulnerability scanners, SIEM, IDS/IPS
- **Privacy Infrastructure**: Encrypted communications, anonymous browsing, data minimization
- **Penetration Testing Lab**: Isolated networks for ethical hacking practice
- **Threat Intelligence**: Self-hosted threat feeds, honeypots, attack pattern analysis
- **Compliance Testing**: GDPR, HIPAA, SOC2 compliance practice environments

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Password Management** | Vaultwarden, Bitwarden, Passbolt, KeePassXC | "Enterprise password management with audit logging" |
| **VPN/Remote Access** | WireGuard, OpenVPN, Tailscale, ZeroTier | "Zero-trust network architecture with split tunneling" |
| **Firewall/Network Security** | pfSense, OPNsense, pfBlockerNG, Fail2ban | "Network segmentation with intrusion prevention" |
| **Honeypots** | Cowrie, Dionaea, Canarytokens, Glastopf | "Threat intelligence collection through honeypot deployment" |
| **SIEM** | Wazuh, Security Onion, ELK Stack for security | "Security event correlation with custom detection rules" |
| **Vulnerability Scanning** | OpenVAS, Nuclei, Trivy, Grype | "Automated vulnerability assessment and remediation tracking" |
| **Encryption** | Nextcloud with encryption, Cryptomator, VeraCrypt | "End-to-end encrypted data storage with key management" |
| **Anonymous Communication** | Tor hidden services, I2P, Matrix with PGP | "Privacy-preserving communication infrastructure" |
| **Whistleblowing** | GlobaLeaks | "Secure drop infrastructure for sensitive communications" |

#### Skill Development Benefits
- Hands-on experience with security tools used in enterprise
- Understanding of attack vectors through defensive implementation
- Incident response practice through simulated attacks
- Security audit and compliance documentation

#### Career Relevance
> **Interview Talking Point**: *"I built a security operations center in my homelab with Wazuh SIEM collecting logs from 15+ services, WireGuard for secure remote access, and a honeypot network that has captured 500+ attack attempts. I can demonstrate my incident response playbooks and threat hunting methodology."*

---

### Web Developers

| Aspect | Details | Details |
|--------|---------|---------|
| **Primary Motivation** | Full-stack deployment experience; staging/production parity; portfolio hosting |

#### Primary Use Cases
- **Full-Stack Deployment**: Deploy React/Vue/Angular frontends with Node/Python/Go backends
- **Database Management**: PostgreSQL, MySQL, MongoDB, Redis for application development
- **API Development**: REST and GraphQL API hosting with documentation
- **Static Site Generation**: Jekyll, Hugo, Next.js, Gatsby deployment
- **Microservices Architecture**: Multiple services communicating via API gateways
- **Development Environments**: Codespaces-like remote development setup

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Web Servers** | Nginx, Caddy, Apache, Traefik | "Reverse proxy configuration with SSL termination" |
| **Application Platforms** | Render alternative (Coolify), CapRover, Dokku | "Platform-as-a-Service for application deployment" |
| **Databases** | PostgreSQL, MySQL, MariaDB, MongoDB, Redis | "Database schema design with connection pooling" |
| **API Management** | PostgREST, Has GraphQL, Kong, APISIX | "Auto-generated REST APIs from PostgreSQL schemas" |
| **Static Sites** | Hugo, Jekyll, Gatsby, Next.js, Astro | "Static site generation with CI/CD deployment" |
| **Code Editors** | Code-server, Theia, OpenVSCode | "Browser-based IDE with extension support" |
| **Testing** | Selenium, Playwright, Cypress, TestContainers | "End-to-end testing with headless browsers" |
| **Domain Management** | Pi-hole, AdGuard Home, internal DNS | "Custom domain resolution with ad blocking" |

#### Skill Development Benefits
- End-to-end deployment experience from code to production
- Database optimization and migration practice
- API design and documentation best practices
- Performance optimization and caching strategies

#### Career Relevance
> **Interview Talking Point**: *"I deploy my full-stack applications on my homelab using a Docker Compose stack with Nginx reverse proxy, PostgreSQL with connection pooling, Redis caching, and automated deployments via GitHub Actions. My portfolio site has 99.9% uptime and loads in under 200ms globally."*

---

### System Administrators

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Enterprise sysadmin skills; virtualization mastery; backup/disaster recovery |

#### Primary Use Cases
- **Virtualization**: KVM, Proxmox, Incus/LXD for VM and container management
- **Backup Systems**: Automated backup with verification and offsite replication
- **User Management**: LDAP, Active Directory alternatives, SSO implementation
- **Patch Management**: Automated OS and application updates
- **Resource Monitoring**: Capacity planning and performance optimization
- **Disaster Recovery**: Backup verification, restoration drills, failover testing

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Virtualization** | Proxmox VE, Incus/LXD, KVM, QEMU | "Type-1 and Type-2 hypervisor management" |
| **Backup** | Restic, BorgBackup, Kopia, Duplicati, UrBackup | "Incremental encrypted backups with deduplication" |
| **Directory Services** | FreeIPA, Samba AD, OpenLDAP, Keycloak | "Centralized authentication with group policy" |
| **File Services** | Samba, NFS, AFP, Nextcloud | "Cross-platform file sharing with permission management" |
| **Monitoring** | Zabbix, Nagios, Icinga, Prometheus Node Exporter | "Infrastructure monitoring with custom alerting" |
| **Configuration** | Ansible, SaltStack, Puppet | "Automated configuration management for 50+ nodes" |
| **Ticketing** | OTRS, Request Tracker, Znuny | "IT service management with SLA tracking" |
| **Documentation** | BookStack, Wiki.js, DokuWiki | "IT documentation with version control" |

#### Skill Development Benefits
- Real-world experience with enterprise sysadmin tools
- Understanding of backup strategies and recovery time objectives
- Capacity planning and resource optimization
- Documentation and knowledge base creation

#### Career Relevance
> **Interview Talking Point**: *"I manage a virtualized infrastructure with Proxmox hosting 20+ VMs and containers. My backup strategy uses Restic with local and cloud replication, achieving RPO of 1 hour and RTO of 4 hours. I maintain complete documentation in BookStack and use Ansible for configuration management."*

---

### Network Engineers

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Network protocol mastery; routing/switching practice; SDN implementation |

#### Primary Use Cases
- **Network Segmentation**: VLANs, subnetting, network isolation
- **Routing Protocols**: BGP, OSPF, static routing practice
- **DNS Infrastructure**: Internal DNS with split-horizon, DNSSEC
- **Network Monitoring**: Traffic analysis, flow monitoring, packet capture
- **Wireless Networks**: Enterprise Wi-Fi with captive portals
- **Network Automation**: Python/Ansible for network device configuration

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Routing/Firewall** | pfSense, OPNsense, VyOS, FRRouting | "BGP/OSPF routing with policy-based routing" |
| **DNS** | Pi-hole, AdGuard Home, BIND, Unbound, CoreDNS | "Authoritative and recursive DNS with DNSSEC" |
| **DHCP** | dnsmasq, ISC DHCP, Kea | "Dynamic IP allocation with reservation management" |
| **Network Monitoring** | ntopng, Smokeping, Cacti, LibreNMS | "Network performance monitoring with historical trending" |
| **Packet Analysis** | Wireshark, tcpdump, Zeek, Suricata | "Deep packet inspection for traffic analysis" |
| **SDN** | Open vSwitch, OVN, OpenFlow | "Software-defined networking with flow programming" |
| **Wireless** | FreeRADIUS, pfSense Captive Portal | "Enterprise Wi-Fi with 802.1X authentication" |
| **Network Automation** | Nornir, Netmiko, Ansible Network | "Automated network device configuration" |

#### Skill Development Benefits
- Hands-on experience with routing protocols and network design
- Understanding of network security and segmentation
- Traffic analysis and troubleshooting skills
- Network automation and scripting

#### Career Relevance
> **Interview Talking Point**: *"I designed a multi-VLAN homelab with BGP routing between zones, DNSSEC-enabled internal DNS, and network monitoring with ntopng and LibreNMS. I use Ansible for network automation and have implemented 802.1X authentication for wireless access."*

---

### Data Engineers

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Big data processing experience; pipeline orchestration; data lake implementation |

#### Primary Use Cases
- **Batch Processing**: Spark, Flink, Dask for large-scale data processing
- **Stream Processing**: Kafka, Pulsar, Redpanda for real-time data
- **Data Warehousing**: ClickHouse, DuckDB, TimescaleDB for analytics
- **Pipeline Orchestration**: Airflow, Prefect, Dagster for workflow management
- **Data Quality**: Great Expectations, dbt tests for data validation
- **Data Catalog**: Metadata management and data lineage tracking

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Batch Processing** | Apache Spark, Dask, Ray | "Distributed data processing with 100GB+ datasets" |
| **Stream Processing** | Apache Kafka, Redpanda, Apache Pulsar | "Real-time event streaming with exactly-once semantics" |
| **Data Warehousing** | ClickHouse, DuckDB, TimescaleDB, Apache Druid | "Columnar storage for analytical queries" |
| **Orchestration** | Apache Airflow, Prefect, Dagster, Kestra | "DAG-based workflow orchestration with backfill" |
| **Data Transformation** | dbt, Dataform, SQLMesh | "Analytics engineering with version-controlled SQL" |
| **Object Storage** | MinIO, Ceph RGW, SeaweedFS | "S3-compatible object storage with lifecycle policies" |
| **Data Quality** | Great Expectations, Soda, Monte Carlo | "Data quality testing with automated alerting" |
| **Data Catalog** | DataHub, Amundsen, Metabase | "Data discovery with lineage tracking" |

#### Skill Development Benefits
- Production-scale data processing experience
- Understanding of distributed systems and fault tolerance
- Pipeline design patterns and best practices
- Data modeling and optimization

#### Career Relevance
> **Interview Talking Point**: *"I built a data platform processing 50GB daily with Spark on a 3-node cluster, orchestrated by Airflow with 30+ DAGs. The stack includes MinIO for object storage, ClickHouse for analytics, and Great Expectations for data quality. I can demonstrate my pipeline architecture and discuss optimization strategies."*

---

### Data Scientists/ML Engineers

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | ML model training/deployment; GPU resource access; MLOps practice |

#### Primary Use Cases
- **Model Training**: GPU-accelerated training with PyTorch/TensorFlow
- **Model Serving**: KServe, Seldon, Triton for production inference
- **Experiment Tracking**: MLflow, Weights & Biases self-hosted
- **Feature Stores**: Feast, Tecton for feature management
- **AutoML**: Auto-sklearn, H2O, FLAML for automated model development
- **LLM Deployment**: Ollama, vLLM, Text Generation Inference

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **ML Platforms** | MLflow, Kubeflow, ClearML | "End-to-end MLOps with experiment tracking" |
| **Model Serving** | KServe, Seldon Core, Triton Inference Server | "Production model serving with canary deployments" |
| **LLM Inference** | Ollama, vLLM, Text Generation Inference, LocalAI | "Large language model deployment with quantization" |
| **Vector Databases** | Qdrant, Weaviate, Chroma, Milvus | "Vector similarity search for RAG applications" |
| **Notebooks** | JupyterHub, Papermill, Deepnote self-hosted | "Collaborative notebook environment with resource isolation" |
| **AutoML** | H2O, Auto-sklearn, FLAML, PyCaret | "Automated model selection and hyperparameter tuning" |
| **Feature Stores** | Feast, Tecton, Hopsworks | "Feature management with point-in-time correctness" |
| **Data Labeling** | Label Studio, CVAT, Labelbox self-hosted | "Active learning pipelines with human-in-the-loop" |

#### Skill Development Benefits
- GPU resource management and optimization
- Model deployment patterns and A/B testing
- Understanding of inference optimization techniques
- MLOps best practices and tooling

#### Career Relevance
> **Interview Talking Point**: *"I deployed a local LLM infrastructure with Ollama serving Llama-3-8B with 4-bit quantization, integrated with a Qdrant vector database for RAG. My MLflow instance tracks 100+ experiments, and I use KServe for model serving with canary deployments. I can demonstrate my RAG pipeline and discuss optimization strategies."*

---

### IoT/Home Automation Enthusiasts

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Smart home control; device integration; privacy-preserving automation |

#### Primary Use Cases
- **Home Automation**: Home Assistant, OpenHAB for device control
- **Device Integration**: MQTT brokers for sensor/actuator communication
- **Voice Control**: Home Assistant Voice, Mycroft, Rhasspy
- **Energy Monitoring**: Home energy management and optimization
- **Security Systems**: IP camera integration, motion detection, alerts
- **Environmental Monitoring**: Temperature, humidity, air quality tracking

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Home Automation** | Home Assistant, OpenHAB, Domoticz | "IoT device integration with 100+ devices" |
| **MQTT Brokers** | Eclipse Mosquitto, EMQX, HiveMQ CE | "Message queuing for IoT device communication" |
| **Voice Assistants** | Home Assistant Voice, Rhasspy, Mycroft | "Privacy-preserving voice control with local NLP" |
| **Camera Systems** | Frigate, Shinobi, ZoneMinder, iSpy | "AI-powered object detection for security monitoring" |
| **Energy Monitoring** | Home Assistant Energy, Emoncms, OpenEnergyMonitor | "Energy consumption tracking with predictive analytics" |
| **Dashboard** | Grafana, Node-RED, Home Assistant Dashboards | "IoT data visualization with real-time updates" |
| **Automation** | Node-RED, Home Assistant Automations, HASS.io | "Event-driven automation with conditional logic" |
| **Firmware** | ESPHome, Tasmota, OpenTherm | "Custom IoT firmware for device control" |

#### Skill Development Benefits
- IoT protocol understanding (MQTT, Zigbee, Z-Wave)
- Edge computing and local processing
- Real-time data processing
- Integration of heterogeneous systems

#### Career Relevance
> **Interview Talking Point**: *"I built a smart home with Home Assistant integrating 150+ devices across Zigbee, Z-Wave, and Wi-Fi protocols. My MQTT broker handles 1000+ messages/minute, and I use ESPHome for custom device firmware. The system processes all data locally with no cloud dependency."*

---

### Privacy Advocates

| Aspect | Details | Details |
|--------|---------|---------|
| **Primary Motivation** | Data sovereignty; surveillance resistance; digital rights |

#### Primary Use Cases
- **Communication Privacy**: Encrypted email, chat, video conferencing
- **Data Minimization**: Self-hosted alternatives to tracking services
- **Anonymous Browsing**: Tor, privacy-focused search engines
- **Identity Management**: Self-sovereign identity, decentralized authentication
- **Financial Privacy**: Crypto wallets, privacy-focused banking

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Email** | Mailcow, Mailu, Postfix + Dovecot + SpamAssassin | "Self-hosted email with DKIM, DMARC, SPF" |
| **Chat** | Matrix/Synapse, XMPP + Prosody, Signal (self-hosted relay) | "End-to-end encrypted communication" |
| **Video** | Jitsi Meet, BigBlueButton, PeerJS | "Privacy-preserving video conferencing" |
| **Search** | SearXNG, Whoogle, Startpage self-hosted | "Privacy-focused search without tracking" |
| **Identity** | Keycloak, Authelia, Authentik, Ory | "Single sign-on with OIDC/SAML integration" |
| **File Sharing** | Nextcloud, Syncthing, Resilio Sync | "Encrypted file synchronization" |
| **Password Managers** | Vaultwarden, Bitwarden, Passbolt | "Self-hosted password management" |
| **Analytics** | Plausible, Matomo, Umami | "Privacy-respecting web analytics" |

#### Skill Development Benefits
- Understanding of encryption and privacy technologies
- System hardening and security best practices
- Digital sovereignty implementation
- Threat modeling and risk assessment

#### Career Relevance
> **Interview Talking Point**: *"I built a privacy-first infrastructure with self-hosted email (Mailcow), Matrix for encrypted chat, Jitsi for video calls, and SearXNG for search. All services use end-to-end encryption, and I've implemented a zero-knowledge architecture for data storage. This demonstrates my understanding of privacy engineering principles."*

---

### Media Enthusiasts

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Media library management; transcoding; content automation |

#### Primary Use Cases
- **Video Streaming**: Personal Netflix with movies/TV shows
- **Music Streaming**: Self-hosted Spotify/Apple Music alternative
- **Live TV**: DVR functionality with channel surfing
- **Media Automation**: Automatic downloading and organizing
- **Transcoding**: Hardware-accelerated format conversion
- **Metadata Management**: Poster art, descriptions, ratings

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Video Streaming** | Plex, Jellyfin, Emby | "Media server with hardware transcoding" |
| **Music Streaming** | Navidrome, Subsonic, Airsonic | "Music streaming with offline playback" |
| **Podcast Management** | Podcast Addict self-hosted, Podgrab | "Podcast automation and distribution" |
| **Download Automation** | Sonarr, Radarr, Lidarr, Prowlarr | "Automated media acquisition with indexer integration" |
| **Live TV/DVR** | Tvheadend, NextPVR, Channels DVR | "OTA TV recording with EPG integration" |
| **Transcoding** | HandBrake, FFmpeg, Jellyfin Transcoding | "Hardware-accelerated video transcoding" |
| **Metadata** | Tautulli, Plex Meta Manager | "Media library analytics and automation" |
| **Comic Books** | Komga, Calibre-web | "Comic and ebook library management" |

#### Skill Development Benefits
- Media encoding and transcoding knowledge
- Network storage optimization for large files
- Hardware acceleration (GPU transcoding)
- Content management workflows

#### Career Relevance
> **Interview Talking Point**: *"I built a media server infrastructure with Jellyfin serving 20TB of content, hardware-accelerated transcoding on an RTX 3090, and automated media acquisition via Sonarr/Radarr. The system handles 10+ concurrent streams with adaptive bitrate streaming."*

---

### Small Business Owners

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Cost reduction; data control; business continuity |

#### Primary Use Cases
- **Document Management**: Shared drives, version control, collaboration
- **CRM/ERP**: Customer and inventory management
- **Communication**: Business email, team chat, video meetings
- **Accounting**: Invoicing, expense tracking, financial reports
- **Project Management**: Task tracking, team collaboration
- **Website Hosting**: Business website, e-commerce, blog

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **CRM** | SuiteCRM, Vtiger, EspoCRM, CrmOnTap | "Customer relationship management with sales pipelines" |
| **ERP** | Odoo, ERPNext, Dolibarr | "Enterprise resource planning with inventory management" |
| **Document Management** | Nextcloud, ownCloud, Seafile | "Document collaboration with version control" |
| **Accounting** | Akaunting, Invoice Ninja, GnuCash | "Financial management with invoicing and reporting" |
| **Project Management** | Taiga, Leantime, OpenProject, Focalboard | "Agile project management with sprint planning" |
| **E-commerce** | WooCommerce, Shopware, PrestaShop, Saleor | "E-commerce platform with payment integration" |
| **HR Management** | OrangeHRM, Sentrifugo, OrangeHRM | "Human resources management with payroll" |
| **Knowledge Base** | BookStack, Wiki.js, XWiki | "Company documentation and knowledge management" |

#### Skill Development Benefits
- Business process automation
- Cost-benefit analysis of self-hosting vs. SaaS
- Data governance and compliance
- Vendor management and migration strategies

#### Career Relevance
> **Interview Talking Point**: *"I deployed a complete business infrastructure on-premise with Odoo ERP managing inventory and accounting, Nextcloud for document collaboration, and SuiteCRM for customer management. This reduced our SaaS costs by 60% while maintaining full data control and achieving GDPR compliance."*

---

### Educators/Researchers

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Educational content delivery; research data management; collaboration |

#### Primary Use Cases
- **Learning Management**: Course materials, assignments, grading
- **Research Data**: Secure storage, sharing, version control
- **Collaboration**: Team research, document co-authoring
- **Publication**: Academic blog, preprint server, data repository
- **Simulation**: Computational resources for research
- **Library**: Digital collections, e-books, journals

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **LMS** | Moodle, Canvas self-hosted, Chamilo | "Learning management with assignment grading" |
| **Research Data** | Dataverse, CKAN, Zenodo self-hosted | "Research data repository with DOI assignment" |
| **Collaboration** | Nextcloud Collaborate, CryptPad | "Real-time document collaboration" |
| **Publications** | Overleaf (self-hosted LaTeX), Sci-Hub mirror | "Academic publishing with version control" |
| **E-books** | Calibre-Web, Komga, Feedr | "Digital library management" |
| **Surveys** | LimeSurvey, Formspree self-hosted | "Research survey collection with data export" |
| **Reference Management** | Paperless-ngx, Zotero server | "Bibliography management with PDF organization" |
| **Statistical Analysis** | RStudio Server, JupyterHub, SageMath | "Statistical computing with collaborative notebooks" |

#### Skill Development Benefits
- Educational technology implementation
- Research data management best practices
- Open access publishing
- Academic collaboration tools

#### Career Relevance
> **Interview Talking Point**: *"I deployed a Moodle LMS serving 500+ students with integrated plagiarism detection, a Dataverse research data repository with DOI assignment, and JupyterHub for collaborative data analysis. This infrastructure supports both teaching and research with full data sovereignty."*

---

### Students

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Learning infrastructure; portfolio building; cost-effective tools |

#### Primary Use Cases
- **Study Tools**: Note-taking, flashcards, productivity
- **Development Practice**: Coding environment, version control
- **Collaboration**: Group projects, study groups
- **Portfolio**: Personal website, project showcase
- **Learning Resources**: Course hosting, documentation
- **Budget Tools**: Free alternatives to expensive software

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Note-taking** | Joplin, Logseq, Obsidian (self-sync), Trilium | "Personal knowledge management with linking" |
| **Flashcards** | Anki sync server, Mochi | "Spaced repetition for learning" |
| **Development** | GitLab, Gitea, Forgejo, Code-server | "Version control and remote development" |
| **Portfolio** | Hugo, Jekyll, WordPress, Ghost | "Personal website with static site generation" |
| **Productivity** | Todoist self-hosted (Vikunja), Notion alternative (AppFlowy) | "Task management and productivity tracking" |
| **Video Learning** | PeerTube, MediaCMS | "Video hosting for tutorials and lectures" |
| **Cloud IDE** | Gitpod self-hosted, Theia, Code-server | "Browser-based development environments" |
| **Documentation** | MkDocs, Docusaurus, Docsify | "Technical documentation with search" |

#### Skill Development Benefits
- Infrastructure skills without cloud costs
- Portfolio development for job applications
- Self-directed learning and problem-solving
- Open source contribution experience

#### Career Relevance
> **Interview Talking Point**: *"I built my homelab on a Raspberry Pi cluster for under $200, hosting GitLab for version control, Code-server for remote development, and a Hugo portfolio site. This infrastructure has helped me learn DevOps practices and build a portfolio that demonstrates practical skills to recruiters."*

---

### Career Shifters

| Aspect | Details |
|--------|---------|
| **Primary Motivation** | Skill demonstration; portfolio proof; confidence building |

#### Primary Use Cases
- **Skill Validation**: Hands-on practice with industry tools
- **Portfolio Development**: Tangible projects for interviews
- **Certification Prep**: Lab environment for exam practice
- **Networking**: Community engagement, knowledge sharing
- **Confidence Building**: "I can do this" through successful deployments
- **Career Transition**: Demonstrating skills without traditional experience

#### Key Services/Applications

| Category | Tools | CV-Relevant Skills |
|----------|-------|-------------------|
| **Learning Platforms** | Open edX, Kanboard, Moodle | "Self-paced learning with progress tracking" |
| **Portfolio Hosting** | GitHub Pages, Netlify self-hosted, Hugo | "Static site deployment with CI/CD" |
| **Certification Labs** | Kubernetes (k3s), AWS local (LocalStack) | "Certification preparation in isolated environment" |
| **Community** | Discourse, Flarum, NodeBB | "Community forum management and engagement" |
| **Documentation** | Notion alternative, Obsidian publish | "Knowledge base creation and sharing" |
| **Video Tutorials** | PeerTube, MediaCMS | "Video content creation and hosting" |
| **Blog** | Ghost, WordPress, WriteFreely | "Technical blogging and thought leadership" |
| **Analytics** | Plausible, Umami | "Website analytics for portfolio traffic" |

#### Skill Development Benefits
- Practical experience with enterprise tools
- Portfolio pieces that demonstrate competency
- Confidence through successful deployments
- Community engagement and networking

#### Career Relevance
> **Interview Talking Point**: *"As a career shifter, I built a homelab to demonstrate skills I couldn't get through traditional employment. My portfolio shows a 3-node Kubernetes cluster, CI/CD pipelines, and data processing workflows. This practical experience helped me land my first DevOps role despite having no prior professional experience."*

---

## 2. Comprehensive Service Catalog

### Development & DevOps

#### CI/CD & Continuous Deployment

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Jenkins** | Industry-standard automation server with extensive plugin ecosystem | MIT | Medium |
| **GitLab CI** | Integrated CI/CD within GitLab, excellent for monorepos | MIT | Medium |
| **Tekton** | Kubernetes-native CI/CD with cloud-native patterns | Apache-2.0 | High |
| **ArgoCD** | GitOps continuous deployment for Kubernetes | Apache-2.0 | Medium |
| **FluxCD** | GitOps tool for declarative continuous delivery | Apache-2.0 | Medium |
| **Drone** | Container-native CI/CD with simple YAML configuration | Apache-2.0 | Low |
| **Woodpecker** | Fork of Drone with active development | MIT | Low |
| **Gitea Actions** | GitHub Actions compatible for Gitea | MIT | Medium |

#### Monitoring & Observability

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Prometheus** | Time-series database with powerful query language (PromQL) | Apache-2.0 | Medium |
| **Grafana** | Visualization and analytics platform | AGPL-3.0 | Low |
| **VictoriaMetrics** | High-performance Prometheus alternative | GPL-3.0 | Medium |
| **Thanos** | Horizontal Prometheus with long-term storage | Apache-2.0 | High |
| **Loki** | Log aggregation inspired by Prometheus | AGPL-3.0 | Medium |
| **Tempo** | Distributed tracing backend | AGPL-3.0 | Medium |
| **Zabbix** | Enterprise-grade monitoring with auto-discovery | GPL-2.0 | High |
| **Nagios** | Classic monitoring with extensive plugin support | GPL-2.0 | Medium |
| **Middleware** | DORA metrics engineering effectiveness | Apache-2.0 | Low |

#### API Management

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Kong** | Cloud-native API gateway with plugin ecosystem | Apache-2.0 | Medium |
| **Traefik** | Cloud-native reverse proxy with automatic service discovery | MIT | Low |
| **APISIX** | High-performance API gateway with dynamic routing | Apache-2.0 | Medium |
| **Gloo** | Kubernetes-native API gateway based on Envoy | Apache-2.0 | High |
| **PostgREST** | Auto-generated REST API from PostgreSQL schema | PostgreSQL | Low |
| **Hasura** | GraphQL layer over PostgreSQL with real-time subscriptions | Apache-2.0 | Medium |
| **gRPC Gateway** | gRPC to JSON proxy with automatic documentation | BSD-3-Clause | Medium |

#### Project Management

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Taiga** | Agile project management for startups | GPLv3 | Low |
| **OpenProject** | Enterprise project management with Gantt charts | GPL-3.0 | Medium |
| **Leantime** | Project management for non-project managers | GPL-2.0 | Low |
| **Focalboard** | Notion/Trello alternative by Mattermost | MIT | Low |
| **Vikunja** | Task management with Gantt and todo lists | GPL-3.0 | Low |
| **Kanboard** | Minimal Kanban board project management | MIT | Low |
| **Redmine** | Flexible project management with wiki | GPL-2.0 | Medium |
| **Plane** | Notion/Jira alternative with issues, cycles, modules | Apache-2.0 | Medium |

---

### Security & Privacy

#### Password Managers

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Vaultwarden** | Rust implementation of Bitwarden API | Apache-2.0 | Low |
| **Bitwarden** | Official self-hosted password manager | AGPL-3.0 | Medium |
| **Passbolt** | Password manager for teams with GPG encryption | AGPL-3.0 | Medium |
| **Keeweb** | Local-first password manager with cloud sync | MIT | Low |

#### VPN & Remote Access

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **WireGuard** | Modern VPN with kernel module for performance | GPL-2.0 | Low |
| **OpenVPN** | Mature VPN solution with extensive documentation | AGPL-2.0 | Medium |
| **Tailscale** | Zero-config Mesh VPN based on WireGuard | Apache-2.0 | Low |
| **ZeroTier** | Software-defined networking with easy setup | GPL-2.0 | Low |
| **Cloudflare Tunnel** | Secure tunnel without public IPs | Proprietary | Low |

#### Honeypots & Threat Intelligence

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Cowrie** | SSH/Telnet honeypot for attack logging | GPL-3.0 | Low |
| **Dionaea** | Malware capture honeypot | GPL-2.0 | Medium |
| **Canarytokens** | Intrusion detection traps | GPL-3.0 | Low |
| **Glastopf** | Web application honeypot | GPL-3.0 | Low |
| **Conpot** | ICS/SCADA honeypot | GPL-3.0 | Medium |

#### Security Information & Event Management (SIEM)

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Wazuh** | XDR and SIEM with vulnerability detection | AGPL-3.0 | High |
| **Security Onion** | Complete intrusion detection platform | GPL-2.0 | High |
| **ELK Stack** | Elastic Search, Logstash, Kibana for security analytics | SSPL | High |
| **Graylog** | Log management with security focus | GPL-2.0 | Medium |

#### Encryption & Privacy

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Cryptomator** | Client-side encryption for cloud storage | BSD-3-Clause | Low |
| **VeraCrypt** | On-the-fly encryption for volumes | Apache-2.0 | Low |
| **Age** | Modern encryption tool with simple design | BSD-3-Clause | Low |
| **GPG Suite** | OpenPGP implementation for all platforms | GPL-3.0 | Low |

---

### Communication

#### Email Solutions

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Mailcow** | Dockerized mail suite with modern UI | GPL-3.0 | Medium |
| **Mailu** | Simple mail server as Docker images | MIT | Medium |
| **iRedMail** | Full-featured mail server installation script | GPL-3.0 | Medium |
| **Maddy Mail Server** | All-in-one mail server replacing Postfix/Dovecot | GPL-3.0 | Medium |
| **Modoboa** | Mail hosting with modern web interface | ISC | Medium |
| **Stalwart** | Modern JMAP/IMAP/SMTP server in Rust | AGPL-3.0 | Medium |

#### Chat & Messaging

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Matrix/Synapse** | Decentralized real-time communication | Apache-2.0 | Medium |
| **Element** | Matrix client for web, iOS, Android | Apache-2.0 | Low |
| **Rocket.Chat** | Team communication with integrations | MIT | Medium |
| **Zulip** | Open-source Slack alternative with threading | Apache-2.0 | High |
| **Mattermost** | Slack alternative for enterprises | AGPL-3.0 | Medium |
| **Conduit** | Rust-based Matrix homeserver | Apache-2.0 | Low |
| **Gotify** | Notification server with Android client | MIT | Low |
| **ntfy** | Simple push notifications via HTTP | Apache-2.0 | Low |

#### Video Conferencing

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Jitsi Meet** | Video conferencing with no account required | Apache-2.0 | Medium |
| **BigBlueButton** | Web conferencing for education | LGPL-3.0 | High |
| **LiveKit** | Real-time audio/video with WebRTC | Apache-2.0 | Medium |
| **Mediasoup** | SFU multimedia router for WebRTC | ISC | High |

#### Forums & Social

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Discourse** | Modern forum platform by StackOverflow founder | GPL-2.0 | High |
| **Flarum** | Beautiful and lightweight forum software | MIT | Low |
| **NodeBB** | Forum built on Node.js with real-time updates | GPL-3.0 | Medium |
| **Mastodon** | Decentralized Twitter alternative | AGPL-3.0 | Medium |
| **PeerTube** | Decentralized video hosting | AGPL-3.0 | Medium |

---

### Storage & File Management

#### Cloud Storage

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Nextcloud** | Complete productivity platform with file sync | AGPL-3.0 | Medium |
| **ownCloud** | Enterprise file sync and share | AGPL-3.0 | Medium |
| **Seafile** | High-performance file sync with reliability | GPL-2.0 | Low |
| **Pydio** | Enterprise file sharing and collaboration | AGPL-3.0 | Medium |
| **Filebrowser** | Single-user file manager for web | Apache-2.0 | Low |

#### Object Storage

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **MinIO** | High-performance S3-compatible object storage | AGPL-3.0 | Low |
| **Ceph RGW** | Distributed object storage with S3/Swift API | LGPL-2.1 | High |
| **SeaweedFS** | Fast distributed storage for blobs | Apache-2.0 | Medium |
| **Garage** | S3-compatible object storage for self-hosting | AGPL-3.0 | Low |

#### Backup Solutions

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Restic** | Fast, secure, deduplicated backup | BSD-2-Clause | Low |
| **BorgBackup** | Deduplicating backup with compression | BSD-3-Clause | Low |
| **Kopia** | Cross-platform backup with GUI | Apache-2.0 | Low |
| **Duplicati** | Encrypted backup for Windows/Mac/Linux | LGPL-3.0 | Low |
| **UrBackup** | Client/server backup system | MPL-2.0 | Medium |
| **Proxmox Backup Server** | Deduplicated backup for Proxmox | AGPL-3.0 | Medium |

#### File Transfer

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **FilePizza** | Peer-to-peer file transfer in browser | MIT | Low |
| **Transfer.sh** | Temporary file sharing service | MIT | Low |
| **FreshPM** | File drop with authentication | MIT | Low |
| **Syncthing** | Continuous file synchronization | MPL-2.0 | Low |

---

### Media

#### Video Streaming

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Jellyfin** | Free software media server with transcoding | GPL-2.0 | Low |
| **Plex** | Media server with optional premium features | Proprietary | Low |
| **Emby** | Media server with hardware transcoding | Proprietary | Low |
| **Navidrome** | Subsonic-compatible music streaming | GPL-3.0 | Low |

#### Media Automation

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Sonarr** | TV show automatic downloader | GPL-3.0 | Low |
| **Radarr** | Movie automatic downloader | GPL-3.0 | Low |
| **Lidarr** | Music automatic downloader | GPL-3.0 | Low |
| **Readarr** | Book automatic downloader | GPL-3.0 | Low |
| **Prowlarr** | Indexer manager for *arr stack | GPL-3.0 | Low |
| **Bazarr** | Subtitle automatic downloader | GPL-3.0 | Low |

#### Live TV & DVR

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Tvheadend** | TV streaming and recording | GPL-3.0 | Medium |
| **NextPVR** | Personal video recorder application | GPL-3.0 | Medium |
| **ChannelStore** | Channel guide and EPG management | MIT | Low |

#### Audio Streaming

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Navidrome** | Modern music server and streamer | GPL-3.0 | Low |
| **Airsonic** | Web-based media streamer | GPL-3.0 | Medium |
| **Subsonic** | Original music streaming server | GPL-3.0 | Medium |
| **Audiobookshelf** | Self-hosted audiobook and podcast server | GPL-3.0 | Low |

---

### Productivity

#### Calendar & Contacts

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Radicale** | Simple CalDAV/CardDAV server | GPL-3.0 | Low |
| **Baïkal** | Lightweight CalDAV/CardDAV server | GPL-3.0 | Low |
| **DAViCal** | Full-featured CalDAV server | GPL-2.0 | Medium |
| **SabreDAV** | CalDAV/CardDAV framework | MIT | Medium |

#### Note-taking & Knowledge

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Joplin** | Note-taking with end-to-end encryption | MIT | Low |
| **Logseq** | Privacy-first knowledge base | GPL-3.0 | Low |
| **Trilium** | Hierarchical note-taking application | AGPL-3.0 | Low |
| **Outline** | Wiki/knowledge base for teams | AGPL-3.0 | Medium |
| **AppFlowy** | Notion alternative built with Flutter | AGPL-3.0 | Low |

#### Task Management

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Vikunja** | Task management with multiple views | GPL-3.0 | Low |
| **Focalboard** | Trello/Notion alternative | MIT | Low |
| **Planka** | Trello alternative with real-time updates | AGPL-3.0 | Low |
| **Todo.txt** | Simple todo list format | MIT | Low |

#### Office Suites

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **OnlyOffice** | Full office suite with collaboration | AGPL-3.0 | Medium |
| **Collabora Online** | LibreOffice in the browser | MPL-2.0 | Medium |
| **CryptPad** | Encrypted real-time collaboration | AGPL-3.0 | Medium |

---

### IoT & Home Automation

#### Home Automation Platforms

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Home Assistant** | Most popular open source home automation | Apache-2.0 | Low |
| **OpenHAB** | Vendor-agnostic home automation | EPL-2.0 | Medium |
| **Domoticz** | Simple home automation system | GPL-3.0 | Low |
| **Homebridge** | HomeKit support for non-HomeKit devices | MIT | Low |

#### MQTT Brokers

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Eclipse Mosquitto** | Lightweight MQTT broker | EPL-2.0 | Low |
| **EMQX** | Scalable IoT message broker | Apache-2.0 | Medium |
| **HiveMQ CE** | Enterprise-grade MQTT broker | Apache-2.0 | Medium |
| **VerneMQ** | Distributed MQTT message broker | Apache-2.0 | Medium |

#### IoT Dashboards

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Node-RED** | Event-driven wiring for IoT | Apache-2.0 | Low |
| **Grafana IoT** | Grafana with IoT data sources | AGPL-3.0 | Medium |
| **ThingsBoard** | IoT platform with device management | Apache-2.0 | Medium |
| **Blynk** | IoT dashboard with mobile apps | Apache-2.0 | Medium |

#### Smart Home Security

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Frigate** | NVR with real-time object detection | MIT | Medium |
| **Shinobi** | CCTV software with motion detection | AGPL-3.0 | Medium |
| **ZoneMinder** | Full-featured CCTV system | GPL-2.0 | High |
| **iSpy** | Cross-platform surveillance software | AGPL-3.0 | Medium |

---

### Database & Analytics

#### Relational Databases

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **PostgreSQL** | Advanced relational database | PostgreSQL | Low |
| **MySQL** | Popular relational database | GPL-2.0 | Low |
| **MariaDB** | MySQL fork with additional features | GPL-2.0 | Low |
| **SQLite** | Embedded relational database | Public Domain | Low |

#### Time-Series Databases

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **InfluxDB** | Time-series database for metrics | MIT | Low |
| **TimescaleDB** | Time-series extension for PostgreSQL | BSD-2-Clause | Low |
| **QuestDB** | High-performance time-series database | Apache-2.0 | Low |
| **Prometheus** | Time-series database for monitoring | Apache-2.0 | Medium |

#### Columnar/Analytics Databases

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **ClickHouse** | Fast columnar OLAP database | Apache-2.0 | Medium |
| **DuckDB** | In-process analytical database | MIT | Low |
| **Apache Druid** | Real-time analytics database | Apache-2.0 | High |
| **Apache Pinot** | Real-time distributed OLAP datastore | Apache-2.0 | High |

#### NoSQL Databases

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **MongoDB** | Document-oriented NoSQL database | SSPL | Low |
| **Redis** | In-memory data structure store | BSD-3-Clause | Low |
| **Cassandra** | Distributed NoSQL database | Apache-2.0 | High |
| **RethinkDB** | Open-source document database | Apache-2.0 | Medium |

#### Business Intelligence

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Metabase** | Simple business analytics and dashboards | AGPL-3.0 | Low |
| **Redash** | Query and visualize data | BSD-2-Clause | Medium |
| **Superset** | Data exploration and visualization | Apache-2.0 | Medium |
| **Lightdash** | BI tool built on top of dbt | GPL-3.0 | Medium |
| **Apache Superset** | Enterprise data exploration | Apache-2.0 | Medium |

---

### AI/ML & GenAI

#### LLM Inference

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Ollama** | Simple LLM inference with model library | MIT | Low |
| **vLLM** | High-throughput LLM inference | Apache-2.0 | Medium |
| **Text Generation Inference** | Optimized LLM inference by HuggingFace | Apache-2.0 | Medium |
| **LocalAI** | OpenAI API compatible local inference | MIT | Low |
| **llama.cpp** | Efficient LLM inference in C++ | MIT | Low |

#### Vector Databases

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Qdrant** | Vector similarity search engine | Apache-2.0 | Low |
| **Weaviate** | Vector search with ML models | BSD-3-Clause | Medium |
| **Chroma** | AI-native embedding database | Apache-2.0 | Low |
| **Milvus** | Cloud-native vector database | Apache-2.0 | Medium |
| **Pgvector** | Vector similarity for PostgreSQL | Apache-2.0 | Low |

#### MLOps Platforms

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **MLflow** | Machine learning lifecycle platform | Apache-2.0 | Medium |
| **Kubeflow** | ML on Kubernetes | Apache-2.0 | High |
| **ClearML** | MLOps platform with experiment tracking | Apache-2.0 | Medium |
| **Weights & Biases** | Experiment tracking and model management | Proprietary | Low |

#### RAG & Knowledge

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **LangChain** | LLM application framework | MIT | Medium |
| **LlamaIndex** | Data framework for LLM applications | MIT | Medium |
| **Dify** | LLM app development platform | Apache-2.0 | Medium |
| **Flowise** | Drag-and-drop LLM flow builder | MIT | Low |

#### Computer Vision

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **OpenCV** | Computer vision library | Apache-2.0 | Medium |
| **YOLO** | Real-time object detection | AGPL-3.0 | Medium |
| **Stable Diffusion** | Image generation model | CreativeML Open RAIL-M | Medium |

---

### Web & CMS

#### Blogging Platforms

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Ghost** | Modern publishing platform | MIT | Low |
| **WordPress** | Most popular CMS with blogging | GPL-2.0 | Low |
| **Hugo** | Fast static site generator | Apache-2.0 | Low |
| **Jekyll** | Static site generator by GitHub | MIT | Low |
| **WriteFreely** | Minimal federated blogging | AGPL-3.0 | Low |
| **Plume** | Federated blogging platform | AGPL-3.0 | Medium |

#### Content Management Systems

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Strapi** | Headless CMS with REST/GraphQL | MIT | Medium |
| **Directus** | Headless CMS wrapping SQL database | GPL-3.0 | Medium |
| **Keystone** | Headless CMS built on GraphQL | MIT | Medium |
| **Cockpit** | Simple headless CMS | MIT | Low |
| **Grav** | Flat-file CMS with modern features | MIT | Low |

#### Static Site Generators

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Hugo** | Fastest static site generator | Apache-2.0 | Low |
| **Jekyll** | GitHub Pages default generator | MIT | Low |
| **Gatsby** | React-based static site generator | MIT | Medium |
| **Next.js** | React framework for static/SSR | MIT | Medium |
| **Astro** | Modern static site generator | MIT | Low |
| **Zola** | Fast static site generator in Rust | MIT | Low |

#### E-commerce

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **WooCommerce** | WordPress e-commerce plugin | GPL-3.0 | Low |
| **Shopware** | Modern e-commerce platform | MIT | Medium |
| **PrestaShop** | Full-featured e-commerce solution | OSL-3.0 | Medium |
| **Saleor** | GraphQL-first e-commerce platform | BSD-3-Clause | Medium |
| **Medusa** | Headless commerce platform | MIT | Medium |

---

### Network Utilities

#### DNS Servers

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Pi-hole** | Network-wide ad blocking with DNS | EUPL-1.2 | Low |
| **AdGuard Home** | DNS sinkhole with parental control | GPL-3.0 | Low |
| **BIND** | Industry-standard DNS server | MPL-2.0 | High |
| **Unbound** | Validating recursive DNS resolver | BSD-3-Clause | Medium |
| **CoreDNS** | Cloud-native DNS server | Apache-2.0 | Medium |
| **PowerDNS** | High-performance DNS server | GPL-2.0 | Medium |

#### Reverse Proxies

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Nginx** | High-performance web server and proxy | BSD-2-Clause | Medium |
| **Caddy** | Automatic HTTPS web server | Apache-2.0 | Low |
| **Traefik** | Cloud-native reverse proxy | MIT | Low |
| **HAProxy** | High availability load balancer | GPL-2.0 | Medium |
| **Envoy** | Cloud-native edge/service proxy | Apache-2.0 | High |

#### Remote Access

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **Tailscale** | Zero-config mesh VPN | Apache-2.0 | Low |
| **Headscale** | Open-source Tailscale control server | GPL-3.0 | Low |
| **MeshCentral** | Full remote management | Apache-2.0 | Low |
| **Apache Guacamole** | Clientless remote desktop gateway | Apache-2.0 | Medium |
| **RustDesk** | Open-source TeamViewer alternative | AGPL-3.0 | Low |

#### Network Monitoring

| Tool | Description | License | Complexity |
|------|-------------|---------|------------|
| **ntopng** | Network traffic probe and analyzer | GPL-3.0 | Medium |
| **Smokeping** | Network latency monitoring | GPL-2.0 | Low |
| **LibreNMS** | Auto-discovering network monitoring | GPL-2.0 | Medium |
| **Cacti** | Network graphing solution | GPL-2.0 | Medium |
| **Checkmk** | Comprehensive monitoring solution | GPL-2.0 | High |

---

## Quick Reference: Profile-to-Service Mapping

| Profile | Top 5 Essential Services |
|---------|-------------------------|
| **DevOps Engineer** | Kubernetes (k3s), ArgoCD, Prometheus, Grafana, Harbor |
| **Security Professional** | Wazuh, WireGuard, Vaultwarden, pfSense, Cowrie |
| **Web Developer** | PostgreSQL, Redis, Nginx, GitLab, Code-server |
| **System Administrator** | Proxmox, Restic, FreeIPA, Ansible, Zabbix |
| **Network Engineer** | pfSense, Pi-hole, ntopng, OpenWRT, LibreNMS |
| **Data Engineer** | Apache Spark, Airflow, MinIO, ClickHouse, dbt |
| **Data Scientist** | Ollama, MLflow, Qdrant, JupyterHub, KServe |
| **IoT Enthusiast** | Home Assistant, Mosquitto, Node-RED, Frigate, ESPHome |
| **Privacy Advocate** | Mailcow, Matrix/Synapse, Jitsi, SearXNG, Nextcloud |
| **Media Enthusiast** | Jellyfin, Sonarr, Radarr, Prowlarr, Tautulli |
| **Small Business Owner** | Odoo, Nextcloud, SuiteCRM, Akaunting, BookStack |
| **Educator/Researcher** | Moodle, Dataverse, JupyterHub, Calibre-Web, LimeSurvey |
| **Student** | GitLab, Code-server, Hugo, Vikunja, Logseq |
| **Career Shifter** | k3s, Jenkins, Portfolio (Hugo), PeerTube, Discourse |

---

## Implementation Priority Guide

### For Beginners (First 90 Days)

1. **Week 1-2**: Docker + Portainer (service management)
2. **Week 3-4**: Pi-hole/AdGuard Home (network-level ad blocking)
3. **Week 5-6**: Nextcloud (file storage and sync)
4. **Week 7-8**: Vaultwarden (password management)
5. **Week 9-10**: Jellyfin (media streaming)
6. **Week 11-12**: Home Assistant (automation) or GitLab (development)

### For Intermediate Users (6-12 Months)

1. **Kubernetes Cluster**: k3s with 3 nodes
2. **CI/CD Pipeline**: ArgoCD + GitHub Actions
3. **Monitoring Stack**: Prometheus + Grafana + Loki
4. **Backup System**: Restic with offsite replication
5. **Database Cluster**: PostgreSQL with replication

### For Advanced Users (12+ Months)

1. **High-Availability**: Keepalived + HAProxy
2. **Service Mesh**: Istio or Linkerd
3. **Data Platform**: Spark + Airflow + MinIO
4. **ML Platform**: Ollama + MLflow + Qdrant
5. **Disaster Recovery**: Automated failover testing

---

> **Note**: This matrix is based on the awesome-selfhosted project data covering 100+ categories and 1000+ projects. Tools are selected based on community adoption, active maintenance, and relevance to professional skill development.
