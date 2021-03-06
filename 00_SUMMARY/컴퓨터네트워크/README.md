# ๐จ ์ปดํจํฐ ๋คํธ์ํฌ

### โ๏ธ OSI 7๊ณ์ธต


| ๊ณ์ธต | ์ด๋ฆ | ๋จ์ | ์ฃผ์ | ์์ |
|-----|-----|-----|-----|-----|
| 1 | **๋ฌผ๋ฆฌ ๊ณ์ธต (Physical Layer)**        | ๋นํธ(bit) |  |
| 2 | **๋ฐ์ดํฐ ๋งํฌ ๊ณ์ธต (Data Link Layer)**  | ํ๋ ์(frame) | Physical | `HDLC`, `PPP`, `LLC` |
| 3 | **๋คํธ์ํฌ ๊ณ์ธต (Network Layer)**      | ํจํท(packet) | Logical | `X25`, `IP` |
| 4 | **์ ์ก ๊ณ์ธต (Transport Layer)**       | ์ธ๊ทธ๋จผํธ(segment) | Port | `HDLC`, `PPP`, `LLC` |
| 5 | ์ธ์ ๊ณ์ธต (Session Layer)             | ๋ฉ์์ง(message) | Application-Specific | |
| 6 | ํํ ๊ณ์ธต (Presentation Layer) | | |
| 7 | ์์ฉ ๊ณ์ธต (Application Layer) | | |


### โ๏ธ TCP/IP Protocol Suite

1. **Network Interface (Data Link)**: LAN, WAN๊ณผ ๊ด๋ จ๋ ๊ธฐ์ ; `Ethernet`, `HDLC` ๋ฑ
2. **Internet (Network)**: ์ฃผ์ ์ง์ , ๊ฒฝ๋ก ์ค์  ๋ฑ; `IP`, `ARP` ๋ฑ
3. **Transport**: ์ ์ก์ ์ํ ํ๋กํ ์ฝ; `TCP`, `UDP` ๋ฑ
4. **Application**: ์์ฉ ํ๋ก๊ทธ๋จ์์ ์ฌ์ฉ; `HTTP`, `DNS`, `FTP` ๋ฑ



## 1๏ธโฃ ๋ฌผ๋ฆฌ ๊ณ์ธต (Physical Layer)

### โ๏ธ ๋ค์คํ(Multiplexing)

- ์ฃผํ์ ๋ถํ  ๋ค์คํ(FDM)
    - ์ฃผํ์๋ฅผ ํน์  ๊ธธ์ด๋งํผ ์ชผ๊ฐ์ด ๋๋  ์ฌ์ฉํ๋ ๋ฐฉ์
    - ์ฃผํ์ ์ฌ์ด์๋ ๊ฐ์ญ ๋ฐฉ์ง๋ฅผ ์ํ ๊ฐ๋ ๋ฐด๋(guard band)๊ฐ ํ์
- ์๋ถํ  ๋ค์คํ(TDM)
- ์ฝ๋ ๋ถํ  ๋ค์คํ(CDM)
- ํ์ฅ๋ถํ  ๋ค์คํ(WDM)


## 2๏ธโฃ ๋ฐ์ดํฐ ๋งํฌ ๊ณ์ธต (Data Link Layer)

### โ๏ธ Framing

- ๋ฌผ๋ฆฌ ๋ฐ์ดํฐ๋ฅผ frame ๋จ์๋ก ๋๊ธฐ ์ํด์๋ first bit๊ณผ last bit์ ๊ฒฐ์ ์ด ํ์ํจ
- `SYN`๊ณผ `ETX` flag๋ก ํ๋ ์์ ์์๊ณผ ๋์ ์๋ฆผ
    - โ ๏ธ Body๋ถ๋ถ์ ETX์ ๊ฐ์ ๋ฐ์ดํธ๊ฐ ๋ํ๋๋ฉด ๋์ค์ ํ๋ ์์ด ๋๋ ๊ฒ์ผ๋ก ์ค์ธํ  ์ ์๋ค.
        - โ Byte-oriented: Escape character๋ฅผ ETX byte ์์ ๋ถ์ฌ์ ์ ์ก
        - โ Bit-oriented: **Bit Stuffing**์ ์ด์ฉํ์ฌ ํด๊ฒฐ (1์ด ์ฐ์๋์ด 5๊ฐ ๋ํ๋๋ฉด 0ํ๋ ๋ถ์ฌ์ ์ ์กํ๊ณ , ๋ฐ๋๋ก receiver ์ชฝ์์๋ 1์ด 5๊ฐ ์ฐ์๋์ด ๋ํ๋๋ฉด ๋ค์ ๋นํธ ๋ฌด์)

    <img src="./images/ethernet-frame.png" alt="Ethernet Frame" width="800">


