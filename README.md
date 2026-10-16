# AdGuard Home no Swarm

O serviço `adguard_adguardhome` fornece DNS e painel administrativo pela rede
`traefik-local`. O único Traefik recebe a LAN em `192.168.10.91`, encaminhando
DNS TCP/UDP 53 ao AdGuard e `http://adguard.localnetwork` ao painel na porta 3000.
Não há portas publicadas no host `.90`.

## Deploy e dados

Na raiz do repositório:

```bash
make deploy-adguard
make deploy-traefik
```

O Makefile cria os diretórios persistentes e as redes necessárias. Configuração
e dados ficam em `adguard/data/conf` e `adguard/data/work`, ignorados pelo Git.
O deploy preserva a configuração existente; não gera nem troca credenciais.

O ambiente atual já está configurado. O usuário inicial é `admin`; a credencial
inicial está em `data/conf/admin-password.txt`. Em instalação nova, complete o
assistente em `http://adguard.localnetwork`, configure a interface web na porta
3000 e o DNS na porta 53, ouvindo nas interfaces do container.

## Configuração pelo painel

- Upstreams: `170.239.224.25` e `170.239.224.30`, em paralelo, timeout 3 segundos.
- Sem servidores fallback adicionais nem listas de bloqueio ativas.
- DNS rewrites de `adguard.localnetwork`, `waha.localnetwork`,
  `stremio.localnetwork` e
  `fred.localnetwork` para `192.168.10.91`.
- DHCP desativado. Os clientes DNS permitidos são a rede overlay `10.0.2.0/24`,
  pois o Traefik encaminha as consultas e aparece como cliente nos logs.

Mantenha a função de reescrita habilitada. Faça backup dos dados antes de uma
atualização. Para editar `AdGuardHome.yaml` manualmente, pare o serviço primeiro;
o aplicativo grava esse arquivo ao salvar alterações no painel.

As instruções do Windows, inclusive a regra NRPT que preserva IPv6, estão no
README da raiz. Nenhum script Python ou PowerShell é necessário.
