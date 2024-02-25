# mainUnit
Default user:     `admin`  
Default password: `pwd4muesam`


## Mosquitto Installation (MQTT)
1. `sudo apt-get update`
3. `sudo apt-get upgrade`
4. `sudo apt install mosquitto mosquitto-clients`
5. Folgende Konfiguration mit nano in die Datei einfügen und mit Strg+O speichern und dann mit Strg+X schliessen.

   `sudo nano /etc/mosquitto/mosquitto.conf`
      ```ruby
      pid_file /var/run/mosquitto.pid
      persistence true
      persistence_location /var/lib/mosquitto/
      log_dest file /var/log/mosquitto/mosquitto.log
      include_dir /etc/mosquitto/conf.d
      listener 1883
      listener 9001
      protocol websockets
      allow_anonymous false
      password_file /etc/mosquitto/passwd
      ```
6. Falls Authentication in den letzten 2 Zeilen auskommentiert ist user hinzufügen (falls die pw-Datei schon vorhanden ist ohne -c)  
   User sind: `muesam_mainUnit`, `muesam_armUnit`, `muesam_driveUnit`  
   Passwort bei allen: `pwd4muesam` (kann angepasst werden wenn nötig)
   
   `sudo mosquitto_passwd -c /etc/mosquitto/passwd ein_weiterer_benutzername`
7. Status, Starten, Stoppen des Brokers:
    `sudo systemctl status mosquitto`

## Commands
| Command | Description |
| --- | --- |
| ls | Dateien auflisten |
| cd | Change Directory |

## Client Codebeispiele
   - Client Thema abonnieren: `mosquitto_sub -h <Broker-Adresse> -t <Topic> -u <Benutzername> -P <Passwort>`
     
     Python Beispiel:  
     ```python
     import paho.mqtt.client as mqtt

      def on_connect(client, userdata, flags, rc):
          print("Verbunden mit Broker. Rückgabecode: " + str(rc))
          client.subscribe("dein/topic")
      
      def on_message(client, userdata, msg):
          print("Nachricht empfangen: " + msg.topic + " " + str(msg.payload))
      
      client = mqtt.Client()
      client.on_connect = on_connect
      client.on_message = on_message
      
      client.username_pw_set("<Benutzername>", "<Passwort>")
      client.connect("<Broker-Adresse>", 1883, 60)
      
      client.loop_forever()

     ```

    