### โ๏ธ Error Detection

- **VRC** (Vertical Redundancy Check)
    - 7๋นํธ + 1 [ํจ๋ฆฌํฐ ๋นํธ](https://ko.wikipedia.org/wiki/ํจ๋ฆฌํฐ_๋นํธ)๋ก ๊ตฌ์ฑ (ex. ASCII)
    - ex) 1110111**0** 1110111**0** 1110111**0** 1110110**1**
- **LRC** (Longitudinal Redundancy Check)
    - VRC์ ๋ง์ง๋ง์ row parities๋ก ๊ตฌ์ฑ๋ ๋ฐ์ดํธ ์ถ๊ฐ (BBC; block control character)
    - ex) 1100111**1** 1011101**1** 0111001**0** 0101001**1 01010101**
- **CRC** (Cyclic Redundancy Check)
    - ๋ฐ์ดํฐ๋ฅผ ์ด๋ค divisor๋ก ๋๋ ๋ค์ ๊ทธ ๋๋จธ์ง๋ฅผ data ๋ค์ ๋ถ์ฌ์ ์ ์ก
    - Receiver์ธก์์๋ data + CRC๋ฅผ ๊ฐ์ divisor๋ก ๋๋์ด ๋๋จธ์ง๊ฐ 0์ธ์ง ํ์ธ
    - CRC๋ algebraic polynomial๋ก ํํ๋จ
    - CRC-16์ divisor๊ฐ 17-bit ์ด๋ฉฐ HDLC์ ์ฌ์ฉ
    - CRC-32์ divisor๊ฐ 33-bit ์ด๋ฉฐ LAN์ ์ฌ์ฉ

    <img src="./images/crc.png" alt="CRC" width="700">

- **Checksum**
    - Sender๊ฐ 1์ ๋ณด์๋ฅผ ์ทจํด ์ ์กํ๋ฉด Receiver ์ธก์์๋ checksum์ ํฌํจํ ๋ชจ๋  ๋นํธ๊ฐ 0์ด ๋๋์ง ํ์ธ


### โ๏ธ Error Correction (ARQ)

- ARQ: Automatic Repeat Request (์๋ ๋ฐ๋ณต ์์ฒญ)
- Stop-and-Wait ARQ
    - ํ ๋ฒ์ frame์ ํ๋์ฉ ๋ณด๋ด๋ ๋ฐฉ์ (Sliding Window ์กด์ฌ X)
    - ๐ก **Piggybacking**: sender์ receiver๊ฐ ์๋ก ๋ฐ์ดํฐ๋ฅผ ์ฃผ๊ณ ๋ฐ๋ ๊ฒฝ์ฐ๋ผ๋ฉด data frame์ ACK ์ ๋ณด๋ฅผ ํจ๊ป ์ค์ด ๋ณด๋ด๋ method

    <img src="./images/stop-and-wait-arq.png" alt="Stop & Wait ARQ" width="800">

- **Go-Back-N ARQ**
    - ์ด๋ค ํ๋ ์ ํ๋๊ฐ ์์๋๊ฑฐ๋ ๋์ฐฉํ์ง ์์ ๊ฒฝ์ฐ ACK๋ฅผ ๋ฐ์ ๋ง์ง๋ง frame๋ถํฐ ๋ชจ๋  ํ๋ ์์ ์ฌ์ ์ก

    <img src="./images/go-back-n-arq.png" alt="Go-back-N ARQ" width="800">

