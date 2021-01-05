---
title: IoT CLı komutları için Defender ile çalışma
description: Bu makalede, sensörlerde ve şirket içi yönetim konsollarında IoT CLı komutları için Defender açıklanır.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/12/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 48c525004b095c1e0f498f86a5395d0002be26f1
ms.sourcegitcommit: 5ef018fdadd854c8a3c360743245c44d306e470d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845256"
---
# <a name="work-with-defender-for-iot-cli-commands"></a>IoT CLı komutları için Defender ile çalışma

Bu makalede algılayıcılar ve şirket içi yönetim konsolları için CLı komutları açıklanmaktadır. Komutlara Yöneticiler, Six kullanıcıları ve destek kullanıcıları erişilebilir.

Bakım etkinliklerini veya bir uyarı gerektirmeyen bir etkinliği planlarken dışlama kuralları tanımlayın.

## <a name="create-local-alert-exclusion-rules"></a>Yerel uyarı dışlama kuralları oluşturma

CLı 'ya aşağıdaki komutu girerek bir dışlama kuralı oluşturabilirsiniz:

```azurecli-interactive
alerts exclusion-rule-create [-h] -n NAME [-ts TIMES] [-dir DIRECTION]  
[-dev DEVICES] [-a ALERTS]
```

Uyarı dışlama kuralları içinde tanımlayabilmeniz gereken öznitelikler şunlardır:

| Öznitelik | Açıklama |
|--|--|
| [-h] | Komut için yardım bilgilerini yazdırır. |
| -n ad | Oluşturulmakta olan kuralın adı. |
| [-TS zamanları] | Kuralın etkin olduğu zaman aralığı. Bu şöyle belirtilmelidir:<br />`xx:yy-xx:yy`<br />Aralarında virgül kullanarak birden fazla zaman aralığı tanımlayabilirsiniz. Örneğin: `xx:yy-xx:yy, xx:yy-xx:yy`. |
| [-dir yönü] | Kuralın uygulanma yönü. Bu şöyle belirtilmelidir:<br />`both | src | dst` |
| [-geliştirme CIHAZLARı] | Kural tarafından dışlanacak cihazların IP adresi ve adres türü şöyle belirtilir:<br />`ip-x.x.x.x`<br />`mac-xx:xx:xx:xx:xx:xx`<br />`subnet: x.x.x.x/x` |
| [-a uyarıları] | Kuralın dışlanacaktır uyarının adı:<br />`0x00000`<br />`0x000001` |

## <a name="append-local-alert-exclusion-rules"></a>Yerel uyarı dışlama kuralları Ekle

CLı 'ya aşağıdaki komutu girerek, geçerli uyarı dışlama kurallarına yeni kurallar ekleyebilirsiniz:

```azurecli-interactive
alerts exclusion-rule-append [-h] -n NAME [-ts TIMES] [-dir DIRECTION]  
[-dev DEVICES] [-a ALERTS]
```

Burada kullanılan öznitelikler, yerel uyarı dışlama kuralları oluştururken açıklanan özniteliklere benzerdir. Buradaki kullanımda, öznitelikler mevcut kurallara uygulanır.

## <a name="show-local-alert-exclusion-rules"></a>Yerel uyarı dışlama kurallarını göster

Var olan tüm dışlama kurallarını görüntülemek için aşağıdaki komutu girin:

```azurecli-interactive
alerts exclusion-rule-list [-h] -n NAME [-ts TIMES] [-dir DIRECTION]  
[-dev DEVICES] [-a ALERTS]
```

## <a name="delete-local-alert-exclusion-rules"></a>Yerel uyarı dışlama kurallarını Sil

Aşağıdaki komutu girerek, var olan bir uyarı dışlama kuralını silebilirsiniz:

```azurecli-interactive
alerts exclusion-rule-remove [-h] -n NAME [-ts TIMES] [-dir DIRECTION]  
[-dev DEVICES] [-a ALERTS]
```

Uyarı dışlama kurallarıyla aşağıdaki özniteliği kullanabilirsiniz:

| Öznitelik | Açıklama|
| --------- | ---------------------------------- |
| -n ad | Silinecek kuralın adı. |

## <a name="sync-time-from-the-ntp-server"></a>NTP sunucusundan eşitleme süresi

