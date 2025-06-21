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


### Video utili

WIP: trascrivere comandi utili recuperati da questi video

https://www.youtube.com/watch?v=qy1_jV1fgJU 
https://www.youtube.com/watch?v=VJtIedYfvSk
https://www.youtube.com/watch?v=oP6Zd5NFGnU

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


### Problema utilizzando DHCP

WIP in corso, da risolvere questa problematica quando si vuole usare il raspberry come DHCP al posto del router:

2025-06-19 22:34:13.790 failed to send UDP request: Network unreachable

## Configurazione PiVPN e Wireguard

Guida di riferimento: https://www.youtube.com/watch?v=GZIk_Dl73xo

TODO: da inserire i comandi recuperati dal video precedente

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

WIP


```bash
sxiv -v
```

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

