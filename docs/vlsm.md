# Direccionamiento VLSM

El escenario pide dimensionar cinco áreas por sede y reservar 25 % de crecimiento. La columna **Hosts previstos** muestra la demanda asignada por el Grupo 3 a cada subred. La demanda no representa un recuento de equipos dibujados. Los prefijos se eligieron de mayor a menor según su capacidad. La WAN necesita dos direcciones utilizables por circuito, una por extremo, y usa `/30`. [Ver el dimensionamiento y las decisiones de diseño](informe.md).

## Subredes LAN por VLAN

| Sede | VLAN | Subred | Broadcast | Hosts previstos | IP utilizables | Router activo | Router de respaldo | IP virtual HSRP |
| --- | ---: | --- | --- | ---: | ---: | --- | --- | --- |
| Dublín | 10 | 3.0.6.128/25 | 3.0.6.255 | 76 | 126 | 3.0.6.129 | 3.0.6.130 | 3.0.6.131 |
| Dublín | 20 | 3.0.5.128/25 | 3.0.5.255 | 106 | 126 | 3.0.5.129 | 3.0.5.130 | 3.0.5.131 |
| Dublín | 30 | 3.0.7.0/25 | 3.0.7.127 | 74 | 126 | 3.0.7.1 | 3.0.7.2 | 3.0.7.3 |
| Dublín | 40 | 3.0.6.0/25 | 3.0.6.127 | 106 | 126 | 3.0.6.1 | 3.0.6.2 | 3.0.6.3 |
| Dublín | 50 | 3.0.5.0/25 | 3.0.5.127 | 106 | 126 | 3.0.5.1 | 3.0.5.2 | 3.0.5.3 |
| Luleå | 10 | 3.0.11.128/26 | 3.0.11.191 | 32 | 62 | 3.0.11.129 | 3.0.11.130 | 3.0.11.131 |
| Luleå | 20 | 3.0.12.192/26 | 3.0.12.255 | 31 | 62 | 3.0.12.193 | 3.0.12.194 | 3.0.12.195 |
| Luleå | 30 | 3.0.13.96/27 | 3.0.13.127 | 30 | 30 | 3.0.13.97 | 3.0.13.98 | 3.0.13.99 |
| Luleå | 40 | 3.0.12.128/26 | 3.0.12.191 | 31 | 62 | 3.0.12.129 | 3.0.12.130 | 3.0.12.131 |
| Luleå | 50 | 3.0.13.0/26 | 3.0.13.63 | 31 | 62 | 3.0.13.1 | 3.0.13.2 | 3.0.13.3 |
| Menlo Park | 10 | 3.0.3.0/24 | 3.0.3.255 | 156 | 254 | 3.0.3.1 | 3.0.3.2 | 3.0.3.3 |
| Menlo Park | 20 | 3.0.1.0/24 | 3.0.1.255 | 156 | 254 | 3.0.1.1 | 3.0.1.2 | 3.0.1.3 |
| Menlo Park | 30 | 3.0.4.0/24 | 3.0.4.255 | 155 | 254 | 3.0.4.1 | 3.0.4.2 | 3.0.4.3 |
| Menlo Park | 40 | 3.0.2.0/24 | 3.0.2.255 | 156 | 254 | 3.0.2.1 | 3.0.2.2 | 3.0.2.3 |
| Menlo Park | 50 | 3.0.0.0/24 | 3.0.0.255 | 157 | 254 | 3.0.0.1 | 3.0.0.2 | 3.0.0.3 |
| Prineville | 10 | 3.0.11.64/26 | 3.0.11.127 | 32 | 62 | 3.0.11.65 | 3.0.11.66 | 3.0.11.67 |
| Prineville | 20 | 3.0.12.64/26 | 3.0.12.127 | 31 | 62 | 3.0.12.65 | 3.0.12.66 | 3.0.12.67 |
| Prineville | 30 | 3.0.13.64/27 | 3.0.13.95 | 30 | 30 | 3.0.13.65 | 3.0.13.66 | 3.0.13.67 |
| Prineville | 40 | 3.0.11.192/26 | 3.0.11.255 | 31 | 62 | 3.0.11.193 | 3.0.11.194 | 3.0.11.195 |
| Prineville | 50 | 3.0.12.0/26 | 3.0.12.63 | 31 | 62 | 3.0.12.1 | 3.0.12.2 | 3.0.12.3 |
| Singapur | 10 | 3.0.9.0/25 | 3.0.9.127 | 68 | 126 | 3.0.9.1 | 3.0.9.2 | 3.0.9.3 |
| Singapur | 20 | 3.0.8.128/25 | 3.0.8.255 | 69 | 126 | 3.0.8.129 | 3.0.8.130 | 3.0.8.131 |
| Singapur | 30 | 3.0.9.128/25 | 3.0.9.255 | 68 | 126 | 3.0.9.129 | 3.0.9.130 | 3.0.9.131 |
| Singapur | 40 | 3.0.8.0/25 | 3.0.8.127 | 69 | 126 | 3.0.8.1 | 3.0.8.2 | 3.0.8.3 |
| Singapur | 50 | 3.0.7.128/25 | 3.0.7.255 | 69 | 126 | 3.0.7.129 | 3.0.7.130 | 3.0.7.131 |
| São Paulo | 10 | 3.0.10.192/26 | 3.0.10.255 | 56 | 62 | 3.0.10.193 | 3.0.10.194 | 3.0.10.195 |
| São Paulo | 20 | 3.0.10.64/26 | 3.0.10.127 | 56 | 62 | 3.0.10.65 | 3.0.10.66 | 3.0.10.67 |
| São Paulo | 30 | 3.0.11.0/26 | 3.0.11.63 | 55 | 62 | 3.0.11.1 | 3.0.11.2 | 3.0.11.3 |
| São Paulo | 40 | 3.0.10.0/26 | 3.0.10.63 | 57 | 62 | 3.0.10.1 | 3.0.10.2 | 3.0.10.3 |
| São Paulo | 50 | 3.0.10.128/26 | 3.0.10.191 | 56 | 62 | 3.0.10.129 | 3.0.10.130 | 3.0.10.131 |

