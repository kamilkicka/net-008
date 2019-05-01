Kolejnymi zdublowanymi urządzeniami w naszej sieci są **Switche L3** (pracujące
tak jak routery na trzeciej warstwie modelu OSI) - Switche warstwy trzeciej w
procesie wymiany danych wykorzystują zarówno adresy MAC (warstwa L2) jak i adres
IP tych urządzeń (warstwa L3). Dlaczego zatem w naszej sieci chcemy wykorzystać
rozwiązania wykorzystujące 3 warstwę? Ponieważ nadmiarowa sieć oparta jedynie o
przełączniki w warstwy 2 operujące na adresach MAC szybko może napotkać na
ograniczenia wynikające z ilości urządzeń pracujących w sieć i wysyłanych przez
nie danych rozgłoszeniowych czyli tzw. broadcastów.

Dzieląc sieć dzięki przełącznikom L3 na kilka różnych podsieci o różnych
adresacjach IP możemy ograniczyć ruch broadcastowy tylko do tej jednej podsieci
w której znajduje się nadawca. Dzięki takiemu rozwiązaniu zostanie ograniczony
wpływ urządzeń  
z jednej podsieci na drugą co w konsekwencji spowoduje ograniczenie widoczności
adresów MAC jedynie do podsieci w której pracuje urządzenie.

**Switch L2** (warstwy drugiej modelu OSI) jest urządzeniem sieciowym łączącym
segmenty sieci, jego zadaniem jest takie kierowanie przesyłaniem ramek
ethernetowych aby mogły trafić do docelowego hosta. W celu komunikacji
wykorzystuje się adresy fizyczne (MAC) urządzeń w tej sieci. Adresy te są
przechowywane w tablicy ARP. Są to dwuczęściowe 48-bitowe wartości dwójkowe
przedstawione w formie 12 cyfr szesnastkowych. Format zapisu adresu MAC może być
różny jednak wszystkie występujące formy są poprawne i dozwolone. np:
dc:9f:db:8c:bd:a9 lub dc9f.db8c.bda9. Połączenia pomiędzy switchami l2 a l3 są
zdublowane, został na nich zaimplementowany protokół STP oraz LACP.

Dodatkowo switche L2 posiadają łącza trunk do switch-ów L3, pozostałe porty
zostały skonfigurowane i przydzielone do vlanu przypisanego do konkretnego
działu

i jednocześnie numeru portu na patchpanelu w serwerowni, porty pracują w trybie
access dzięki czemu w łatwy sposób możemy przydzielać dostęp do wybranego zasobu
sieci osobie z odpowiednimi kompetencjami bądź uprawnieniami, jednak czasem mogą
pojawiać się pętle. W tym przypadku **STP (Spanning Tree Protocol)** zapobiega
powstawaniu niechcianych pętli sieciowych, które mogłyby spowodować wspomniane
niżej burze rozgłoszeniowe. **Pętla sieciowa** może być skutkiem niewłaściwej
nadmiarowości na warstwie pierwszej i drugiej modelu OSI. Jest to spowodowane
tym, że ramka Ethernet nie posiada parametru TTL (Time to Live) –czyli parametru
który zmniejsza wartość pola o 1 za każdym razem natrafiając na router, blokując
dzięki temu dostarczenie pakietu do kolejnego urządzenia sieciowego. Brak
mechanizmu blokującego propagacje tych ramek sprawia, że mogą one krążyć bez
końca aż do fizycznego przerwania połączenia, co wiąże się najczęściej z
fizycznym restartem switcha.

Celem STP jest zapewnienie tylko jednej właściwej logicznej ścieżki poprzez
zablokowanie zdublowanych w niewłaściwy sposób połączeń. Dzięki protokołowi w
przypadku awarii odblokowanej ścieżki, trasa przeliczana jest na nowo i zapasowe
połączenie zostaje aktywowane.

>   Kwestie, które należy wziąć pod uwagę przy implementacji sieci nadmiarowej
>   (dot. L1 oraz L2):

-   **Niestabilność bazy adresów MAC**: adresy w tablicy MAC urządzenia L2 mogą
    zostać zdublowane na różnych portach. Co jest przyczyną pochłaniania zasobów
    urządzeń powodujących zakłócenia w przekazywaniu danych.

