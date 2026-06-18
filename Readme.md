# Zabbix Template – ASKEY RTL6300 5G CPE

Zabbix monitoring template for the ASKEY RTL6300 5G CPE via its REST API.

## Requirements

- Zabbix 6.0+
- Network access from Zabbix server/proxy to the device REST API (port 80)

## Installation

1. Import `zbx_template_5g_device.xml` via *Configuration → Templates → Import*
2. Assign the template to a host
3. Set the host **IP address** in the host interface — the template uses `{HOST.CONN}` to reach the device

## Collected items

| Item | Source endpoint | Description |
|------|----------------|-------------|
| Technology status | `/lte/status_info` | Current radio technology (LTE / 5G_NSA / 5G_SA) |
| Signal level | `/lte/status_info` | Signal strength 0–5 |
| SIM status | `/lte/status_info` | SIM card ready/not ready |
| Roaming status | `/lte/status_info` | Home network or roaming |
| Network operator (PLMN) | `/lte/status_info` | Operator name |
| LTE RSRP / RSRQ / SINR | `/lte/signal_info` | LTE RF parameters |
| 5G NR RSRP / RSRQ / SINR | `/lte/signal_info` | 5G NR RF parameters |
| 5G NR Band | `/lte/signal_info` | Active 5G NR band number |
| 5G NR PCI | `/lte/signal_info` | Physical Cell ID |

Each endpoint is fetched once per minute as a master HTTP item; all other items are dependents parsed via JSONPath — no redundant HTTP calls.

## Triggers

| Trigger | Severity |
|---------|----------|
| Device not connected via 5G | Warning |
| Signal level < 3 | Warning |
| Signal level ≤ 1 | High |
| SIM card not ready | High |
| LTE RSRP < -110 dBm | High |
| 5G NR RSRP < -120 dBm | High |
| Device in roaming | Info |
| 5G NR PCI changed (cell handover) | Info |