- **Selective Repeat ARQ**
    - ์์๋๊ฑฐ๋ ์ฌ๋ผ์ง ํ๋ ์๋ง ์ฌ์ ์ก

    <img src="./images/selective-repeat-arq.png" alt="Selective-Repeat ARQ" width="800">



### โ๏ธ Media Access Control

- ์ฌ๋ฌ ์ ์ ๊ฐ ๋์์ ๊ฐ์ ์ฑ๋์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ์ ๋ฌธ์ 
    1. Fair among users
    2. High efficiency
    3. Low delay
    4. Fault tolerant
- Random Access Protocols
    - **CSMA** (Carrier Sense Multiple Access)
        - Carrier Sensing: ์ ์ก ์ ์ ๋งค์ฒด๊ฐ ์ฌ์ฉ์ค์ธ์ง ํ์ธ
        - โ ๏ธ ๊ฐ์ง ํ์ด๋ฐ์๋ ๋งค์ฒด๊ฐ ๋น์ด์์ด๋ ๊ทธ ์ดํ ํ๋ ์์ด ๋์ฐฉํด ์ถฉ๋ํ  ์ ์๋ค: ์ ํ์ง์ฐ(propagation delay)์ด ์กด์ฌํ๊ธฐ ๋๋ฌธ
    - **CSMA/CD** (Carrier Sense Multiple Access with **Collision Detection**)
        - ์ ์  Ethernet์์ ์ฌ์ฉ
        - Binary Back-off ์๊ณ ๋ฆฌ์ฆ
            - ์ถฉ๋ ๋ฐ๊ฒฌ์๋ง๋ค jamming signal ์ ์ก ํ K โ K + 1
            - (0 ~ 2<sup>K</sup> - 1์ฌ์ด์ ๋๋คํ ์) * (maximum propagation time or average transmission time) ๋งํผ ๊ธฐ๋ค๋ฆฐ๋ค
    - **CSMA/CA** (Carrier SenseMultiple Access with **Collision Avoidance**)
        - ์ถฉ๋ ๊ฐ์ง๊ฐ ์ด๋ ค์ด ๋ฌด์  ๋คํธ์ํฌ์์ ์ฌ์ฉ
        - ์ฑ๋์ด idle ์ํ๋ผ๊ณ  ์ฌ๊ฒจ์ ธ๋ ์ ์ ๊ธฐ๋ค๋ฆฐ ๋ค ์ ์ก์ ์งํ
- Controlled Access Protocols
    - **Reservation**
        - Frame์ด mini-slot์ผ๋ก ์ชผ๊ฐ์ ธ ์ ์กํ  ๋ฐ์ดํฐ๊ฐ ์์ผ๋ฉด ์์ ์๊ฒ ํ ๋น๋ slot์ ๋ฐ์ดํฐ๋ฅผ ์ค์ด์ ๋ณด๋ด๋ ๋ฐฉ๋ฒ
    - **Polling**
        - Primary ์คํ์ด์์ด ์กด์ฌ. ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ์ค๋น๊ฐ ๋๋ฉด primary์ธก์์ `POLL` signal์ ๋ณด๋
        - Primary ์คํ์ด์์์ ๋ณด๋ผ ๋ฐ์ดํฐ๊ฐ ์์ผ๋ฉด `SEL` signal์ ๋ณด๋: Selecting
    - **Token passing**
        - Ring ํํ๋ก ๋คํธ์ํฌ๋ฅผ ๊ตฌ์ฑํ๊ณ  token์ ๋๋ฆฌ๋ ๋ฐฉ์



## 3๏ธโฃ ๋คํธ์ํฌ ๊ณ์ธต (Network Layer)

### โ๏ธ  Switching

- Circuit Switching
    - ์ ์ฒด ๋ฉ์์ง๊ฐ ์ชผ๊ฐ์ง์ง ์์ ์ฑ๋ก source์์ destination๊น์ง ์ ์ก๋จ
    - ex) ์ ํ์ 