Bir NTP sunucusundan zaman eşitlemesini etkinleştirebilir ve devre dışı bırakabilirsiniz.

### <a name="enable-ntp-sync"></a>NTP eşitlemesini etkinleştir

Aşağıdaki komutun girilmesi, geçerli saatin belirtilen NTP sunucusundan düzenli olarak alınmasını etkinleştirir:

```azurecli-interactive
ntp enable IP
```

Komutu içinde tanımlayabilmeniz gereken öznitelik, NTP sunucusunun IP adresidir.

### <a name="disable-ntp-sync"></a>NTP eşitlemesini devre dışı bırak

Aşağıdaki komutun girilmesi, belirtilen NTP sunucusuyla zaman eşitlemesini devre dışı bırakacak:

```azurecli-interactive
ntp disable IP
```

Komutu içinde tanımlayabilmeniz gereken öznitelik, NTP sunucusunun IP adresidir.

## <a name="configure-the-network"></a>Ağı yapılandırma

Aşağıdaki tabloda IoT için Azure Defender için ağ seçeneklerinizi yapılandırmak üzere kullanılabilecek komutlar açıklanmaktadır:

|Ad|Komut|Açıklama|
|-----------|-------|-----------|
|Ping|`ping IP `| IoT Platformu için Defender dışındaki adreslere ping atar.|
|Blink|`network blink`|Ağ yapılandırma parametrelerinin değiştirilmesini izin vermez.|
|Ağı yeniden yapılandırın |`network edit-settings`| Ağ yapılandırma parametrelerinin değiştirilmesini izin vermez. |
|Ağ ayarlarını göster |`network list`|Ağ bağdaştırıcısı parametrelerini görüntüler. |
|Ağ yapılandırmasını doğrulama |`network validate` |Çıkış ağı ayarlarını gösterir. <br /> <br />Örnek: <br /> <br />Geçerli ağ ayarları: <br /> Arabirim: eth0 <br /> IP: 10.100.100.1 <br />alt ağ: 255.255.255.0 <br />Varsayılan ağ geçidi: 10.100.100.254 <br />DNS: 10.100.100.254 <br />arabirimleri izleme: eth1|
|Sertifikayı içeri aktar |`certificate import FILE` |HTTPS sertifikasını içeri aktarır. Bir. CRT dosyasına yol gösteren tam yolu belirtmeniz gerekir \* . |
|Tarihi göster |`date` |Ana bilgisayarda GMT biçiminde geçerli tarihi döndürür. |

## <a name="filter-network-configurations"></a>Ağ yapılandırmasını filtrele

`network capture-filter`Komut, yöneticilerin çözümlenmesi gerekmeyen ağ trafiğini ortadan kaldırmasına olanak tanır. Bir içerme listesi veya dışlama listesi kullanarak trafiği filtreleyin.

```azurecli-interactive
network capture-filter
```

Komutu girdikten sonra aşağıdaki soruda istenir:

>`Would you like to supply devices and subnet masks you wish to include in the capture filter? [Y/N]:`

`Y`Aşağıdaki sözdizimine göre cihaz, kanal, bağlantı noktası ve alt küme ekleyebileceğiniz bir nano dosya açmak için seçin:

| Öznitelik | Açıklama |
|--|--|
| 1.1.1.1 | Bu cihaz için tüm trafiği içerir. |
| 1.1.1.1, 2.2.2.2 | Bu kanala yönelik tüm trafiği içerir. |
| 1.1.1, 2.2.2 | Bu alt ağ için tüm trafiği içerir. |

Bir satırı bırakarak bağımsız değişkenleri ayırın.

Bir cihaz, kanal veya alt ağ eklediğinizde, algılayıcı, genellikle işlenmediği bağlantı noktaları ve trafik dahil olmak üzere bu bağımsız değişken için geçerli tüm trafiği işler.

Bundan sonra şunlar istenir:

>`Would you like to supply devices and subnet masks you wish to exclude from the capture filter? [Y/N]:`

`Y`Aşağıdaki sözdizimine göre cihaz, kanal, bağlantı noktası ve alt küme ekleyebileceğiniz bir nano dosya açmak için seçin:

