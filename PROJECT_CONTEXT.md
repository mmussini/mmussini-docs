# mmussini — Project Context

> Punto di ingresso per le sessioni sui progetti minori dell'account
> `mmussini` (contesto Home). Eccezione: i repo `IOC_test_*` sono **Work**
> e seguono le regole di `Mead-Informatica`; qui sono solo elencati.
> La narrazione datata va in `SESSION_LOG.md`.

## Stato attuale / prossimi step

1. **k8s_cluster** — backup CNPG su S3 (era in `TODOs.sh`, rimosso con la
   PR #3: il contenuto è nella history del repo).

## Repo

Tutti sotto `~/documents/it/software/github/mmussini/`.

| Repo | Contesto | Note |
|---|---|---|
| `k8s_cluster` | Home | Provisioning del cluster k3s (Ansible). Installa **solo** Keycloak/operator/CNPG, **mai** realm/client applicativi (quelli sono `maw_kc_init` in My-Asset-Wise) |
| `telechat` | Home | Listener di chat Telegram per eseguire comandi |
| `leprotti` | Home | App React organigramma (`leprotti-orgchart/`) |
| `home_page`, `curriculum_vitae`, `cli_utilities`, `api_tizer_specks`, `wireguard_configurator`, `templates` | Home | — |
| `IOC_test_NOC`, `IOC_test_NOC_lab`, `IOC_test_SOC`, `IOC_test_SOC_lab` | **Work** | Repo di test del lavoro |
| `mmussini-docs` | Home | Questo repo |

## k8s_cluster — eseguire un ruolo Ansible sul cluster reale

Va lanciato da `optimus` (192.168.77.66), l'unico host con rete verso
192.168.122.x. Il vault password file **non va mai spostato** via
scp/rsync: lo posiziona l'utente su optimus quando serve.
**Comandi scritti per la vecchia macchina WSL, da rivalidare su Kali.**

```
rsync -az --delete --exclude .git \
  ~/documents/it/software/github/mmussini/k8s_cluster/k8s-ansible/ 192.168.77.66:~/k8s_cluster-ansible/
ssh 192.168.77.66 "cd ~/k8s_cluster-ansible && ansible-playbook playbook.yml --tags <nome-ruolo> --vault-password-file='<path-vault-password-file-su-optimus>'"
```