- **Packet Switching**
    - ๋ฉ์์ง๊ฐ packet ๋จ์๋ก ์ชผ๊ฐ์ ธ์ ์ ์ก๋จ
    - Connectionless service
        - Forwarding decision์ destination address ๊ธฐ๋ฐ์ผ๋ก ํจ
        - Routing table์ `destination address - output interface` ํํ๋ก ๋งคํ
    - Connection-oriented service
        - Forwarding decision์ `label`๋ก ํจ
        - Routing table์ `port - label` ํํ๋ก ๋งคํ (incoming / outgoing ๋ฐ๋ก)
        - โ Setup์ teardown ์กด์ฌ, ์ผ์ข์ **virtual circuit**์ ๋ง๋๋ ๊ฐ๋

    <img src="./images/tcp-swtiching-connection-oriented.png" alt="Packet Swtiching / connection-oriented" width="700">

### โ๏ธ IP Address

- Classful addressing
    - Class A - ์์ ๋ฐ์ดํธ: 0, ์ฃผ์: 0~127
    - Class B - ์์ ๋ฐ์ดํธ: 10, ์ฃผ์: 128~191
    - Class C - ์์ ๋ฐ์ดํธ: 110, ์ฃผ์: 192~223
    - Class D - ์์ ๋ฐ์ดํธ: 1110, ์ฃผ์: 224~239 โ Milticating
    - Class E - ์์ ๋ฐ์ดํธ: 1111, ์ฃผ์: 240~255 โ Reserved
- Subnetting
    - IP address AND Subnet Mask โ Network address
    - ex) `141.14.72.24` AND `255.255.192.0` โ `141.14.64.0`
- Classless addressing
    - Variable-length blocks
        - `x.y.z.t/n`
        - ex) 140.120.84.24/20 โ 140.120.80.0/20
        - โ ๏ธ 2์ ๋ฉฑ์๋ก๋ง ์ฌ์ฉ์ด ๊ฐ๋ฅํ๋ค


### โ๏ธ IP

- IP ์์์ ํจํท์ datagram์ด๋ผ ๋ถ๋ฆผ
- ํค๋๋ 20~60๋ฐ์ดํธ์ ํฌ๊ธฐ์
- โ ๏ธ Checksum์ header์ ๋ํด์๋ง ํด์ค (๋ฐ์ดํฐ๋ ์ ์ก ๊ณ์ธต์์ ์ฒ๋ฆฌ)

<img src="./images/ip-datagram-format.png" alt="IP Datagram format" width="500">


### โ๏ธ ARP (Address Resolution Protocol)

- Logical address๋ฅผ ๋ฌผ์ด๋ณด๋ฉด physical address๋ฅผ ์๋ ค์ฃผ๋ ํ๋กํ ์ฝ
- IP ์ฃผ์์ ํจ๊ป ์ฌ์ฉ๋๋ฉฐ, LAN ๋ฑ์์ ์ฌ์ฉ
- ARP request โ broadcast
- ARP reply โ unicast



## 4๏ธโฃ ์ ์ก ๊ณ์ธต (Transport Layer)


### โ๏ธ UDP (User Datagram Protocol)

- ํ๋ก์ธ์ค๊ฐ ํต์ ์ ์ํด ํฌํธ๋ฅผ ์ ๊ณต
- 8๋ฐ์ดํธ ํค๋๋ก ๊ณ ์  (Source **port** number, Destination **port** number, total length, checksum ๊ฐ๊ฐ 2๋ฐ์ดํธ์ฉ)
- TCP์ ๋น๊ตํ์ฌ ์ ์ ๊ธฐ๋ฅ๊ณผ ์ ๋ขฐ์ฑ ๋ณด์ , ํ์ง๋ง ๋น ๋ฅธ ์๋๋ก ๋์ํ์ฌ ๋ฐ์ดํฐ ์ ์ค์๋ ํฐ ์๊ด์ด ์๋ ๊ฒฝ์ฐ์ ์ฃผ๋ก ์ฌ์ฉ



### โ๏ธ TCP (Transmission Control Protocol)

- ์ ๋ขฐ์ฑ์๋ ํต์ ์ ์ํด Flow Control, Error Control, Congestion Control ๋ฑ์ ์ํ

<img src="./images/tcp-segment-format.png" alt="TCP segment format" width="500">

