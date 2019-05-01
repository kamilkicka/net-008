PROJEKT

Implementacja nadmiarowej sieci LAN dla firmy

w ramach jednego budynku

Opracowali:

1.  Dariusz Gwadera

2.  Kamil Kicka

3.  **Opis oraz założenia projektu:**

Tematem projektu jest:

>   **zaprojektowanie nadmiarowej sieci LAN w wielokondygnacyjnym budynku.**

Sieć zlokalizowana zostanie w 4 kondygnacyjnym budynku, w każdej z kondygnacji
będą pomieszczenia biurowe z 50 stanowiskami z dostępem do sieci.

**W celu zapewnienia jednolitości, sieć oparta jest na następujących zasadach:**

-   W budynku są 2 przyłącza Internetowe realizowane przez niezależnych
    operatorów.  
    Użytkownicy sieci są logicznie podzieleni na różne działy oraz odseparowani
    od warstwy zarządzania.

-   Na każdym piętrze znajduje się punkt dystrybucyjny (IDF) wyposażony w
    przełączniki L2.

-   Główny punkt dystrybucyjny (MDF) wraz serwerownią znajduje się na ostatnim
    piętrze budynku. Zapasowy MDF znajduje się w piwnicy,

-   W obu MDF znajdują się Switche L3, które terminują VLANy do których są
    przypisane poszczególne stanowiska komputerowe.

-   Podstawowym rodzajem kabla jest ekranowana skrętka FTP kategorii 7, może ona
    przenosić sygnały o częstotliwości 600Mhz – do 10Gb/s . Kablami tymi zostaną
    wykonane wszystkie połączenia poziome wewnątrz budynku.

-   Trasy kablowe zostały tak zaprojektowane aby zminimalizować ryzyko uszkodzeń
    okablowania oraz w razie konieczności umożliwić prace konserwacyjne lub
    przyszłą rozbudowę,

-   Wszystkie punkty abonenckie z każdego piętra zbiegają się w punkcie
    dystrybucyjnym danego piętra.

-   Połączenia pomiędzy piętrami połączone są bezpośrednio z serwerownią główną
    oraz zapasową kablem światłowodowym, 2 niezależnymi trasami

-   Urządzenia sieciowe to produkty firmy Cisco które posiadają odpowiednią
    wersję oprogramowania pozwalającą na zastosowanie wymaganych protokołów
    sieciowych oraz zapewniają odpowiednie bezpieczeństwo sieci komputerowej.

**2. Cel projektu**

Celem pracy jest zaprojektowanie sieci komputerowej, która spełni poniższe
wymagania:

-   Infrastruktura musi zapewniać niezawodny dostęp do sieci komputerowej
    użytkownikom. Biorąc pod uwagę redundancję możemy założyć, że wszelkie
    awarie lub rozbudowa dla końcowego odbiorcy pozostaną niezauważone.

-   Sieć komputerowa jest przewidziana na wiele lat, dodatkowo jest skalowalna

>   i jej rozbudowa nie wymaga zmiany infrastruktury bazowej.

-   Nadmiarowość okablowania oraz urządzeń, poprzez zastosowanie odpowiednich
    protokołów nie powoduje tworzenia się pętli sieciowych.

Projekt pozwala na stworzenie sieci łączącej rozwiązania i technologie
stanowiące gwarancję poprawności działania oferowanych usług, ich integralność
oraz prawidłową współpracę ze sobą.

Graficzny schemat projektowanej sieci:

![](media/bcf5759ffe4e25d98259e2f3528b90b3.png)

**3.Teoria**

**Hierarchiczny model sieci**

Prawidłowe działanie sieci wymaga zastosowania odpowiednich rozwiązań

i protokołów sieciowych, które są zbiorem procedur oraz reguł stosowanych w celu
konkretnej komunikacji oraz działania urządzeń sieciowych. Poniżej znajduje się
lista protokołów zastosowana w projekcie, wraz z krótkim objaśnieniem
problematyki związanej

z zastosowaniem konkretnego rozwiązania.

Projektowana sieć LAN składa się ze zdublowanych urządzeń w 2 i 3 warstwie
modelu ISO/OSI. tj.:

-   Dwóch routerów brzegowych zapewniających bezawaryjny dostęp do sieci WAN
    wykorzystujących protokół HSRP.

-   Dwóch modularnych switchy warstwy 3 wykorzystujących protokół LACP lub PAgP
    w celu agregacji i redundancji połączeń na warstwie 2 modelu ISO/OSI oraz
    zapewnienia równoważenia się ruchu sieciowego, wraz z protokołem drzewa
    opinającego w celu zabezpieczenia sieci przed powstaniem pętli.

-   Zastosowania modularnych switchy warstwy drugiej dla każdego punktu
    dystrybucyjnego, podłączonych do dwóch switchy warstwy trzeciej. Switche te
    będą wykorzystywać protokół STP do zabezpieczenia sieci przed występowaniem
    pętli