| Öznitelik | Açıklama |
|--|--|
| 1.1.1.1 | Bu cihaz için tüm trafiği dışlar. |
| 1.1.1.1, 2.2.2.2 | Bu kanal için tüm trafiği, yani iki cihaz arasındaki tüm trafiği hariç tutar. |
| 1.1.1.1, 2.2.2.2, 443 | Bu kanala yönelik tüm trafiği bağlantı noktasıyla dışlar. |
| 1.1.1 | Bu alt ağ için tüm trafiği dışlar. |
| 1.1.1, 2.2.2 | Alt ağlar arasındaki tüm trafiği dışlar. |

Bir satırı bırakarak bağımsız değişkenleri ayırın.

Bir cihazı, kanalı veya alt ağı dışladığınızda, algılayıcı bu bağımsız değişken için geçerli tüm trafiği dışarıda bırakır.

### <a name="ports"></a>Bağlantı noktaları

UDP ve TCP bağlantı noktalarını tüm trafiğe dahil edin veya hariç tutun.

>`502`: tek bağlantı noktası

>`502,443`: her iki bağlantı noktası

>`Enter tcp ports to include (delimited by comma or Enter to skip):`

>`Enter udp ports to include (delimited by comma or Enter to skip):`

>`Enter tcp ports to exclude (delimited by comma or Enter to skip):`

>`Enter udp ports to exclude (delimited by comma or Enter to skip):`

### <a name="components"></a>Bileşenler

Sizden şunlar istenir:

>`In which component do you wish to apply this capture filter?`

Seçenekleriniz şunlardır:  `all` , `dissector` ,, `collector` `statistics-collector` , `rpc-parser` , veya `smb-parser` .

Çoğu kullanım durumunda, öğesini seçin `all` .

### <a name="custom-base-capture-filter"></a>Özel temel yakalama filtresi

Temel yakalama filtresi, bileşenleri için temeldir. Örneğin, filtre, bileşen için hangi bağlantı noktalarının kullanılabilir olduğunu belirler.

`Y`Aşağıdaki seçeneklerin tümü için seçin. Değişiklikler ayarlandıktan sonra tüm filtreler temele eklenir. Değişiklik yaparsanız, var olan taban çizgisinin üzerine yazılır.

>`Would you like to supply a custom base capture filter for the dissector component? [Y/N]:`

>`Would you like to supply a custom base capture filter for the collector component? [Y/N]:`

>`Would you like to supply a custom base capture filter for the statistics-collector component? [Y/N]:`

>`Would you like to supply a custom base capture filter for the rpc-parser component? [Y/N]:`

>`Would you like to supply a custom base capture filter for the smb-parser component? [Y/N]:`

>`Type Y for "internal" otherwise N for "all-connected" (custom operation mode enabled) [Y/N]:`

1.1.1 gibi bir alt ağı dışlanmasını tercih ederseniz:

- `internal` yalnızca o alt ağı dışarıda bırakır.

- `all-connected` Bu alt ağı ve bu alt ağdan gelen ve giden tüm trafiği hariç bırakır.

Seçmenizi öneririz `internal` .

> [!NOTE]
> Seçenekleriniz, araçtaki tüm filtreler için kullanılır ve oturum bağımlı değildir. Diğer bir deyişle, `internal`   bazı filtreler ve diğerleri için hiçbir zaman seçim yapamazsınız  `all-connected`   .

### <a name="comments"></a>Yorumlar

İçindeki filtreleri görüntüleyebilirsiniz  ```/var/cyberx/properties/cybershark.properties``` :

- **İstatistikler-toplayıcı**:  `bpf_filter property` içinde ```/var/cyberx/properties/net.stats.collector.properties```

- **dissektör**: `override.capture_filter`   içindeki özelliği ```/var/cyberx/properties/cybershark.properties```

- **RPC-Parser**: `override.capture_filter` içindeki özelliği ```/var/cyberx/properties/rpc-parser.properties```

- **SMB-Parser**:  `override.capture_filter`   içindeki özelliği ```/var/cyberx/properties/smb-parser.properties```

- **Collector**: `general.bpf_filter`   içindeki özelliği ```/var/cyberx/properties/collector.properties```

Six kullanıcısı için aşağıdaki kodu girerek varsayılan yapılandırmayı geri yükleyebilirsiniz:

```azurecli-interactive
sudo cyberx-xsense-capture-filter -p all -m all-connected
```

