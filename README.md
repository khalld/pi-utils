# pi-utils

Repository contenente tutte le procedure per configurazione di Raspberry Pi Zero 2W.

L'obettivo del progetto è configurare un Raspberry come adblocker, accesso da remoto tramite vpn wireguard per permettere la fruzione di microservizi SaaS. *Utilizzo di ownCloud per la sincronizzazione dei file e rendere accessibili i file da remoto (WIP)*.

Si consiglia di eseguirli in ordine in modo tale da arginare eventuali problematiche.

## Configurazione IP statico

**TIP:** si consiglia per prima cosa di assegnare IP statico sul router.

1. entrare in ssh 
2. eseguire `ip r | grep default`, il secondo ip mostrato dovrebbe essere raspberry mentre il primo dovrebbe essere quello del router
3. Per recuperare il gateway eseguire `sudo nano /etc/resolv.conf`, il primo ip dovrebbe essere quello da configurare successivamente nel gateway

    ![alt text](<imgs/Pasted Graphic 1.png>)

4. Seguire passaggi in: https://www.youtube.com/watch?v=VJtIedYfvSk

WIP: trascrivere i passaggi da eseguire successivamente

### Video utili

WIP: trascrivere comandi utili recuperati da questi video

- https://www.youtube.com/watch?v=qy1_jV1fgJU 
- https://www.youtube.com/watch?v=oP6Zd5NFGnU

## Configure Pihole

Guida di riferimento https://www.raspberrypi.com/tutorials/running-pi-hole-on-a-raspberry-pi/ 

1. Eseguire `nmtui` , edit connection, configurare la connessione ed inserire in ipv4 configuration manual ed impostare i dati recuperati precedentemente

![alt text](<imgs/Pasted Graphic 2.png>)

2. Configurare come DNS ip google o altri a piacere
3. `sudo systemctl restart NetworkManager`

### Tip: disabilitare modulo wlan0 di default

Si consiglia di utilizzare un modulo ethernet esterno in modo tale da fruire di una connessione più stabile. Si consiglia di conseguenza, dopo aver completato la configurazione dell' `eth0` di disabilitare il modulo `wlan0` per evitare che venga caricato di default (si consiglia di associare un IP statico prima della disabilitazione in modo tale da non perdere l'accesso al dispositivo).

Comando per disabilitare il modulo `wlan0`:

```bash
rfkill block wlan
```

Per capire se il modulo è stato disabilitato, eseguire:

```bash
ip addr
```

Il seguente comando riavviera il modulo `wlan0` al successivo reboot. Per disabilitare il modulo di default:

1. Editare il seguente file `/boot/firmware/config.txt`
2. Aggiungere la stringa `dtoverlay=disable-wifi` alla fine del file
3. Riavviare, eseguendo `ip addr` non dovrebbe più essere presente il modulo `wlan0`

### Configurazione DNS 

Consigliato utilizzare il raspberry come DNS del router in modo tale da evitare di dover configurare ogni dispositivo della rete locale e fruire di un adblocker direttamente da ogni dispositivo.

1. Abilitare DHCP su PiHole (anche Ipv6, consigliato per evitare problemi di lentezza di connessione)
2. Disabilitare DHCP del router
3. Configurare DNS del router con l'IP statico del Raspberry Pi

*(NB 21-06-25: In corso test per capire se la configurazione funziona correttamente, in alcuni dispositivi non ho capito perche non agisce correttamente mentre in altri funziona)*

### Problema utilizzando DHCP (NON tramite DNS)

In alcuni router non e' possibile utilizzare il raspberry come DNS, di consguenza si consiglia di utilizzare il raspberry come DHCP e DNS. Tuttavia questo processo non e consigliato inquanto potrebbe causare problemi di connessione (nel mio caso lentezza della banda). Si dovrebbe configurare il router in modo tale che punti come all'IP del raspberry come ed abilitare DHCP su pihole. Una problematica riscontrata e' che l'accesso al router verra' nascosto dal raspberry. Si consiglia di utilizzare il metodo tradizionale per DNS e configurare ogni dispositivo della rete locale puntando l'IP del raspberry come DNS.

Esempio di problema riscontrato utilizzando questo metodo:

```bash
2025-06-19 22:34:13.790 failed to send UDP request: Network unreachable
```

## Configurazione PiVPN e Wireguard

Guida di riferimento: https://www.youtube.com/watch?v=GZIk_Dl73xo

```bash
curl -L https://install.pivpn.io | bash
```

Seguire il procedimento guidato, si consiglia di utilizzare le seguenti configurazioni:

- Abilitare Ipv6
- Mantenere DHCP
- Installare Wireguard
- Use public IP

Comando per generazione qrcode:

```bash
pivpn -qr
```

Documentazione wireguard: https://docs.pi-hole.net/guides/vpn/wireguard/server/

## Port forwarding sul router

Modello di riferimento del router: EG8145X6

Abilitare funzionalità DMZ inserendo come host address l'IP statico del Raspberry:

![alt text](<imgs/Pasted Graphic-1.png>)

Configurazione port forwarding della porta scelta allo step precedente (esempio 51820):

![alt text](<imgs/Pasted Graphic 1-1.png>)

## Configurazione ownCloud

WIP

## Improve security accessing SSH

WIP

## Installazione node

Vista le limitazioni del Raspberry Pi Zero 2W, si consiglia di utilizzare nodejs.

https://github.com/nvm-sh/nvm

## Visualizzatore di immagini da terminale - WIP

WIP - da eliminare se eventualmente non funziona


```bash
sxiv -v
```

## Jellyfin - WIP

```
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```

Reference:
- https://jellyfin.org/docs/general/installation/linux#debian--ubuntu-and-derivatives

## File manager - WIP

installato filemanager 

   sudo apt install pcmanfm

   pcmanfm

You can also directly access shares using URLs like smb://servername/share or smb://xxx.xxx.xxx.xxx/share. 

-- non lo so fare funzionare, in caso disinstalla

Da testare

https://filebrowser.org/installation

se funziona eliminare quello di sopra

## NGIX - WIP

### Testing NGIX with a simple node.js microservice

## Comandi utili

Check per recuperare ip raspberry al primo avvio:

```bash
ping nomepi.local
```

Comando di spegnimento

```bash
sudo shutdown -h now
```

Recuperare IP raspberry (deve essere dentro ssh)

```bash
hostname -I
```

Misura temperatura:

```bash
vcgencmd measure_temp
```

To display it in a more human-readable format (GB, MB, etc.)

```bash
free -h
```

