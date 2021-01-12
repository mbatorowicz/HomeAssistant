# HomeAssistant

Blokowanie internetu na urządzeniach w sieci lokalnej za pomocą NodeRed i routera Mikrotik.

1. Zainstaluj w NodeRed: node-red-contrib-mikrotik
2. Zaloguj się do Mikrotik za pomocą WinBox
3. kliknij IP > Firewall,
4. w Filter Rules kliknij na + aby dodać nowy filtr,
5. w zakładce General w polu Chain wybierz input,
6. jeśli chcesz filtrować po IP to w Src. Address wpisz IP blokowanego urządzenia,
7. jeśli chcesz filtrować po MAC (ja tak mam u siebie) to w zakładce Advanced Src. MAC Address wpisz interesujący cię MAC,
8. w zakładce Action w polu Action wybierz drop i klikamy OK aby zatwierdzić filtr.
9. Ponownie kliknij + aby utworzyć kolejny filtr,
10. w zakładce General w polu Chain wybierz forward, i resztę wykonaj jak w punktach 6, 7, 8.
  
Zapamiętaj nr filtrów - są w kolumnie #
Możesz sprawdzić czy filtrowanie działa klikając na utworzone filtry i Enable (niebieski "ptaszek")
  
11. W NodeRed możesz zaimportować ten przepływ i przystosować go do własnych potrzeb.
UWAGA, we fragmencie "=numbers=12,13\" 12 i 13 zamień numerami swoich filtrów

[{"id":"53a3349.6970ccc","type":"mikrotik","z":"f31031fd.61805","device":"dd10d61.b103c28","name":"","action":"9","command":"","command-type":"str","x":320,"y":300,"wires":[["ad815e1c.e674f"]]},{"id":"a6f2c7b3.c504f8","type":"inject","z":"f31031fd.61805","name":"Włącz net","props":[{"p":"payload"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"[\"/ip/firewall/filter/disable\",\"=numbers=12,13\"]","payloadType":"json","x":120,"y":280,"wires":[["53a3349.6970ccc"]]},{"id":"ad815e1c.e674f","type":"debug","z":"f31031fd.61805","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":490,"y":300,"wires":[]},{"id":"4d620395.8a518c","type":"inject","z":"f31031fd.61805","name":"Wyłącz net","props":[{"p":"payload"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"[\"/ip/firewall/filter/enable\",\"=numbers=12,13\"]","payloadType":"json","x":120,"y":320,"wires":[["53a3349.6970ccc"]]},{"id":"dd10d61.b103c28","type":"mikrotik-device","host":"192.168.1.1","port":"8728","username":null,"password":null}]