-   **Burze rozgłoszeniowe** inaczej burza broadcastowa (broadcast storm) to
    anomalia w wyniku której dochodzi do bezustannego krążenia ramek
    rozgłoszeniowych. Anomalia ta powoduje znaczne obciążenie ruchu sieciowego
    oraz zasobów urządzeń. Do takiej sytuacji nie dochodzi

>   w przypadku urządzeń warstwy trzeciej.

-   **Wielokrotna transmisja ramki**: Wiele kopii ramek unicast może być
    dostarczonych do stacji docelowych. Wiele protokołów oczekuje, że otrzyma
    tylko jedną kopię każdej transmisji. Wiele kopii tej samej ramki może
    spowodować nieodwracalne błędy.

![](media/ce8afb2f7a6b587829b58df458abe11e.png)

Sytuacja korzystna dla powstawania pętli sieciowych

**STP** czyli **Spanning Tree Protocol** określa się również oznaczeniem 802.1D.
Protokół tworzy na własne potrzeby schemat sieci budując drzewo połączeń
pomiędzy switchami co pozwala na komunikację pomiędzy nimi maksymalnie 1
ścieżką. W momencie wykrycia braku dostępności danej ścieżki przełącza na
alternatywną trasę. Aby protokół mógł działać wymagany jest root bridge (switch)
który będzie zarządzał trasami. Aby go wyłonić switche komunikują się za
pośrednictwem ramki **BPDU (Bridge Protocol Data Units)** które zawierają bridge
ID wraz z priorytetem oraz adres MAC. Na podstawie wartości najniższego bitowego
BID zostaje wybrany root switch. Co istotne ramki BPDU są przepuszczane i
interpretowane przez port nawet w przypadku blokady ruchu dla tego portu,
chociażby by po to, by móc na nowo przeliczyć najkorzystniejszą trasę dla danego
połączenia.

Po ustaleniu hierachii urządzeń wyliczana jest najkrótsza możliwa ścieżka. Jest
to robione za pomocą algorytmu STA(spanning tree algorhitm). Algorytm ten
wykorzystuje koszty portów oraz koszty ścieżek. Aby wyznaczyć koszt ścieżki
brany jest pod uwagę koszt każdego portu i prędkość zaczynając od miejsca
początkowego aż do docelowego. Brane są pod uwagę wszystkie przełączniki na tej
trasie.

**Algorytm wybiera trasę, w której koszt ścieżki jest najmniejszy.**

Protokół STP konfiguruje konkretne role portów po przeliczeniu najlepszych
ścieżek:

\-Porty główne są portami najbliższymi root brigde’a. Każdy przełącznik posiada
swój port główny.

\-Porty desygnowane. Są to porty, które nie są główne. Znajdują się one na
łączach typu trunk. Przełącznik główny jest wyjątkiem, ponieważ nie posiada
żadnego portu głównego.

\-Porty alternatywne są portami blokowanymi w celu zabezpieczenia przed
powstaniem pętli.

![](media/f56d57aea68cf22b2b9820b6ca8121a7.jpg)

**Rapid Spanning Tree Protocol (RSTP)** inaczej 802.1w jest rozwiniętą wersją
STP 802.1D. Większość parametrów została w zasadzie niezmieniona co ułatwia
nowym użytkownikom znającym protokół STP nauczenie wdrożenie konfiguracji
protokołu RSTP. Tutaj nie istnieją żadne porty zablokowane, nie przewiduje się
szukania portu w stanie zablokowanym. Występujące to:

-   odrzucania (discarding)

-   uczenia się (learning)

-   przekazywania (forwarding)

Stany blocking i listening postanowiono zgrupować w jeden stan discarding co
znacznie przyspieszyło proces przełączania pakietów, ponieważ port w stanie
discarding może słuchać ramek ale nie może już wysyłać ani odbierać żadnych
danych.

Stany portów są ściśle związane z rolami jakie odgrywają w protokole STP oraz
RSTP, w poniższej tabeli przedstawiliśmy funkcję i rolę dla obu protokołów:

