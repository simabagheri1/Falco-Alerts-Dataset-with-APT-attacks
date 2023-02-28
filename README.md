

## FALCO

Falco is the open source standard for real-time detection of threats and anomalies across containers, Kubernetes, and cloud services.

### Dataset

We build a relatively large Falco alert dataset for Kubernetes with both normal and APT attacks data to facilitate the learning of predictive models used for attack prediction and to support future research. For the attack alerts, we apply [CALDERA, an adversary emulation platform] (https://caldera.mitre.org), developed by MITRE to mimic the attacks in a Kubernetes cluster in the form of MITRE ATT&CK tactic sequences. For the normal alerts, we take advantage of the fact that Falco generates normal daily routine alerts even in the absence of any attack. We then label these alerts as “attack” or “normal”, respectively. Our dataset contains 231K alerts including 2,314 attack alerts and 228,686 normal alerts.

For our research we sufficed to extract the MITRE ATT&CK tactics out of the Falco alerts, and perform ML-based prediction based on the tactic. Note that Falco only has MITRE ATT&CK tactic tag inside its alerts, therefore, for expanding the predictive model to perorm based on MITRE ATT&CK techniques, NLP methods can be taken to extract techniques out of the alert text (future work).

### Challenges

As Falco reports alerts together for all resources in a cluster, we need to aggregate those alerts to reconstruct the attack steps. For this purpose, we first
automatically group the alerts by resources (i.e., container ID) and then extract MITRE ATT&CK tactics’ property from the sequence of alerts. Additionally, the dataset is unbalanced with the number of normal alerts significantly higher than the attack alerts, as Falco generates a considerable number of similar alerts for system events. To obtain a realistic balanced dataset, we undersample the normal and oversample the attack alerts as attack alerts are less than 1 percent of the data.

### Our Testbed
For collecting runtime security alerts, we deploy Falco in a Kubernetes cluster using the official Helm deployment. Our Kubernetes cluster is hosted over 11 VMs
running Lubuntu 20.04. One VM as master node (eight vCPUs and 32GB RAM), and ten other VMs as worker nodes (each four vCPUs and 8GB RAM). The physical hardware of our cloud is composed of one physical rack-mount server with 2x Intel(R) Xeon(R) Gold 5120 CPU @ 2.20GHz and 128GB of DDR4-2933 running Debian 10.

## Dataset Structure
### Attack Falco Alert Profile
Sample Falco alerts collected from three simulated attacks using CALDERA:
* [Strategic Web Compromise] (https://www.fireeye.com/blog/threat-research/2015/07/second_adobe_flashz0.html)
* [APT 29] (https://attack.mitre.org/groups/G0016)
* [APT 3] (https://attack.mitre.org/groups/G0022)

### Collected Alerts
The **Raw Alerts** folder include the alerts we collected from eleven Pods inside Node xxxxx in our testbed while injecting eight attacks in the normal operation of the cluster.
The Final Processed and Labeled Alerts folder is a procesed representation of the Raw Alerts and they are Labeled. Attack labels is done through identifying attack alerts seperately.
**Final Balanced Labeled Falco Alerts file** is the balanced file. We undersample the normal alerts and oversample the attack alerts.

### Falco Alerts of Simulated Attacks
Eight simulated attacks were shuffled and replayed for eleven Pods in Node xxxxx in our testbed. This file contains the collected Falco alerts of the replayed attacks. 

### MITRE Tactic Sequence
Formatting: {Cointainer ID : MITRE ATT&CK Tactic}
From the Collected Alerts in each Pod (including normal and attack alerts), the MITRE ATT&CK tactic tag is extracted
