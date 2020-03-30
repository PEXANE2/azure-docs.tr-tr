---
title: Azure Sentinel'de gelişmiş çok aşamalı saldırı algılama
description: Uyarı yorgunluğunu azaltmak ve gelişmiş çok aşamalı saldırı algılamasına dayanan eyleme geçirilebilir olaylar oluşturmak için Azure Sentinel'deki Fusion teknolojisini kullanın.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 87ca322cbdfdd8a53a3ecefcb120a961ea1bb936
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587932"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Azure Sentinel'de gelişmiş çok aşamalı saldırı algılama


> [!IMPORTANT]
> Azure Sentinel'deki bazı Birleştirme özellikleri şu anda genel önizlemededir.
> Bu özellikler hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.



Azure Sentinel, makine öğrenimine dayalı Fusion teknolojisini kullanarak, öldürme zincirinin çeşitli aşamalarında gözlenen anormal davranışları ve şüpheli etkinlikleri birleştirerek çok aşamalı saldırıları otomatik olarak algılayabilir. Azure Sentinel daha sonra yakalanması çok zor olacak olaylar oluşturur. Bu olaylar iki veya daha fazla uyarı veya etkinlik kapsar. Tasarım olarak, bu olaylar düşük hacimli, yüksek sadakat ve yüksek öneme sahiptir.

Ortamınız için özelleştirilmiş bu algılama, yalnızca yanlış pozitif oranları azaltmanın yanında, sınırlı veya eksik bilgilere sahip saldırıları da algılayabilir.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Gelişmiş çok aşamalı saldırı algılamasını yapılandırma

Bu algılama Varsayılan olarak Azure Sentinel'de etkinleştirilir. Durumu denetlemek veya belki de birden çok uyarıya dayalı olaylar oluşturmak için alternatif bir çözüm kullandığınıziçin devre dışı kalmak için aşağıdaki yönergeleri kullanın:

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın

2. Azure **Sentinel** > **Yapılandırma** > **Analitiğine** Gidin

3. **Etkin kuralları** seçin ve **NAME** sütununda Gelişmiş Çok Aşamalı **Saldırı Algılama'yı** bulun. Bu algılamanın etkin veya devre dışı bırakıldığını doğrulamak için **STATUS** sütununa bakın.

4. Durumu değiştirmek için bu girişi ve **Gelişmiş Çok Aşamalı Saldırı Algılama** bıçağını seçin, **Düzenley'i**seçin.

5. Kural **oluşturma sihirbazı bıçak,** durum değişikliği sizin için otomatik olarak seçilir, bu nedenle **Sonraki seçin: Gözden geçirin**ve sonra **Kaydet**. 

Kural şablonları gelişmiş çok aşamalı saldırı algılama için geçerli değildir.

> [!NOTE]
> Azure Sentinel şu anda makine öğrenim sistemlerini eğitmek için 30 günlük geçmiş verileri kullanmaktadır. Bu veriler, makine öğrenimi ardışık boru hattından geçerken microsoft'un anahtarları kullanılarak her zaman şifrelenir. Ancak, Azure Sentinel çalışma alanınızda CMK'yı etkinleştirdiyseniz, eğitim verileri [Müşteri Yönetilen Anahtarlar (CMK)](customer-managed-keys.md) kullanılarak şifrelenmez. Fusion'ı devre dışı bırakmak için **Azure Sentinel** \> **Configuration** \> **Analytics \> Active kuralları \> Gelişmiş Çok Aşamalı Saldırı Algılama** kurallarına gidin ve **Durum** sütununda Devre Dışı **Bırakma'yı seçin.**

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Fusion Palo Alto Networks ve Microsoft Defender ATP kullanarak