- **Three-way handshaking**
    - `โ  SYN` โ `โก SYN+ACK` โ `โข ACK`์ ์์๋ก ์ปค๋ฅ์ ์๋ฆผ
    - `โ  FIN` โ `โก ACK` โ `โข FIN` โ `โฃ ACK`์ ์์๋ก ์ปค๋ฅ์ ์ข๋ฃ
    - ๋์์ (simultaneous) ์ปค๋ฅ์ ์๋ฆฝ ์ `โ  SYN / SYN` โ `โก SYN+ACK / SYN+ACK` 2๋ฒ ๋ง์ ์๋ฆฝ ๊ฐ๋ฅ

<img src="./images/tcp-connection.png" alt="TCP Connection Flow and Three-way handshaking" width="600">


- **Flow Control**
    - ์๋์ ์์  ๋ฒํผ ์ํฉ์ ๊ณ ๋ คํ์ฌ ๋ณด๋ด๋ ํจํท์ ์์ ์กฐ์ ํ๋ ๊ฒ
    - **Sliding Window**
        - ์๋์ฐ ํฌ๊ธฐ : `min(rwnd, cwnd)` rwnd=Receiver window, cwnd: congestion window

- **Error Control**
    - **ACK ์ ์ก ๊ธฐ์ค**
        1. Piggy back: ๋ณด๋ธ ๋ฐ์ดํฐ๊ฐ ์๋ ๊ฒฝ์ฐ ๋ฐ์ดํฐ์ ACK๋ฅผ ํจ๊ป ์ ์ก
        2. ๋ณด๋ผ ๋ฐ์ดํฐ๊ฐ ์๋ ๊ฒฝ์ฐ 500ms time-outํ์ ACK ์ ์ก
        3. Delayed ACK: ๋ฐ์ดํฐ๊ฐ ๋ง์ด ๋ค์ด์ค๋ฉด timeout์ด ๋์ง ์์๋ ACK ์ ์ก
        4. ๋ฒํผ ์ค๊ฐ์ gap์ด ์๊ธด ๊ฒฝ์ฐ โ ๋ฐ์ ์ ์ผ ๋ง์ง๋ง ๋ฒํธ๋ก ACK ์ ์ก
        5. Filled: Gap์ด ๋ค์ ์ฑ์์ง๋ฉด ACK ์ ์ก
        6. ์ค๋ณต ๋ฐ์ดํฐ๊ฐ ๋ค์ด์ค๋ฉด ACK๋ฅผ ๋ฐ๋ก ์ ์ก

<img src="./images/tcp-error-control.png" alt="TCP Error Control" width="800">

- **Congestion Control**
    - Slow Start: ์ฒ์ฒํ ์์ํด์ exponentiallyํ๊ฒ ์ฆ๊ฐํจ
    - Congestion Avoidance: ์ฒ์ฒํ ์ฆ๊ฐ์ํค๋ค๊ฐ congestion ๋ฐ์์ ์ค์ฌ๋๊ฐ

<img src="./images/tcp-congestion-control.png" alt="TCP congestion control" width="650">

<img src="./images/tcp-congestion-control-example.png" alt="TCP congestion control example" width="650">




## 7๏ธโฃ ์์ฉ ๊ณ์ธต (Application Layer)

### โ๏ธ ๋ํ์ ์ธ ์์ฉ ๊ณ์ธต ํ๋กํ ์ฝ

- DNS: Domain Name Space
- FTP: File Transfer Protocol
    - passive open & active open
    - Control process์ data transfer process์ ๋ถ๋ฆฌ
- HTTP: Hyper Text Transfer Protocol

---

### **์ฐธ๊ณ  ์๋ฃ**

- ํ์๋ํ๊ต ๋ฐ์ดํฐํต์  ์์ ๊ฐ์์๋ฃ, ์ต์ง์ ๊ต์ (ITE3003)
    - ๊ต์ฌ - Data Communications and Networking 5/e, Behrouz A. Forouzan
- ํ์๋ํ๊ต ์ปดํจํฐ๋คํธ์ํฌ ์์ ๊ฐ์์๋ฃ, ์กฐ์ธํ ๊ต์ (ENE4019)
    - ๊ต์ฌ - TCP/IP Protocol Suite 4e, B.A. Forouzan
