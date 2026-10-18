# mmussini — Project Context

> Punto di ingresso per le sessioni sui progetti minori dell'account
> `mmussini` (contesto Home). Eccezione: i repo `IOC_test_*` sono **Work**
> e seguono le regole di `Mead-Informatica`; qui sono solo elencati.
> La narrazione datata va in `SESSION_LOG.md`.

## Stato attuale / prossimi step

1. **k8s_cluster** — backup CNPG fatto (PR #4, 2026-10-18): SeaweedFS su
   optimus (Docker, ZFS `tank/cnpg`, S3 solo su virbr0) + plugin Barman Cloud,
   snapshot ZFS giornalieri. Restore testato. Da verificare: dopo un reboot
   il container riparte (porta legata a virbr0, che nasce con libvirt).

## Repo

Tutti sotto `~/documents/it/software/github/mmussini/`.

| Repo | Contesto | Note |
|---|---|---|
| `k8s_cluster` | Home | Provisioning del cluster k3s (Ansible). Installa **solo** Keycloak/operator/CNPG, **mai** realm/client applicativi (quelli sono `maw_kc_init` in My-Asset-Wise). Gestisce anche SeaweedFS su optimus per i backup CNPG. Vault: serve `optimus_become_password` |
| `telechat` | Home | Listener di chat Telegram per eseguire comandi |
| `leprotti` | Home | App React organigramma (`leprotti-orgchart/`) |
| `home_page`, `curriculum_vitae`, `cli_utilities`, `api_tizer_specks`, `wireguard_configurator`, `templates` | Home | — |
| `IOC_test_NOC`, `IOC_test_NOC_lab`, `IOC_test_SOC`, `IOC_test_SOC_lab` | **Work** | Repo di test del lavoro |
| `mmussini-docs` | Home | Questo repo |

## k8s_cluster — eseguire un ruolo Ansible sul cluster reale

Si lancia direttamente dal repo: questa macchina **è** `optimus` (server
Kali, 192.168.77.66) e il cluster k3s gira su una VM locale (192.168.122.141,
rete `virbr0`). Inventory: `k8s_master` (VM) + `localhost` (optimus, che
vuole `optimus_become_password` dal vault). Il vault password file lo
posiziona l'utente; mai copiarlo né leggerlo. Validato 2026-10-18
(`ansible all -m ping`).

```
cd ~/documents/it/software/github/mmussini/k8s_cluster/k8s-ansible
ansible-playbook playbook.yml --tags <nome-ruolo> --vault-password-file='<path-vault-password-file>'
```