La capacidad indicada excluye red y broadcast. Para estimar las IP disponibles para estaciones hay que descontar, como mínimo, las dos IP de router y la IP virtual. Por eso una `/27` tiene 30 direcciones utilizables y admite hasta 27 estaciones tras esas tres reservas.

## Enlaces WAN

| Subred | Broadcast | Equipo A / interfaz | IP A | Equipo B / interfaz | IP B |
| --- | --- | --- | --- | --- | --- |
| 3.0.13.128/30 | 3.0.13.131 | Router StandBy Menlo Park / Serial0/3/0 | 3.0.13.130 | Router StandBy Luleå / Serial0/3/0 | 3.0.13.129 |
| 3.0.13.132/30 | 3.0.13.135 | Router Activo Luleå / Serial0/0/0 | 3.0.13.133 | Router StandBy Menlo Park / Serial0/0/0 | 3.0.13.134 |
| 3.0.13.136/30 | 3.0.13.139 | Router Activo Menlo Park / Serial0/0/1 | 3.0.13.138 | Router Activo Luleå / Serial0/0/1 | 3.0.13.137 |
| 3.0.13.140/30 | 3.0.13.143 | Router StandBy Luleå / Serial0/2/0 | 3.0.13.141 | Router Activo Menlo Park / Serial0/2/0 | 3.0.13.142 |
| 3.0.13.144/30 | 3.0.13.147 | Router Activo Dublín / Serial0/1/0 | 3.0.13.145 | Router StandBy Menlo Park / Serial0/1/0 | 3.0.13.146 |
| 3.0.13.148/30 | 3.0.13.151 | Router StandBy Menlo Park / Serial0/3/1 | 3.0.13.149 | Router StandBy Dublín / Serial0/3/1 | 3.0.13.150 |
| 3.0.13.152/30 | 3.0.13.155 | Router Activo Menlo Park / Serial0/0/0 | 3.0.13.154 | Router Activo Dublín / Serial0/0/0 | 3.0.13.153 |
| 3.0.13.156/30 | 3.0.13.159 | Router StandBy Dublín / Serial0/1/0 | 3.0.13.157 | Router Activo Menlo Park / Serial0/1/0 | 3.0.13.158 |
| 3.0.13.160/30 | 3.0.13.163 | Router StandBy Prineville / Serial0/2/1 | 3.0.13.161 | Router StandBy Menlo Park / Serial0/2/1 | 3.0.13.162 |
| 3.0.13.164/30 | 3.0.13.167 | Router Activo Prineville / Serial0/3/1 | 3.0.13.165 | Router Activo Menlo Park / Serial0/3/1 | 3.0.13.166 |
| 3.0.13.168/30 | 3.0.13.171 | Router Activo Prineville / Serial0/2/0 | 3.0.13.170 | Router Activo São Paulo / Serial0/2/0 | 3.0.13.169 |
| 3.0.13.172/30 | 3.0.13.175 | Router StandBy Prineville / Serial0/3/0 | 3.0.13.173 | Router StandBy São Paulo / Serial0/3/0 | 3.0.13.174 |
| 3.0.13.176/30 | 3.0.13.179 | Router Activo Menlo Park / Serial0/3/0 | 3.0.13.178 | Router Activo São Paulo / Serial0/3/0 | 3.0.13.177 |
| 3.0.13.180/30 | 3.0.13.183 | Router Activo São Paulo / Serial0/0/1 | 3.0.13.181 | Router StandBy Menlo Park / Serial0/0/1 | 3.0.13.182 |
| 3.0.13.184/30 | 3.0.13.187 | Router StandBy Menlo Park / Serial0/2/0 | 3.0.13.186 | Router StandBy São Paulo / Serial0/2/0 | 3.0.13.185 |
| 3.0.13.188/30 | 3.0.13.191 | Router StandBy São Paulo / Serial0/1/1 | 3.0.13.190 | Router Activo Menlo Park / Serial0/1/1 | 3.0.13.189 |
| 3.0.13.192/30 | 3.0.13.195 | Router StandBy Dublín / Serial0/0/0 | 3.0.13.194 | Router StandBy Luleå / Serial0/0/0 | 3.0.13.193 |
| 3.0.13.196/30 | 3.0.13.199 | Router Activo Dublín / Serial0/1/1 | 3.0.13.198 | Router Activo Luleå / Serial0/1/1 | 3.0.13.197 |
| 3.0.13.200/30 | 3.0.13.203 | Router StandBy Dublín / Serial0/0/1 | 3.0.13.201 | Router StandBy Singapur / Serial0/0/1 | 3.0.13.202 |
| 3.0.13.204/30 | 3.0.13.207 | Router Activo Dublín / Serial0/3/0 | 3.0.13.205 | Router Activo Singapur / Serial0/3/0 | 3.0.13.206 |
| 3.0.13.208/30 | 3.0.13.211 | Router StandBy Prineville / Serial0/3/1 | 3.0.13.210 | Router StandBy Luleå / Serial0/3/1 | 3.0.13.209 |
| 3.0.13.212/30 | 3.0.13.215 | Router Activo Prineville / Serial0/2/1 | 3.0.13.214 | Router Activo Luleå / Serial0/2/1 | 3.0.13.213 |
| 3.0.13.216/30 | 3.0.13.219 | Router StandBy Prineville / Serial0/2/0 | 3.0.13.218 | Router StandBy Singapur / Serial0/2/0 | 3.0.13.217 |
| 3.0.13.220/30 | 3.0.13.223 | Router StandBy São Paulo / Serial0/3/1 | 3.0.13.222 | Router StandBy Singapur / Serial0/3/1 | 3.0.13.221 |
| 3.0.13.224/30 | 3.0.13.227 | Router StandBy Singapur / Serial0/2/1 | 3.0.13.225 | Router Activo Menlo Park / Serial0/2/1 | 3.0.13.226 |
| 3.0.13.228/30 | 3.0.13.231 | Router Activo Singapur / Serial0/1/1 | 3.0.13.229 | Router StandBy Menlo Park / Serial0/1/1 | 3.0.13.230 |
| 3.0.13.232/30 | 3.0.13.235 | Router Activo São Paulo / Serial0/3/1 | 3.0.13.234 | Router Activo Singapur / Serial0/3/1 | 3.0.13.233 |
