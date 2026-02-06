# IPv4/6/TLS/HTTPS giúp bạn chặn đồ của zalo dễ dàng hơn
> ⚠️ DNS ĐƯỢC ÁP DỤNG TẤT BỘ LỌC MẶC ĐỊNH THEO ORG, KHÔNG BAO GỒM **CHẶN QC**
## ☁️ Cloudflare
|TLS | HTTPS| IPv6 | IPv4 |
|---- | -----| -----| -----|
|1g8sabhe6y.cloudflare-gateway.com | https://1g8sabhe6y.cloudflare-gateway.com/dns-query | 2a06:98c1:54::24:3d1 | 172.64.36.1 172.64.36.2 |

## 🛥️ NextDNS
|TLS | HTTPS| IPv6 | IPv4 |
|---- | -----| -----| -----|
| 638162.dns.nextdns.io <br> 638162.dns1.nextdns.io <br> 638162.dns2.nextdns.io | https://dns.nextdns.io/638162 <br>  https://ultralow.dns.nextdns.io/638162 <br>   https://ultralow2.dns.nextdns.io/638162  <br>  https://anycast.dns.nextdns.io/638162  <br>  https://doh3.dns.nextdns.io/638162  <br>  https://doh3.dns1.nextdns.io/638162  <br>  https://doh3.dns2.nextdns.io/638162| 2a07:a8c0::63:8162 <br> 2a07:a8c1::63:8162 <br> 2a07:a8c0:0000:0000:0000:0000:0063:8162 <br> 2a07:a8c1:0000:0000:0000:0000:0063:8162 | nope |

### ℹThông tin thêm
  + dns/dns1/dns2 là lựa chọn server ở ultralow (là server VN)
  + anycast là server ở Singapore có dung lượng cache cao hơn nhưng có thể ping cao hơn
> ⚠ LƯU Ý: NẾU DNS1/2 HOẶC ANYCAST BỊ SẬP THÌ SẼ SẬP HẲN LUÔN, KHUYẾN KHÍCH DÙNG MẶC ĐỊNH ĐỂ GIỮ MỘT MẠNG ỔN ĐỊNH

> DDNS của NextDNS cho ai muốn nhét vào router : `https://link-ip.nextdns.io/638162/15145197addbe44b`