## <a name="define-client-and-server-hosts"></a>İstemci ve sunucu Konakları tanımlama

IoT için Defender, istemci ve sunucu konaklarınızı otomatik olarak algılamadıysanız, istemci ve sunucu konaklarınızı ayarlamak için aşağıdaki komutu girin:

```azurecli-interactive
directions [-h] [--identifier IDENTIFIER] [--port PORT] [--remove] [--add]  
[--tcp] [--udp]
```

Komutuyla aşağıdaki öznitelikleri kullanabilirsiniz `directions` :

| Öznitelik | Açıklama |
|--|--|
| [-h] | Komut için yardım bilgilerini yazdırır. |
| [--tanımlayıcı tanımlayıcı] | Sunucu tanımlayıcısı. |
| [--bağlantı noktası bağlantı noktası] | Sunucu bağlantı noktası. |
| [--Kaldır] | Bir istemciyi veya sunucu konağını listeden kaldırır. |
| [--Ekle] | Listeye bir istemci veya sunucu Konağı ekler. |
| [--TCP] | Bu konakla iletişim kurarken TCP kullanın. |
| [--UDP] | Bu konakla iletişim kurarken UDP kullanın. |

## <a name="system-actions"></a>Sistem eylemleri
Aşağıdaki tabloda, IoT için Defender içinde çeşitli sistem eylemleri gerçekleştirmek üzere kullanılabilecek komutlar açıklanmaktadır:

|Ad|Kod|Açıklama|
|----|----|-----------|
|Konağı yeniden başlatın|`system reboot`|Konak cihazını yeniden başlatır.|
|Konağı kapatma|`system shutdown`|Ana bilgisayarı kapatır.|
|Sistemi yedekleme|`system backup`|Hemen bir yedekleme başlatır (zamanlanmamış bir yedekleme).|
|Sistemi bir yedekten geri yükleme|`system restore`|En son yedeklemeden geri yükler.|
|Yedekleme dosyalarını listeleme|`system backup-list`|Kullanılabilir yedekleme dosyalarını listeler.|
|IoT Platformu Hizmetleri için tüm Defender 'ın durumunu görüntüleme|`system sanity`|IoT Platformu Hizmetleri için tüm Defender 'ın geçerli durumunu listeleyerek sistemin performansını denetler.|
|Yazılım sürümünü göster|`system version`|Sistemde çalışmakta olan yazılımın sürümünü görüntüler.|

## <a name="deploy-ssl-and-tls-certificates-to-appliances"></a>Gereçlere SSL ve TLS sertifikaları dağıtma

SSL ve TLS kurumsal sertifikalarını CLı 'ya aktarmak için aşağıdaki komutu girin:

```azurecli-interactive
cyberx-xsense-certificate-import
```
Aracı kullanmak için, sertifika dosyalarını cihaza yüklemeniz gerekir. Bunu WinSCP veya wget gibi araçlar aracılığıyla yapabilirsiniz. 

Komut aşağıdaki giriş bayraklarını destekler:

| Bayrak | Açıklama |
|--|--|
| -h | Komut satırı yardım söz dizimini gösterir. |
| --CRT | Sertifika dosyasının yolu (. CRT uzantısı). |
| --anahtar | . \* Key dosyası. Anahtar uzunluğu en az 2.048 bit olmalıdır. |
| --Zinciri | Sertifika zinciri dosyasının yolu (isteğe bağlı). |
| --Pass | Sertifikayı şifrelemek için kullanılan parola (isteğe bağlı). |
| --parola-ayarla | Varsayılan değer **yanlış**, **kullanılmıyor** şeklindedir. <br />Önceki sertifikayla sağlanan önceki parolayı kullanmak için **true** olarak ayarlayın (isteğe bağlı). |  |

Aracı kullanırken:

- Sertifika dosyalarının gereç üzerinde okunabilir olduğunu doğrulayın. 

- DNS sunucunuz ve karşılık gelen IP adresi ile gereç etki alanı (sertifikada göründüğü gibi) ile onaylayın. 
    
## <a name="see-also"></a>Ayrıca bkz.

[IoT API algılayıcısı ve Yönetim Konsolu API 'Leri için Defender](references-work-with-defender-for-iot-apis.md)
