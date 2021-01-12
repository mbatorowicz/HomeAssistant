# HomeAssistant
## NodeRed, Mikrotik
### Blokada internetu

Blokowanie internetu na urządzeniach w sieci lokalnej za pomocą NodeRed i routera Mikrotik.

1. Zainstaluj w NodeRed: __node-red-contrib-mikrotik__
2. Zaloguj się do Mikrotik za pomocą WinBox
3. kliknij __IP > Firewall__,
4. w __Filter Rules__ kliknij na __+__ aby dodać nowy filtr,
5. w zakładce __General__ w polu __Chain__ wybierz _input_,
6. jeśli chcesz filtrować po IP to w __Src. Address__ wpisz IP blokowanego urządzenia,
7. jeśli chcesz filtrować po MAC (ja tak mam u siebie) to w zakładce __Advanced__ w polu __Src. MAC Address__ wpisz interesujący cię MAC,
8. w zakładce __Action__ w polu __Action__ wybierz _drop_ i kliknij OK aby zatwierdzić filtr.
9. Ponownie kliknij __+__ aby utworzyć kolejny filtr,
10. w zakładce __General__ w polu __Chain__ wybierz _forward_, i resztę wykonaj jak w punktach 6, 7, 8.
  
Zapamiętaj nr filtrów - są w kolumnie #.__
Możesz sprawdzić czy filtrowanie działa klikając na utworzone filtry i __Enable__ (niebieski "ptaszek")
  
11. W NodeRed możesz zaimportować poniższy przepływ i przystosować go do własnych potrzeb.
_**UWAGA**_, we fragmencie _"=numbers=12,13\"_ 12 i 13 zastąp numerami swoich filtrów.
12. Po zaimportowaniu przepływu w nodzie __node-red-contrib-mikrotik__ dodaj w __Device__ swój router.
```
[
    {
        "id": "53a3349.6970ccc",
        "type": "mikrotik",
        "z": "f31031fd.61805",
        "device": "dd10d61.b103c28",
        "name": "",
        "action": "9",
        "command": "",
        "command-type": "str",
        "x": 320,
        "y": 300,
        "wires": [
            [
                "ad815e1c.e674f"
            ]
        ]
    },
    {
        "id": "a6f2c7b3.c504f8",
        "type": "inject",
        "z": "f31031fd.61805",
        "name": "Włącz net",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "[\"/ip/firewall/filter/disable\",\"=numbers=12,13\"]",
        "payloadType": "json",
        "x": 120,
        "y": 280,
        "wires": [
            [
                "53a3349.6970ccc"
            ]
        ]
    },
    {
        "id": "ad815e1c.e674f",
        "type": "debug",
        "z": "f31031fd.61805",
        "name": "",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 490,
        "y": 300,
        "wires": []
    },
    {
        "id": "4d620395.8a518c",
        "type": "inject",
        "z": "f31031fd.61805",
        "name": "Wyłącz net",
        "props": [
            {
                "p": "payload"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "[\"/ip/firewall/filter/enable\",\"=numbers=12,13\"]",
        "payloadType": "json",
        "x": 120,
        "y": 320,
        "wires": [
            [
                "53a3349.6970ccc"
            ]
        ]
    },
    {
        "id": "dd10d61.b103c28",
        "type": "mikrotik-device",
        "host": "192.168.1.1",
        "port": "8728",
        "username": null,
        "password": null
    }
]
```