##  🧠 Sự khác biệt giữa NextDNS và Cloudflare
| Tiêu chí | Cloudflare DNS (1.1.1.1) | NextDNS |
|---------|---------------------------|---------|
| **Kiểu dịch vụ** | Public DNS resolver tốc độ cao, tập trung vào hiệu năng có biến thể 1.1.1.1 for Families để chặn malware/nội dung người lớn| Public DNS resolver có cấu hình theo hồ sơ ; tập trung mạnh vào lọc nội dung, bảo mật, phân tích log và kiểm soát  |
| **Số lượng server / POP & phân bố** | Chạy trên mạng toàn cầu của Cloudflare, với trung tâm dữ liệu ở hơn 330 thành phố, hàng trăm thành phố tại 100+ quốc gia; mọi data center đều có chức năng DNS, dùng anycast để trỏ tới POP gần nhất| Không công bố số lượng server chính thức; tài liệu cho biết “mạng lưới trải rộng nhiều địa điểm trên toàn thế giới, cố gắng có hiện diện tại thành phố chính của hầu hết quốc gia/bang”. Mỗi địa điểm luôn dùng **2 nhà cung cấp khác nhau** (2 DC, 2 đường mạng) để dự phòng (có VN) |
| **Kiểu phân bố & định tuyến** | Anycast toàn cầu: cùng IP nhưng được quảng bá từ nhiều POP, client sẽ được định tuyến tới server gần/ít độ trễ nhất; Cloudflare có kết nối peering với 13.000+ mạng, giúp giảm hop và độ trễ | Kết hợp **Anycast + Ultralow (unicast có DNS steering)**. Đa số địa điểm có thể truy cập qua anycast; **tất cả** địa điểm có thể truy cập qua ultralow (hostname *.dns.nextdns.io), hệ thống sẽ tự điều hướng tới server tối ưu và reroute khi một POP gặp sự cố. Anycast IP 45.90.28.x/45.90.30.x dùng cho “legacy DNS” (UDP/53)[cite:62] |
| **Giao thức / công nghệ DNS** | Resolver recursive hỗ trợ: truy vấn qua UDP/TCP truyền thống; **DNS over HTTPS (DoH)** dùng HTTP/2 và HTTP/3; **DNS over TLS (DoT)**; hỗ trợ **Oblivious DoH (ODoH)** để tách IP client khỏi truy vấn DNS, nâng cao ẩn danh. Có xác thực DNSSEC và không chuyển tiếp ECS (EDNS Client Subnet) để tăng riêng tư | Resolver recursive hỗ trợ **DoH, DoT và DNS over QUIC (DoQ)** cho tất cả cấu hình. Có anycast và ultralow (unicast + DNS steering) như trên |
| **DNSSEC** | 1.1.1.1 xác thực DNSSEC (validator), cả dạng bình thường và 1.1.1.1 for Families đều validate DNSSEC | NextDNS hỗ trợ DNSSEC ở mức resolver |
| **Riêng tư & log** | 1.1.1.1 được xàm l là resolver “nhanh nhất và ưu tiên quyền riêng tư”; Cloudflare nhấn mạnh không bán dữ liệu người dùng (mà nó ăn luôn), log được giữ thời gian ngắn, có kiểm toán độc lập (KPMG) để đảm bảo cam kết quyền riêng tư (bản) | Log chi tiết theo profile: domain truy vấn, thiết bị, hành vi bị chặn/cho phép; người dùng có thể **tắt log hoàn toàn, chọn thời gian lưu trữ, chọn khu vực lưu trữ** (tắt r yên tâm đi). Log có thể tải về để phân tích/copy lưu trữ. Mức kiểm soát log và vị trí dữ liệu thường cao hơn Cloudflare trong các so sánh độc lập |
| **Tốc độ & độ trễ (mức toàn cầu)** | 1.1.1.1 thường được DNSPerf đo là **resolver nhanh nhất toàn cầu**, Cloudflare công bố trung bình khoảng **14 ms** toàn cầu khi so với các resolver công cộng khác. Ngoài ra, vì Cloudflare cũng là authoritative DNS lớn và có cung cấp CDN nhúng vào trang web, nhiều truy vấn tới domain host tại Cloudflare được trả lời nội bộ nên càng nhanh | Hiệu năng phụ thuộc mạnh vào: vị trí của POP NextDNS gần bạn, tuyến mạng ISP, và việc bạn dùng anycast hay ultralow. |
| **Độ ổn định & độ trễ trong thực tế** | Mạng Cloudflare được thiết kế cho SLA doanh nghiệp, có rất nhiều POP và dung lượng chống DDoS lớn, nên độ ổn định và latency thường rất tốt, ít biến động ở hầu hết khu vực | Diễn đàn NextDNS có ghi nhận một số giai đoạn latency tăng hoặc DNS trục trặc (đặc biệt khi dùng anycast thay vì ultralow). Người dùng nâng cao có thể tinh chỉnh chọn DNS ultralow gần nhất để tối ưu độ trễ |

## 🔏 Cách cài
### 1. 🐪 Android
#### Với Android 9 trở lên 
Tùy hãng máy (Pixel, Samsung, Xiaomi…) tên menu có thể khác, nhưng cách chung:

  + Mở Cài đặt (Settings).

  + Vào Mạng & internet (Network & internet) hoặc Kết nối (Connections).

  + Tìm mục DNS riêng tư (Private DNS) hoặc bạn tìm luôn cho nhanh

  + Bấm vào rồi lựa server TLS [Cloudflare](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-cloudflare) hoặc [NextDNS](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-nextdns) rồi dán vào

> ⚠ LƯU Ý: ĐỂ Ý XEM CÓ ĐOẠN `HTTPS://` NẾU CÓ THÌ XÓA ĐI
#### Với Android 9 trở xuống

