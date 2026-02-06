# DNS IPv4/6/TLS/HTTPS giúp bạn chặn đồ của zalo dễ dàng hơn
> ⚠️ DNS ĐƯỢC ÁP DỤNG TẤT BỘ LỌC MẶC ĐỊNH THEO ORG, KHÔNG BAO GỒM CHẶN QC
## ☁️ Cloudflare
|TLS | HTTPS| IPv6 | IPv4 |
|---- | -----| -----| -----|
|1g8sabhe6y.cloudflare-gateway.com | https://1g8sabhe6y.cloudflare-gateway.com/dns-query | 2a06:98c1:54::24:3d1 | 172.64.36.1 172.64.36.2 |

## 🛥️ NextDNS
|TLS | HTTPS| IPv6 | IPv4 |
|---- | -----| -----| -----|
| 638162.dns.nextdns.io <br> 638162.dns1.nextdns.io <br> 638162.dns2.nextdns.io | https://dns.nextdns.io/638162 <br>  https://ultralow.dns.nextdns.io/638162 <br>   https://ultralow2.dns.nextdns.io/638162  <br>  https://anycast.dns.nextdns.io/638162  <br>  https://doh3.dns.nextdns.io/638162  <br>  https://doh3.dns1.nextdns.io/638162  <br>  https://doh3.dns2.nextdns.io/638162| 2a07:a8c0::63:8162 2a07:a8c1::63:8162 | 172.64.36.1 172.64.36.2 |

### ℹThông tin thêm
  + dns/dns1/dns2 là lựa chọn server ở ultralow (là server VN)
  + anycast là server ở Singapore có dung lượng cache cao hơn nhưng có thể ping cao hơn

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
