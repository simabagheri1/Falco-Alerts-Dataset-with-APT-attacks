**FALCO**

Falco is the open source standard for real-time detection of threats and anomalies across containers, Kubernetes, and cloud services.

We build a relatively large Falco alert dataset for Kubernetes with both normal and APT attacks data to facilitate the learning of predictive models used for attack prediction and to support future research. For the attack alerts, we apply CALDERA, an adversary emulation platform, developed by MITRE to mimic the attacks in a Kubernetes cluster in the form of MITRE ATT&CK tactic sequences. For the normal alerts, we take advantage of the fact that Falco generates normal daily routine alerts even in the absence of any attack. We then label these alerts as “attack” or “normal”,