Dùng [AdGuard](https://github.com/AdguardTeam/AdguardForAndroid/releases) rồi thêm bộ lọc vào 
| Zalo | ZaloPay | Labankey | Zingmp3|
|------|---------|----------|--------|
|[Đây](https://github.com/zalofucker/fuck-you-zalo?tab=readme-ov-file#1-adguard-home--adguard-app) | [Đây](https://github.com/zalofucker/fuck-you-zalopay?tab=readme-ov-file#1-adguard-home--adguard-app) | [Đây](https://github.com/zalofucker/fuck-you-labankey?tab=readme-ov-file#1-adguard-home--adguard-app) | [Đây](https://github.com/zalofucker/fuck-you-zingmp3?tab=readme-ov-file#1-adguard-home--adguard-app) |

### 2. 🍏 Apple (IOS/IpadOS/MacOS/VisionOS/TvOS/...)
Sử dụng Config đã được làm sẵn và thêm dưới dạng Profile vào
#### 🌧 Cloudflare
|TLS | HTTPS |
|:----:|:-------:|
|![QR-cl-tls](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/cl-zalofucker-tls.mobileconfig>) | ![QR-cl-https](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/cl-zalofucker-https.mobileconfig>) |
| [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/cl-zalofucker-tls.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/cl-zalofucker-https.mobileconfig) |

#### 👔 NextDNS
|  | **Mặc định** | **Ultralow1** | **Ultralow2** | **Anycast** |
|:---:|:---:|:---:|:---:|:---:|
| **TLS** | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-tls.mobileconfig>) | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow1-tls.mobileconfig>) | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow2-tls.mobileconfig>) | |
|  | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-tls.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow1-tls.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow2-tls.mobileconfi) | |
| **HTTPS** | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-https.mobileconfig>) | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow1-https.mobileconfig>) | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow2-https.mobileconfig>) | ![Ảnh](https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=<https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-anycast-https.mobileconfig>) |
|  | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-https.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow1-https.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-ultralow2-https.mobileconfig) | [Đây](https://raw.githubusercontent.com/zalofucker/Zalofucker-Dns/refs/heads/main/config/nd-zalofucker-anycast-https.mobileconfig) |

### 3. 🌍 Trình duyệt 
#### 🦊 Firefox based (Zen/Mullvad/Florip/Tor/Flop/....)
  + Mở Firefox.

  + Bấm ☰ (ba que) → Settings (Cài đặt).

  + Trong General (Chung), kéo xuống Network Settings (Thiết lập mạng).

  + Bấm Settings….
    
  + Tích Enable DNS over HTTPS (Bật DNS qua HTTPS).

  +Chọn Use Provider và chọn Custom rồi lựa server HTTPS [Cloudflare](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-cloudflare) hoặc [NextDNS](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-nextdns) rồi dán vào

#### ⛪ Chromium based (ungoogled-chromium/Brave/Cromite/Thorium/...)

  + Mở Chrome.

  + Bấm ⋮ (ba chấm) → Settings (Cài đặt).

  + Vào Privacy and security (Quyền riêng tư và bảo mật).

  + Chọn Security (Bảo mật).
  
  + Kéo xuống Use secure DNS (Sử dụng DNS bảo mật).

  + Bật Use secure DNS.

  Chọn Custom rồi lựa server HTTPS [Cloudflare](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-cloudflare) hoặc [NextDNS](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-nextdns) rồi dán vào

### 4. 📶 Router/Modern/AP
#### dnsmasq

Bước 1: Tạo tệp tin hoặc chỉnh sửa tập tin `dnsmasq.conf`

```
no-resolv
bogus-priv
strict-order
server=
server=
server=
server=
add-cpe-id=
```

  Với `server=` thì lựa server IPv4/6 [Cloudflare](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-cloudflare) hoặc [NextDNS](https://github.com/zalofucker/Zalofucker-Dns?tab=readme-ov-file#%EF%B8%8F-nextdns)
  > ⚠ LƯU Ý: `add-cpe-id=` CHỈ DÙNG KHI BẠN SỬ DỤNG NEXTDNS, CÒN LẠI THÌ BỎ

#### Stubby 

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Tạo tệp tin hoặc chỉnh sửa tập tin `stubby.xml`

```
round_robin_upstreams: 1
upstream_recursive_servers:
  - address_data: 45.90.28.0
    tls_auth_name: "638162.dns.nextdns.io"
  - address_data: 2a07:a8c0::0
    tls_auth_name: "638162.dns.nextdns.io"
  - address_data: 45.90.30.0
    tls_auth_name: "638162.dns.nextdns.io"
  - address_data: 2a07:a8c1::0
    tls_auth_name: "638162.dns.nextdns.io"
```
> ⚠ LƯU Ý: CHỈ HỖ TRỢ STUBBY ĐÃ KẾT NỐI VỚI OPENSSL PHIÊN BẢN >= 1.1.1

#### DNSCrypt

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Tạo tệp tin hoặc chỉnh sửa tập tin `dnscrypt-proxy.toml`

```
server_names = ['NextDNS-638162']

[static]
  [static.'NextDNS-638162']
  stamp = 'sdns://AgEAAAAAAAAAAAAOZG5zLm5leHRkbnMuaW8HLzYzODE2Mg'
```

#### pfSense

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Đến tới Dịch vụ (Services) -> Nhà xử lý DNS ( DNS Resolver) -> Chung (General) -> Tùy chọn khác (Custom Options)

Bước 2: Điền dòng sau
```
server:
  forward-zone:
    name: "."
    forward-tls-upstream: yes
    forward-addr: 45.90.28.0#638162.dns.nextdns.io
    forward-addr: 2a07:a8c0::#638162.dns.nextdns.io
    forward-addr: 45.90.30.0#638162.dns.nextdns.io
    forward-addr: 2a07:a8c1::#638162.dns.nextdns.io
```

#### Knot Resolover

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Tạo tệp tin hoặc chỉnh sửa tệp tin tại `/etc/kresd/custom.conf` 

eg: `nano /etc/kresd/custom.conf`

Bước 2: Điền dòng sau 

```
policy.add(policy.all(policy.TLS_FORWARD({
  {'45.90.28.0', hostname='638162.dns.nextdns.io'},
  {'2a07:a8c0::', hostname='638162.dns.nextdns.io'},
  {'45.90.30.0', hostname='638162.dns.nextdns.io'},
  {'2a07:a8c1::', hostname='638162.dns.nextdns.io'}
})))
```

#### Unbound

> 😡 CỰC LƯU Ý: DO UNBOUND VÀ CNAMES ĐANG ĐẤM NHAU VẬY NÊN LÀ CÓ THỂ CÓ LỖI, XEM TẠI [ĐÂY]( github.com/NLnetLabs/unbound/issues/132)
>> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Tạo tệp tin hoặc chỉnh sửa tệp tin `unbound.conf`

Bước 2: Thêm dòng sau vào

```
forward-zone:
  name: "."
  forward-tls-upstream: yes
  forward-addr: 45.90.28.0#638162.dns.nextdns.io
  forward-addr: 2a07:a8c0::#638162.dns.nextdns.io
  forward-addr: 45.90.30.0#638162.dns.nextdns.io
  forward-addr: 2a07:a8c1::#638162.dns.nextdns.io
```

#### MikroTik

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS

Bước 1: Chạy lệnh sau

```
/tool fetch url=https://curl.se/ca/cacert.pem
/certificate import file-name=cacert.pem
/ip dns set servers=""
/ip dns static add name=dns.nextdns.io address=45.90.28.0 type=A
/ip dns static add name=dns.nextdns.io address=45.90.30.0 type=A
/ip dns static add name=dns.nextdns.io address=2a07:a8c0:: type=AAAA
/ip dns static add name=dns.nextdns.io address=2a07:a8c1:: type=AAAA
/ip dns set use-doh-server=“https://dns.nextdns.io/638162” verify-doh-cert=yes
```

#### tailscale

> ⚠ LƯU Ý: CHỈ HỖ TRỢ NEXTDNS


Bạn hãy đọc qua bài [này nhé](https://tailscale.com/kb/1218/nextdns)

## 🙏 Cảm ơn:
  + mrfvv: Người xây dựng công cụ tạo DNS cho CloudFlare
  + NextDNS : Nhà cung cấp DNS
  + [HzzMoment](https://github.com/hzzmonetvn) : Thg làm QR
  + VOZ : Giúp tôi xây dựng server anycast  
