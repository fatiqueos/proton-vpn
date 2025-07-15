
# Proton VPN Windows Uygulaması

[Proton VPN](https://protonvpn.com) Windows uygulaması, tüm Proton VPN kullanıcıları için tasarlanmıştır — hem ücretsiz hem ücretli aboneler için.  
Kullanıcı kaydı uygulama üzerinden değil, doğrudan web sitesinden yapılır.

---

## 📥 İndirme

En güncel kararlı sürümü şu adreslerden indirebilirsiniz:

- [Proton VPN Resmi Sitesi](https://protonvpn.com/download)  
- [GitHub Releases](https://github.com/ProtonVPN/win-app/releases/latest)
- [fatiqueos Releases](https://github.com/fatiqueos/proton-vpn/releases)
---

## 🧩 Uygulama Bileşenleri

Proton VPN Windows uygulaması aşağıdaki ana bileşenlerden oluşur:

- **Proton VPN GUI (Arayüz) Uygulaması**  
- **Proton VPN Windows Hizmeti**  
- **OpenVPN**  
- **TAP Adaptörü**  
- **Split Tunnel (Bölünmüş Tünel) Callout Sürücüsü**

### GUI Uygulaması

- Kurulum Dizini: `C:\Program Files\Proton\VPN\<sürüm>`  
- Yürütülebilir Dosya: `ProtonVPN.exe`  
- Log Dosyaları: `%LOCALAPPDATA%\ProtonVPN\Logs`  

Uygulama açıldığında Proton VPN hizmetini başlatır, kapandığında ise durdurur.

> **Not:** Debug modunda TLS sertifika pinning devre dışı bırakılabilir. Bunun için uygulama dizinine aşağıdaki gibi bir `ProtonVPN.config` dosyası eklenmelidir:

```json
{
  "TlsPinningConfig": {}
}
```

---

### Proton VPN Hizmeti

- Kurulum Dizini: `C:\Program Files\Proton\VPN\<sürüm>`  
- Hizmet Adı: `ProtonVPN Service`  
- Yürütülebilir Dosya: `ProtonVPNService.exe`  
- Log Dosyaları: `%ALLUSERSPROFILE%\ProtonVPN\Logs`  

Windows hizmeti VPN bağlantısını yönetir, firewall ve split tunnel işlemlerini gerçekleştirir.  
GUI uygulaması tarafından kontrol edilir.

**Hizmetin manuel kurulumu/kaldırılması için:**

```powershell
ProtonVPNService.exe install    # Hizmeti kurar
ProtonVPNService.exe uninstall  # Hizmeti kaldırır
```

---

### OpenVPN

Proton VPN bağlantısı OpenVPN protokolü kullanılarak sağlanır.

- Kurulum Dizini: `C:\Program Files\Proton\VPN\<sürüm>\Resources`  
- Her bağlantı için yeni OpenVPN süreci başlatılır ve bağlantı kesildiğinde kapatılır.  
- OpenVPN, Proton VPN’e özel TAP adaptörünü destekleyecek şekilde yamalanmıştır.  
- OpenVPN yapılandırma dosyası statiktir.

Kaynak kodu: [ProtonVPN/win-openvpn](https://github.com/ProtonVPN/win-openvpn)

---

### TAP Adaptörü

- Adı: `TAP-ProtonVPN Windows Adapter V9`  
- OpenVPN tarafından kullanılan sanal ağ adaptörüdür.  
- Proton VPN’e özel olarak isimlendirilmiş ve modifiye edilmiştir.  

Kaynak kodu: [ProtonVPN/win-tap-adapter](https://github.com/ProtonVPN/win-tap-adapter)

---

### Split Tunnel ve DNS Leak Koruması (Callout Driver)

- Adı: `ProtonVPN Callout Driver`  
- VPN dışı arayüzlerden gelen DNS taleplerini engeller.  
- Split Tunnel etkinleştirildiğinde trafik yönlendirmelerini yönetir.  
- Kernel mod sürücüsü olarak çalışır ve VPN bağlantısına bağlı olarak başlatılır/durdurulur.

---

## 📂 Proje Klasör Yapısı

```plaintext
ProtonVPN/
├── ci/                        # CI/CD betikleri
├── packages/                  # NuGet paketleri
├── Setup/                     # Kurulum dosyaları ve kaynakları
│   ├── Images/                # Kurulum görselleri
│   ├── Installers/            # Oluşturulmuş yükleyiciler
│   ├── ProtonVPNTap-SetupFiles/ # TAP sürücüsü kurulum dosyaları
│   └── SplitTunnel/           # Callout sürücüsü
├── src/                       # Proje kaynak kodları
│   ├── bin/                   # Derleme çıktıları (silinebilir)
│   └── srp/                   # ProtonMail SRP alt modülü
├── test/                      # Test projeleri
```

---

## 🛠️ Visual Studio Çözüm Projeleri

| Proje Adı                   | Açıklama                                     |
|-----------------------------|----------------------------------------------|
| ProtonVPN.App               | Ana GUI uygulaması (WPF, MVVM)                |
| ProtonVPN.Service           | Windows Hizmeti (VPN ve firewall yönetimi)   |
| ProtonVPN.Core              | İş mantığı (business logic)                    |
| ProtonVPN.Common            | Paylaşılan yardımcı sınıflar                   |
| ProtonVPN.Resource          | Paylaşılan kaynaklar                            |
| ProtonVPN.CalloutDriver     | Kernel modu split tunnel sürücüsü (C++)       |
| ProtonVPN.IpFilter          | Windows firewall filtre kütüphanesi (C++)     |
| ProtonVPN.TapInstaller      | TAP adaptör kurulum modülü                      |
| ProtonVPN.TlsVerify         | VPN sunucu sertifika doğrulama aracı           |
| ProtonVPN.Update            | Güncelleme modülü                              |
| ProtonVPN.UpdateService     | Güncelleme hizmeti                             |
| ProtonVPN.Native            | Windows API sarmalayıcı (C#)                   |
| ProtonVPN.NetworkFilter     | Firewall yapılandırma sarmalayıcı (C#)         |
| ProtonVPN.ErrorMessage      | Uygulama hata mesajları                         |

---

## 📢 Katkıda Bulunma

Katkı sağlamak veya sorun bildirmek için lütfen [GitHub Issues](https://github.com/ProtonVPN/win-app/issues) sayfasını kullanın.

---

© 2023 Proton AG  
Lisans bilgisi için [COPYING.md](COPYING.md) dosyasını inceleyin.
