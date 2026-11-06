# Atividade 8 — EtherChannel com LACP

Repositório da atividade prática de configuração e validação de EtherChannel no Cisco Packet Tracer.

## Objetivo

Implementar agregação de enlaces entre switches utilizando **LACP**, formando canais EtherChannel de camada 2 e configurando-os como trunks para transportar as VLANs 10 e 20.

## Topologia e canais

| Canal | Equipamentos | Interfaces físicas | Protocolo |
|---|---|---|---|
| Port-channel 1 (Po1) | SW-CORE-01 ↔ SW-CORE-02 | Gi5/1, Gi6/1, Gi7/1 e Gi8/1 em cada extremidade | LACP |
| Port-channel 2 (Po2) | SW-CORE-01 ↔ SW-DIST-01 | Gi1/1 e Gi2/1 em cada extremidade | LACP |
| Port-channel 2 (Po2) | SW-CORE-02 ↔ SW-DIST-02 | Gi1/1 e Gi2/1 em cada extremidade | LACP |

> Os números dos grupos EtherChannel podem se repetir em switches diferentes. O que importa é que as extremidades de cada enlace estejam configuradas de forma compatível.

## Configurações esperadas

- EtherChannel de camada 2.
- Negociação LACP.
- Port-channels configurados como trunks 802.1Q.
- VLAN nativa 1.
- VLANs permitidas: 10 e 20.

## Validação realizada

Foram analisadas as saídas dos comandos `show etherchannel summary` e `show interfaces trunk` dos quatro switches.

### Resumo do EtherChannel

| Switch | Grupo | Estado | Protocolo | Membros agregados |
|---|---:|---|---|---|
| SW-CORE-01 | Po1 | SU | LACP | Gi5/1, Gi6/1, Gi7/1, Gi8/1 — todos (P) |
| SW-CORE-01 | Po2 | SU | LACP | Gi1/1, Gi2/1 — ambos (P) |
| SW-CORE-02 | Po1 | SU | LACP | Gi5/1, Gi6/1, Gi7/1, Gi8/1 — todos (P) |
| SW-CORE-02 | Po2 | SU | LACP | Gi1/1, Gi2/1 — ambos (P) |
| SW-DIST-01 | Po2 | SU | LACP | Gi1/1, Gi2/1 — ambos (P) |
| SW-DIST-02 | Po2 | SU | LACP | Gi1/1, Gi2/1 — ambos (P) |

**Interpretação:** `S` indica port-channel de camada 2; `U` indica que está em uso; `P` indica que a interface física está agregada ao EtherChannel. Todos os membros apresentados estão agregados corretamente.

### Resumo das trunks

Nos quatro switches, as interfaces Port-channel apresentadas estão em estado `trunking`, com encapsulamento 802.1Q, VLAN nativa 1 e VLANs 10 e 20 permitidas, ativas e em encaminhamento pelo Spanning Tree.

Nos switches SW-DIST-01 e SW-DIST-02, as interfaces físicas Gi5/1 e Gi6/1 também aparecem como trunks individuais. Elas não aparecem como membros do Po2 nos resultados apresentados; sua função deve ser conferida no diagrama da topologia.

## Comandos de verificação

Execute nos switches:

```cisco
show etherchannel summary
show interfaces trunk
show running-config
```