Bu senaryolar, güvenlik analistleri tarafından kullanılan temel günlüklerden ikisini birleştirir: Palo Alto Networks'ten güvenlik duvarı günlükleri ve Microsoft Defender ATP'den son nokta algılama günlükleri. Aşağıda listelenen tüm senaryolarda, harici bir IP adresi içeren bitiş noktasında şüpheli bir etkinlik algılanır ve bunu harici IP adresinden güvenlik duvarına geri gelen anormal trafik takip eder. Azure Sentinel, Palo Alto günlüklerinde [tehdit günlüklerine](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs)odaklanır ve tehditlere izin verildiğinde trafik şüpheli olarak kabul edilir (şüpheli veriler, dosyalar, seller, paketler, taramalar, casus yazılımlar, URL'ler, virüsler, güvenlik açıkları, orman yangını virüsleri, orman yangınları).

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>TOR anonymization hizmetine ağ isteği ve ardından Palo Alto Networks güvenlik duvarı tarafından işaretlenmiş anormal trafik.

Bu senaryoda, Azure Sentinel ilk olarak Microsoft Defender Gelişmiş Tehdit Koruması'nın anormal etkinliklere yol açan bir TOR adonymization hizmetiiçin ağ isteği algıladığını bildiren bir uyarı algılar. Bu işlem {time} anda SID ID {sid} ile {hesap adı} adı altında başlatıldı. Bağlantının giden IP adresi {IndividualIp} idi.
Daha sonra, {TimeGenerated} adresindeki Palo Alto Networks Güvenlik Duvarı tarafından olağandışı bir etkinlik algılandı. Bu, ağınıza kötü amaçlı trafiğin girdiğini gösterir Ağ trafiğinin hedef IP adresi {DestinationIP} olduğunu gösterir.

Bu senaryo şu anda genel önizlemede.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell, Palo Alto Networks güvenlik duvarı tarafından işaretlenmiş anormal trafik takip şüpheli bir ağ bağlantısı yaptı.

Bu senaryoda, Azure Sentinel ilk olarak Microsoft Defender Gelişmiş Tehdit Koruması'nın PowerShell'in Palo Alto Network Güvenlik Duvarı tarafından algılanan anormal bir faaliyete yol açan şüpheli bir ağ bağlantısı yaptığını algıladığını tespit ettiği bir uyarı algılar. Bu işlem{time} anda SID ID {sid} ile {hesap adı} hesabı tarafından başlatıldı. Bağlantının giden IP adresi {IndividualIp} idi. Daha sonra, {TimeGenerated} adresindeki Palo Alto Networks Güvenlik Duvarı tarafından olağandışı bir etkinlik algılandı. Bu, kötü amaçlı trafiğin ağınıza girdiğini gösterir. Ağ trafiği için hedef IP adresi {DestinationIP}'dir.

Bu senaryo şu anda genel önizlemede.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Palo Alto Networks güvenlik duvarı tarafından işaretlenmiş anormal trafik ardından yetkisiz erişim girişimleri geçmişi ile IP giden bağlantı

Bu senaryoda Azure Sentinel, Microsoft Defender Gelişmiş Tehdit Koruması'nın, Palo Alto tarafından anormal bir faaliyetin algılanmasına yol açan yetkisiz erişim girişimleri geçmişi olan bir IP adresine giden bir bağlantı algıladığını bildiren bir uyarı algılar Ağlar Güvenlik Duvarı. Bu işlem{time} anda SID ID {sid} ile {hesap adı} hesabı tarafından başlatıldı. Bağlantının giden IP adresi {IndividualIp} idi. Bundan sonra, Palo Alto Networks Güvenlik Duvarı tarafından {TimeGenerated} adresinde olağandışı etkinlik algılandı. Bu, kötü amaçlı trafiğin ağınıza girdiğini gösterir. Ağ trafiği için hedef IP adresi {DestinationIP}'dir.

Bu senaryo şu anda genel önizlemede.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Kimlik Koruması ve Microsoft Bulut Uygulama Güvenliğini Kullanarak Füzyon

Azure Sentinel, gelişmiş çok aşamalı saldırı algılamasını kullanarak Azure Active Directory Identity Protection ve Microsoft Cloud App Security'deki anormallik olaylarını birleştiren aşağıdaki senaryoları destekler:

- [Atipik bir konuma imkansız seyahat ve ardından anormal Office 365 etkinliği](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Yabancı konum için oturum açma etkinliği ve ardından anormal Office 365 etkinliği](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Enfekte cihazdan giriş etkinliği ve ardından anormal Office 365 etkinliği](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Anonim IP adresinden oturum açma etkinliği ve ardından anormal Office 365 etkinliği](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Sızdırılmış kimlik bilgilerine sahip kullanıcıdan gelen oturum açma etkinliği ve ardından anormal Office 365 etkinliği](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

Azure AD [Kimlik Koruması veri bağlayıcısı](connect-azure-ad-identity-protection.md) ve [Bulut Uygulaması Güvenliği](connect-cloud-app-security.md) bağlayıcıları yapılandırıldı.

Bunu izleyen açıklamalarda, Azure Sentinel bu sayfada temsil edilen verilerinizin gerçek değerini parantez içinde değişken olarak görüntüler. Örneğin, \< *hesap adı* yerine bir hesabın gerçek görüntü adı> ve \< *> sayısı* yerine gerçek sayı.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Atipik bir konuma imkansız seyahat ve ardından anormal Office 365 etkinliği

Azure AD Kimlik Koruması'ndan atipik konum uyarılarına ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarına imkansız seyahati birleştiren yedi olası Azure Sentinel olayı vardır:

- **Office 365 posta kutusu nun sızmasına yol açan atipik konumlara imkansız seyahat**
    
    Bu uyarı, \< *hesap adına* göre oturum açma olayının, kullanıcının gelen kutusunda ayarlanmış şüpheli bir gelen kutusu yönlendirme kuralının ardından, atipik bir konum olan,> bir \< *konuma* imkansız bir seyahatten> bir göstergedir.
    
    Bu, hesabın gizliliğinin ihlal edildiğini ve posta kutusunun kuruluşunuzdaki bilgileri çıkarmak için kullanıldığını gösterebilir. Kullanıcı \< *hesabı adı*>, gelen tüm e-postaları harici \< *adrese* ileten bir gelen kutusu yönlendirme kuralı nı>.

- **Şüpheli bulut uygulaması yönetim etkinliğine yol açan atipik konumlara imkansız seyahat**
    
    Bu uyarı, hesap \< *adına* göre oturum açma olayının, atipik bir \< *konum* olan> yer> imkansız bir seyahatten> bir göstergedir.
    
    Ardından, \< *hesap adı*> \< *tek* bir oturumda> yönetim etkinlikleri nin üzerinde gerçekleştirilir.

- **Toplu dosya silmeişlemine yol açan atipik yerlere imkansız seyahat**
    
    Bu uyarı, \< *atipik* bir konum olan> \< *konuma*> hesap adına göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *nın silinmiş numarası.*

- **Toplu dosya indirmeye giden atipik yerlere imkansız seyahat**
    
    Bu uyarı, hesap \< *adına* göre oturum açma olayının, atipik bir \< *konum* olan> yer> imkansız bir seyahatten> bir göstergedir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> benzersiz dosya *sayısı* üzerinden indirilir.

- **Office 365 kimliğe bürünmeye yol açan atipik konumlara imkansız seyahat**
    
    Bu uyarı, hesap \< *adına* göre oturum açma olayının, atipik bir \< *konum* olan> yer> imkansız bir seyahatten> bir göstergedir. 
    
    Ardından, \< *hesap adı*> tek bir\<oturumda kimliğe bürünme etkinliklerinin alışılmadık bir miktarını *(etkinlik sayısı*>) gerçekleştirilmiştir.

- **Toplu dosya paylaşımına yol açan atipik konumlara imkansız seyahat**
    
    Bu uyarı, hesap \< *adına* göre oturum açma olayının, atipik bir \< *konum* olan> yer> imkansız bir seyahatten> bir göstergedir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *sayısı* üzerinde paylaşılır.

- **Bulut uygulamasında fidye yazılımına yol açan atipik yerlere imkansız seyahat**
    
    Bu uyarı, hesap \< *adına* göre oturum açma olayının, atipik bir \< *konum* olan> yer> imkansız bir seyahatten> bir göstergedir. 
    
    Ardından, \< *hesap adı*>> dosya \< *sayısını* yükledi ve \<toplam> dosya *sayısını* sildi. 
    
    Bu etkinlik deseni, olası bir fidye yazılımı saldırısının göstergesidir.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Yabancı konum için oturum açma etkinliği ve ardından anormal Office 365 etkinliği

Azure AD Kimlik Koruması'ndan gelen yabancı konum uyarıları ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarıları için oturum açma etkinliğini birleştiren yedi olası Azure Sentinel olayı vardır.

- **Exchange Online posta kutusu exfiltration giden yabancı bir konumdan oturum açma olayı**
    
    Bu uyarı, \<> *konumdaki* \<hesap> *adına* göre oturum açma olayının, yabancı bir konum olan> ve ardından kullanıcının gelen kutusunda şüpheli bir gelen kutusu yönlendirme kuralının ayarlandığının bir göstergesidir.
    
    Bu, hesabın gizliliğinin ihlal edildiğini ve posta kutusunun kuruluşunuzdaki bilgileri çıkarmak için kullanıldığını gösterebilir. Kullanıcı \< *hesabı adı*>, gelen tüm e-postaları harici \< *adrese* ileten bir gelen kutusu yönlendirme kuralı nı>. 

- **Şüpheli bulut uygulaması yönetim etkinliğine yol açan yabancı bir konumdan oturum açma etkinliği**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı* tek \<bir oturumda> yönetim etkinlik *sayısı* üzerinden gerçekleştirilen hesap adı>.

- **Toplu dosya silmeişlemine giden yabancı bir konumdan oturum açma olayı**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *nın silinmiş numarası.*

- **Toplu dosya indirmeye giden yabancı bir konumdan oturum açma olayı**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> benzersiz dosya *sayısı* üzerinden indirilir.

- **Office 365 kimliğe bürünmeye giden yabancı bir konumdan oturum açma etkinliği**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir.
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> farklı hesap *sayısı* üzerinden taklit edilir.

- **Toplu dosya paylaşımına yol açan yabancı bir konumdan oturum açma olayı**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *sayısı* üzerinde paylaşılır.

- **Bulut uygulamasında fidye yazılımına yol açan yabancı bir konumdan oturum açma etkinliği**
    
    Bu uyarı,> konumundan, yabancı \<bir \< *konumdan* *> hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*>> dosya \< *sayısını* yükledi ve \<toplam> dosya *sayısını* sildi. 
    
    Bu etkinlik deseni, olası bir fidye yazılımı saldırısının göstergesidir.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Enfekte cihazdan giriş etkinliği ve ardından anormal Office 365 etkinliği

Azure AD Kimlik Koruması'ndan gelen virüslü aygıt uyarılarıve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarının oturum açma etkinliğini birleştiren yedi olası Azure Sentinel olayı vardır:

- **Office 365 posta kutusu ekine giden virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> hesap \< *adına* göre oturum açma olayının bir göstergesidir ve ardından kullanıcının gelen kutusunda şüpheli bir gelen kutusu yönlendirme kuralı ayarlanmıştır.
    
    Bu, hesabın gizliliğinin ihlal edildiğini ve posta kutusunun kuruluşunuzdaki bilgileri çıkarmak için kullanıldığını gösterebilir. Kullanıcı \< *hesabı adı*>, gelen tüm e-postaları harici \< *adrese* ileten bir gelen kutusu yönlendirme kuralı nı>. 

- **Şüpheli bulut uygulaması yönetim etkinliğine yol açan virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir.
    
    Ardından, \< *hesap adı* tek \<bir oturumda> yönetim etkinlik *sayısı* üzerinden gerçekleştirilen hesap adı>.

- **Toplu dosya silmeişlemine yol açan virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *nın silinmiş numarası.*

- **Toplu dosya indirmeye giden virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> benzersiz dosya *sayısı* üzerinden indirilir.

- **Office 365 kimliğe bürünme sine yol açan virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> farklı hesap *sayısı* üzerinden taklit edilir.

- **Toplu dosya paylaşımına yol açan virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *sayısı* üzerinde paylaşılır.

- **Bulut uygulamasında fidye yazılımına yol açan virüslü bir cihazdan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılım bulaşmış olabilecek bir cihazdan> \< *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*>> dosya \< *sayısını* yükledi ve \<toplam> dosya *sayısını* sildi. 
    
    Bu etkinlik deseni, olası bir fidye yazılımı saldırısının göstergesidir.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Anonim IP adresinden oturum açma etkinliği ve ardından anormal Office 365 etkinliği

Azure AD Kimlik Koruması'ndan gelen anonim IP adresi uyarılarından ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarından oturum açma etkinliğini birleştiren yedi olası Azure Sentinel olayı vardır:

- **Office 365 posta kutusu extrtrtrtrasyonuna yol açan anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden* hesap *adına* göre> oturum açma olayının ve ardından kullanıcının gelen kutusuna şüpheli bir gelen kutusu yönlendirme kuralının ayarlandığının bir göstergesidir.
    
    Bu, hesabın gizliliğinin ihlal edildiğini ve posta kutusunun kuruluşunuzdaki bilgileri çıkarmak için kullanıldığını gösterebilir. Kullanıcı \< *hesabı adı*>, gelen tüm e-postaları harici \< *adrese* ileten bir gelen kutusu yönlendirme kuralı nı>. 

- **Şüpheli bulut uygulaması yönetim etkinliğine yol açan anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı* tek \<bir oturumda> yönetim etkinlik *sayısı* üzerinden gerçekleştirilen hesap adı>.

- **Toplu dosya silme işlemine yol açan anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *nın silinmiş numarası.*

- **Toplu dosya indirmeye giden anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> benzersiz dosya *sayısı* üzerinden indirilir.

- **Office 365 kimliğe bürünmeye yol açan anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> farklı hesap *sayısı* üzerinden taklit edilir.

- **Toplu dosya paylaşımına yol açan anonim bir IP adresinden oturum açma olayı**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *sayısı* üzerinde paylaşılır.

- **Bulut uygulamasında fidye yazılımına anonim bir IP adresinden oturum açma etkinliği**
    
    Bu uyarı,> anonim bir proxy \<IP adresi IP \< *adresinden*> *hesap adına* göre oturum açma olayının bir göstergesidir. 
    
    Ardından, \< *hesap adı*>> dosya \< *sayısını* yükledi ve \<toplam> dosya *sayısını* sildi. 
    
    Bu etkinlik deseni, olası bir fidye yazılımı saldırısının göstergesidir.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Sızdırılmış kimlik bilgilerine sahip kullanıcıdan gelen oturum açma etkinliği ve ardından anormal Office 365 etkinliği

Kullanıcıdan gelen oturum açma etkinliğini Azure AD Kimlik Koruması'ndan sızdırılmış kimlik bilgileri uyarıları ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarıyla birleştiren yedi olası Azure Sentinel olayı vardır:

- **Office 365 posta kutusu exfiltrasyonuna yol açan sızdırılmış kimlik bilgilerine sahip kullanıcıdan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının, \<kullanıcının gelen kutusunda şüpheli bir gelen kutusu yönlendirme kuralının ayarlandığının> *hesap adına* göre kullanıldığının ve ardından şüpheli bir gelen kutusu yönlendirme kuralının ayarlandığının göstergesidir. 
    
    Bu, hesabın gizliliğinin ihlal edildiğini ve posta kutusunun kuruluşunuzdaki bilgileri çıkarmak için kullanıldığını gösterebilir. Kullanıcı \< *hesabı adı*>, gelen tüm e-postaları harici \< *adrese* ileten bir gelen kutusu yönlendirme kuralı nı>. 

- **Şüpheli bulut uygulaması yönetim etkinliğine yol açan sızdırılmış kimlik bilgilerine sahip kullanıcıdan oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir.
    
    Ardından, \< *hesap adı* tek \<bir oturumda> yönetim etkinlik *sayısı* üzerinden gerçekleştirilen hesap adı>.

- **Toplu dosya silmeişlemine yol açan sızdırılmış kimlik bilgileriyle Kullanıcı'dan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir.
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *nın silinmiş numarası.*

- **Toplu dosya indirmeye giden sızdırılmış kimlik bilgileriyle Kullanıcıdan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir.
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> benzersiz dosya *sayısı* üzerinden indirilir.

- **Office 365 kimliğe bürünmeye yol açan sızdırılmış kimlik bilgilerine sahip kullanıcıdan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir. 
    
    Ardından, \< *hesap adı*> tek \<bir oturumda> farklı hesap *sayısı* üzerinden taklit edilir.

- **Toplu dosya paylaşımına yol açan sızdırılmış kimlik bilgileriyle Kullanıcı'dan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir.
    
    Ardından, \< *hesap adı*> \<tek bir oturumda> benzersiz dosya *sayısı* üzerinde paylaşılır.

- **Bulut uygulamasında fidye yazılımına sızdırılmış kimlik bilgileriyle Kullanıcıdan oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \< *hesap adına* göre> sızdırılan kimlik bilgilerini kullandığının bir göstergesidir. 
    
    Ardından, \< *hesap adı*>> dosya \< *sayısını* yükledi ve \<toplam> dosya *sayısını* sildi. 
    
    Bu etkinlik deseni, olası bir fidye yazılımı saldırısının göstergesidir.

## <a name="next-steps"></a>Sonraki adımlar

Artık gelişmiş çok aşamalı saldırı algılama hakkında daha fazla bilgi edindiğiniziçin, verilerinizde ve olası tehditlerde görünürlük elde etmeyi öğrenmek için aşağıdaki hızlı başlangıçilginizi çekebilir: [Azure Sentinel ile çalışmaya başlayın.](quickstart-get-visibility.md)

Sizin için oluşturulan olayları araştırmaya hazırsanız, aşağıdaki öğreticiye bakın: [Azure Sentinel ile ilgili olayları araştırın.](tutorial-investigate-cases.md)