| **Rola portu**  | **Protokół STP/RSTP** | **Funkcja**                                            |
|-----------------|-----------------------|--------------------------------------------------------|
| Root Port       | STP/RSTP              | najlepszy link w kierunku root switcha                 |
| Alternate Port  | RSTP                  | zastępuje link w przypadku awarii linku głównego.      |
| Designated Port | STP/RSTP              | Jako jedyny odpowiada                                  |
|                 |                       | za przesyłanie i odbieranie ramek                      |
| Backup Port     | RSTP                  | zastąpi port desygnowany w przypadku awarii linku      |
| Disabled        | STP/RSTP              | administracyjnie położony port                         |

Kolejną odmiana STP jest Multiple Spanning Tree Protocol (MSTP) Ta odmiana
protokołu drzewa opinającego pozwala przypisać konkretne ścieżki w sieci dla
konkretnych sieci VLAN. MSTP ma możliwość obsłużenia kilku instancji STP
pracujących w tej samej sieci, co jest bardzo wygodne gdy w sieci pracuje kilka
lub kilkanaście vlanów a my nadal mamy możliwość pełnej kontroli nad ruchem.

**LACP oraz Etherchannel**

Etherchannel jest funkcją dla urządzeń CISCO, bazującą na standardzie 802.3ad
przekształconym później na standard 802.1ax opisujący agregację łączy. Pozwala
ona grupować połączenia fizyczne w jedno połączenie logiczne. Dzięki temu
prędkości połączeń fizycznych dla każdego połączenia są sumowane. Dodatkowo w
przypadku awarii z któregoś łącza nie ma przerwania w działaniu sieci. W
implementacji Etherchannel, wszystkie porty danego urządzenia mają ten sam adres
IP oraz MAC.

LACP ( Link Aggregation Control Protocol) jest to protokół otwarty należący do
IEEE. Protokół ten pozwala na dynamiczne zestawienie Etherchannel pomiędzy
urządzeniami. W LACP wyróżniane są dwa rodzaje połączeń. Active oraz Passive.
Połączenie Active wysyła specjalną ramkę, po rozpoznaniu przez łącze Passive
dochodzi do zestawienia Etherchannel.

Na uwadzę należy mieć fakt iż na utworzenie kanału wykorzystaliśmy po 2 porty
1Gb/s z każdej strony i ich łączna prędkość łącza wynosi 2 Gb/s to pojedyncza
sesja jest prowadzona tylko z wykorzystaniem tylko jednego łącza a więc jej
maksymalny transfer to ok. 1Gb/s

Grupę agregacji łączy czyli LAG (Link Aggregatnion Group) w przypadku urządzeń
CISCO możemy stworzyć jednym ze sposobów:

-   Statyczny Etherchannel - zwany równiez jako “mode on” polega na ręcznej
    konfiguracji grupy.

-   Dynamiczny Etherchannel - wykorzystuje jeden z dwóch dostępnych protokołów
    aby automatycznie utworzyć grupę LAG. Protokoły te to: LACP i PAgP który
    nalezy do CISCO

W pierwszym przypadku wymuszamy utworzenie kanału EtherChannel bez negocjacji co
oznacza że nie wykorzystujemy żadnego protokołu i żadne pakiety nie są pomiędzy
przełącznikami wymieniane, wybranie tej metody spowoduje utworzenie kanału nawet
w przypadku kiedy po drugiej stronie przełącznik nie jest skonfigurowany
prawidłowo.

Stosując metodę dynamiczną jak zostało wspomniane wcześniej protokoły LACP lub
PAgP dbają o prawidłową konfigurację tworzonego łącza

W naszym przypadku wykorzystaliśmy protokół LACP z racji na możliwość
wykorzystania go nie tylko na urządzeniach CISCO. Ostatnim etapem jest zadbanie
o to aby interfejsy fizyczne tworzące grupę miały konfigurację zgodną co do
szybkości oraz duplexu.

![](media/98a210d38c5f37324a6af88e0532f409.png)

**Inter-VLAN Routing oraz Router on Stick.**

W celu zapewnienia dostępności danych pomiędzy sieciami VLAN należy
skonfigurować routery tak, aby przełączały ruch pomiędzy tymi sieciami.

Jednym ze sposobów na trasowanie takiego ruchu jest wykorzystanie metody
**Router on Stick** (router na patyku). W tym celu na każdym interfejsie tworzy
się subinsterfejsy o adresacji poprawnej dla każdego segmentu VLAN.