>   i burz rozgłoszeniowych. Połączenia pomiędzy switchami L2 będą zdublowane
>   oraz będą wykorzystywać protokół LACP.

W projekcie zaimplementowany jest tzw. “Hierarchiczny model sieci”, który ma
ułatwić poprawne jej zaprojektowanie, a w efekcie właściwe działanie i jak
najmniejszą podatność na awarie. Model ten składa się z 3 warstw.

![](media/59d74bb6faf96e2461c4a2771470b165.jpg)

Pierwszą jest “**warstwa rdzenia”** lub inaczej warstwa szkieletowa, która
odpowiada za odbieranie ruchu z całej sieci dystrybucyjnej, właściwe oraz
szybkie przełączanie pakietów. Dzięki tej warstwie można uzyskać dostęp do
Internetu, w tymprzypadku jedynymi elementami tej sieci są Routery.

Kolejnym projektowanym elementem jest “**warstwa dystrybucji”,** która
pośredniczy w wymianie informacji pomiędzy warstwą sieci a warstwą dostępu. To
tutaj gromadzone są dane otrzymywane z warstwy dostępu przed ich transmisją do
warstwy rdzenia. Warstwa ta kontroluje przepływ danych w sieci stanowiąc
centralny punkt w którym są realizowane reguły określające przepływ tego
ruchu,wyznacza domeny rozgłoszeniowe, czy realizuje routing pomiędzy sieciami
vlan. Switche pracujące w tej warstwie możemy określić urządzeniami
wielowarstwowymi, które łączą funkcję routera i switcha. Jednak są
zaprojektowane z myślą o szybkim przełączaniu pakietów, dzięki czemu uzyskują
większą wydajność niż routery.

Ostatnią opisywaną i wykorzystaną warstwą jest “**warstwa dostępu**” to ona
zapewnia możliwość korzystania z sieci urządzeniom końcowym takim jak komputery,
drukarki, telefony VOIP czy przenośne urządzenia bezprzewodowe.

Dzięki zastosowaniu hierarchicznego modelu sieci uzyskamy sieć skalowalną, a
więc taką którą z łatwością w przyszłości i wraz ze wzrastającymi potrzebami
będzie można rozbudować i modyfikować, sieć bezpieczną i podzieloną na segmenty
która odseparuje ruch i zabezpieczy go przed “niepożądanymi” użytkownikami.

Najważniejszym jednak aspektem skorzystania z tego modelu jest fakt budowy sieci
nadmiarowej, która przez zwielokrotnienie połączeń kablowych, zastosowania
odpowiednich protokołów zwiększy niezawodność sieci oraz wydajność i jakość
połączeń.

**Urządzenia i ich funkcjonalności wykorzystane do budowy sieci nadmiarowej**

W sieci na warstwie rdzenia pracują 2 routery CISCO w 3 warstwie modelu ISO/OSI,
zapewniają one łączność z siecią WAN poprzez 2 niezależne łącza od niezależnych
dostawców. Zadaniem routera jest takie przekazywanie pakietów pomiędzy siecią
lokalną a siecią zdalną aby te nawzajem się rozumiały i dotarły od konkretnego
źródła do konkretnego celu jak najlepszą trasą. Dzieje się tak dzięki
informacjom zgromadzonym w tablicy routingu urządzeń. Kiedy Router odbierze
pakiet sprawdza jego docelowy adres IP po odnalezieniu właściwego wpisu router
enkapsuluje pakiet IP w ramkę łącza danych odpowiednią dla interfejsu
wyjściowego a następnie wysyła go do adresata.

Aby oba routery mogły pracować jednocześnie i aby nie powodowało to zakłóceń w
poprawności działaniu sieci a zwiększyło jej odporność na awarie został
zaimplementowany protokół HSRP (Hot Standby Router Protocol), który jest
zastrzeżony jedynie dla urządzeń Cisco, powstał w 1994 roku i jest zdefiniowany
w RFC 2281. Konfigurując usługę przypisujemy nasze routery do jednej grupy HSRP,
której nadajemy numer od 0 do 255 tworząc tym samym wirtualny router z własnym
wirtualnym adresem IP oraz MAC, dzięki temu oba routery są osiągane pod tym
samym adresem IP, który pełni rolę bramy domyślnej dla wszystkich stacji
roboczych.

Jak wiadomo w tym samym czasie nie mogą pracować 2 urządzenia, jedno z nich
zostaje zdefiniowane jako aktywne i jest wykorzystywane do przesyłania pakietów,
drugie zaś pełni rolę zapasowego i w razie awarii tego pierwszego lub po
spełnieniu określonych warunków przejmuję rolę aktywnego. Rolą urządzenia
zapasowego wykorzystującego protokół HSRP jest monitorowanie statusu grupy HSRP
aby w razie awarii stać się urządzeniem aktywnym. Aby nasz protokół mógł działać
właściwie i aby spełniał swoją rolę wystarczą dwa urządzenia, jednak w
zależności od potrzeb oraz krytyczności usługi może być ich więcej.