![](media/39b9c129f1f1910d586769e6158c4fe8.jpg)

**4. Wdrożenie sieci oraz testowanie poprawności konfiguracji.**

Projekt w warunkach laboratoryjnych zostanie stworzony przy pomocy emulatora
EVE-NG. Konfiguracja urządzeń zostanie zapisana na nośniku zewnętrznym, aby
można było ją bez problemu wdrożyć na urządzeniach fizycznych. W celu
sprawdzenia poprawności wykonania projektu należy sprawdzić następujące kwestie:

>   \-Dostępność urządzeń w danej sieci VLAN pomiędzy sobą oraz między innymi
>   sieciami VLAN. Posłuży temu komenda PING.

>   \-Prawidłową konfigurację STP, LACP oraz HSRP poprzez fizyczne odłączenie
>   urządzenia od sieci lub odłączenie połączenia. Ma to zasymulować awarię.

>   W przypadku prawidłowej konfiguracji stacja końcowa nie zarejestruje
>   przerwania w dostępie do sieci LAN oraz WAN

**5. Podsumowanie**

Powyższy projekt spełnia zalecenia CISCO w konfiguracji prawidłowej sieci LAN
oraz połączeń redudatnych. Gwarantuje prawidłowe działanie LAN oraz obsługę
awarii. Umożliwia serwisowanie urządzeń oraz rozbudowę sieci bez konieczności
przerywania działania sieci oraz urządzeń końcowych( komputery, drukarki,
telefony VoIP)

Mimo złożoności projektu nie uwzględniono wielu kwestii, które powinny lub mogą
być zaimplementowane, tj:

>   \-Protokół DHCP w celu przydzielania adresacji w sieciach wirtualnych (VLAN)
>   W tym celu można wykorzystać dodatkowy router lub wykorzystać któryś z
>   istniejących w projekcie. Serwer DHCP może zostać również skonfigurowany na
>   dowolnym serwerze Windows lub Linux.

>   \-Protokół routowania dynamicznego w celu połączenia innej sieci LAN poprzez
>   sieć WAN.

W powyższym projekcie nie zawarto osobnej sieci LAN znajdującej się w innej
lokalizacji. W przypadku istnienia takiej sieci w celu komunikacji należy
rozbudować sieć poprzez bezpośrednie połączenie tych lokalizacji lub poprzez
wykorzystanie sieci Internet. Wymaga to zastosowania odpowiednich protokołów
zależnych od wymagań oraz sytuacji przykładowo:

-   W przypadku kilku różnych sieci można zastosować protokół OSPF.

-   W celu zwiększenia bezpieczeństwa sieci należy również stworzyć osobny VLAN
    dla administratorów oraz inżynierów, który nie będzie widoczny dla innych
    podsieci. Tylko ta sieć powinna mieć możliwość dostania się do urządzeń oraz
    ich konfiguracji lub sprawdzenia logów. Połączenie powinno działać poprzez
    SSH

-   Na urządzeniach sieciowych oraz serwerach warto skonfigurować protokół SMNP  
    w celu monitorowania sieci. W tym celu można wykorzystać oprogramowanie
    Nagios lub Zabbix.

-   Zwiększenie bezpieczeństwa poprzez stworzenie serwera Radius, który
    autoryzował by użytkowników domenowych w celu udostępnienia tylko wybranym
    użytkownikom sieć LAN.

-   Zastosowanie port-security, tak aby konkretne porty mogły przyjmować tylko
    jeden adres mac. Pozwoli to zabezpieczyć sieć przed atakami MAC-Flooding

Bibliografia:

\-materiały wykładowe obejmujące przedmiot CCNA3 na uczelni WSEI Kraków
autorstwa mgr. inż. Piotra Pudełko

\-materiały dydaktyczne akademii CISCO przygotowujące to CCNAv6. Semestr 3,
moduł 3 i moduł 4

\-<https://www.cisco.com/c/en/us/td/docs/solutions/Enterprise/Campus/HA_campus_DG/hacampusdg.html>

\-<https://pl.wikipedia.org/wiki/Bridge_Protocol_Data_Unit>

\-<https://www.orbit-computer-solutions.com/understanding-etherchannel-configuration-on-cisco-switches/>

\-<https://en.wikipedia.org/wiki/Link_aggregation>