Wszystkie routery w tej samej grupie wysyłają między sobą pakiety “hello” dla
wersji 1 HSRP są wysyłane na adres multicast 224.0.0.2, natomiast w wersji 2 na
adres: 224.0.0.102. Wirtualny adres MAC HSRP w wersji 1 0000.0C07.ACXX gdzie xx
to numer grupy w zapisie szesnastkowym, zaś wersja 2 używa wirtualnego adresu
0000.0C9F.FXXX gdzie xxx to numer grupy w zapisie szesnastkowym.

W momencie skonfigurowania HSRP na interfejsie, router przechodzi przez szereg
stanów zanim stanie się aktywny, sprawdzając czy w grupie są inne routery i czy
ma zostać routerem aktywnym czy zapasowym.

![enter image description here](media/912476dcf9ce2cfe922e447c98a6a1d4.png)

Proces wyboru aktywnego routera bazuje na priorytecie, który konfiguruje się dla
każdego z urządzeń w grupie i może mieć on wartość od 0 do 255. Domyślnie
priorytet wynosi 100. Router z najwyższym priorytetem w grupie staje się
aktywnym. Jeśli wszystkie urządzenia w grupie mają ten sam priorytet, wówczas
router z najwyższym adresem IP staje się aktywnym. Router z drugim najwyższym
priorytetem staje się zapasowym a pozostałe jeśli są nie pełnią żadnej funkcji.
Routery uczestniczące w HSRP muszą przejść przez 6 stanów które poniżej
opisaliśmy:

1.  **Initial**- to stan początkowy wskazujący na brak działania HSRP. Stan ten
    występuje w przypadku zmiany konfiguracji albo w przypadku wyłączenia
    interfejsu.

2.  **Learn**- nie został jeszcze ustalony wirtualny adres IP i router nie
    otrzymał wiadomości “hello” od routera aktywnego. Dalej więc czeka na taką
    wiadomość

3.  **Listen**- Router otrzymał już adres IP oraz adres MAC routera wirtualnego
    natomiast nie jest ani routerem aktywnym ani zapasowym. Sytuacja ta
    występuje w sieciach gdzie są więcej niż 2 routery w grupie HSRP.

4.  **Speak** - Router wysyła pakiety “hello” i uczestniczy w wyborze aktywnego
    lub rezerwowego urządzenia.

5.  **Standby** - Jest to stan rezerwowy, w tym przypadku router nasłuchuje
    pakietów “hello” od routera aktywnego i przejmuje jego rolę w przypadku gdy
    takie komunikaty przestaną docierać.

6.  **Active** - Stan aktywny, router przepuszcza ruch do wirtualnego adresu IP,
    wysyła również pakiety “hello” ma adres multicast innych routerów w grupie.

Dodatkowo aby zabezpieczyć komunikację pomiędzy routerami możemy uruchomić
uwierzytelnianie na kilka sposobów; Pierwszym jest ustawienie klucza w postaci
zwykłego ciągu znaków przesłanego wraz z komunikatami HSRP w czytelnej i
otwartej postaci.

Znacznie bezpieczniejszą i skuteczniejszą metodą zabezpieczenia jest
zastosowanie MD5 lub konfiguracja pęku kluczy.

HSRP w przeciwieństwie do GLBP nie posiada mechanizmu load-balancingu jednak
możemy go zaimplementować w taki sposób aby najefektywniej wykorzystać posiadany
przez nas sprzęt. W tym celu konfigurujemy jeden z routerów jako active dla
połowy używanych w naszej sieci Vlanów, odpowiednio manipulując priorytetem. W
takim wypadku drugi router konfigurujemy jako active dla drugiej połowy Vlanów.

Ostatnim etapem konfiguracji routerów jest opcja TRACKING która odpowiada za
monitorowanie stanów interfejsów łącz WAN bo bez uruchomienia tej opcji HSRP
zapewni nam redundancję w obrębie naszej sieci. Jeśli jednak port WAN przestanie
pracować właściwie to sieć będzie pracować niestabilnie, tutaj z pomocą
przychodzi bardzo łatwa w zaimplementowaniu funkcja monitorowania stanu
interfejsu - **track** - polega ona na określeniu wartości o jaką zostanie
zmniejszony priorytet aktywnego routera w przypadku wykrycia zmiany stanu
interfejsu.

Co w sytuacji kiedy port działa właściwie a problem leży gdzieś dalej? Aby
zapobiec takim sytuacjom należy zaimplementować funkcję IP SLA dostępną dla
urządzeń CISCO.