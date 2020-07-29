---
title: Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama
description: Uyarı fatıg 'yi azaltmak ve gelişmiş çok aşamalı saldırı algılamasına dayalı eylem yapılabilir olaylar oluşturmak için Azure Sentinel 'de Fusion teknolojisini kullanın.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77587932"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama


> [!IMPORTANT]
> Azure Sentinel 'teki bazı Fusion özellikleri şu anda genel önizlemededir.
> Bu özellikler, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Azure Sentinel, Machine Learning 'i temel alan Fusion teknolojisini kullanarak çok aşamalı saldırıları otomatik olarak algılayabilir ve bu da sonlandırma zincirinin çeşitli aşamalarında gözlemlenen anormal davranışları ve şüpheli etkinlikleri birleştirerek. Azure Sentinel, daha sonra yakalamak çok zor olan olaylar oluşturur. Bu olaylar iki veya daha fazla uyarıyı veya etkinliği büyük bir şekilde kaydeder. Tasarım yaparak, bu olaylar düşük hacimdir, yüksek uygunlukta ve yüksek öneme sahiptir.

Ortamınız için özelleştirilmiş, bu algılama yalnızca hatalı pozitif oranları azaltmıyor ancak sınırlı veya eksik bilgilere sahip saldırıları tespit edebilir.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Gelişmiş çok aşamalı saldırı algılamasını yapılandırma

Bu algılama, Azure Sentinel 'de varsayılan olarak etkinleştirilmiştir. Durumu denetlemek veya birden çok uyarıyı temel alan olaylar oluşturmak için alternatif bir çözüm kullandığınız için belki de devre dışı bırakmak için aşağıdaki yönergeleri kullanın:

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın

2. **Azure Sentinel**  >  **yapılandırma**  >  **Analizi** ' ne gidin

3. **Etkin kurallar** ' ı seçin ve **ad** sütununda **Gelişmiş çok aşamalı saldırı algılamayı** bulun. Bu algılamanın etkin veya devre dışı olduğunu doğrulamak için **durum** sütununu kontrol edin.

4. Durumu değiştirmek için bu girişi seçin ve **Gelişmiş çok aşamalı saldırı algılama** dikey penceresinde **Düzenle**' yi seçin.

5. **Kural oluşturma Sihirbazı** dikey penceresinde durum değişikliği sizin için otomatik olarak seçilir, bu nedenle **İleri**' yi seçin ve ardından **Kaydet**' i seçin. 

Kural şablonları gelişmiş çok aşamalı saldırı algılama için geçerli değildir.

> [!NOTE]
> Azure Sentinel, makine öğrenimi sistemlerini eğitmek için şu anda 30 günlük geçmiş veri kullanmaktadır. Bu veriler, makine öğrenimi ardışık düzeninde geçerken Microsoft 'un anahtarları kullanılarak her zaman şifrelenir. Ancak, Azure Sentinel çalışma alanınızda CMK 'yı etkinleştirdiyseniz eğitim verileri, [müşteri tarafından yönetilen anahtarlar (CMK)](customer-managed-keys.md) kullanılarak şifrelenmez. Fusion 'un devre dışı bırakılması için **Azure Sentinel**   \>  **yapılandırma**   \>  **Analizi \> etkin kurallar \> Gelişmiş çok aşamalı saldırı algılama** ' ya gidin ve **durum** sütununda **devre dışı bırak** ' ı seçin.

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Palo Alto Networks ve Microsoft Defender ATP kullanarak Fusion

Bu senaryolar, Güvenlik analistleri tarafından kullanılan temel günlüklerin ikisini birleştirir: Palo Alto Networks ve uç nokta algılama günlüklerinden Microsoft Defender ATP. Aşağıda listelenen tüm senaryolarda, bir dış IP adresi içeren bitiş noktasında şüpheli bir etkinlik algılanır, ardından bu, dış IP adresinden güvenlik duvarından geri gelen anormal trafik tarafından izlenir. Palo Alto günlüklerinde, Azure Sentinel [tehdit günlüklerine](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs)odaklanır ve tehditlere izin verildiğinde trafik şüpheli olarak değerlendirilir (şüpheli veriler, dosyalar, floods, paketler, taramalar, casus yazılım, URL 'ler, virüsler, güvenlik açıkları, yavalar, yavalar, yavalar).

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Anonim olarak çalışan hizmeti olan ağ isteği, Palo Alto Networks güvenlik duvarı tarafından işaretlenen anormal trafik tarafından izlenir.

Bu senaryoda, Azure Sentinel önce, Microsoft Defender Gelişmiş tehdit koruması 'nın anormal etkinliklere yol açabilecek bir TOR anonim seçme hizmetine bir ağ isteği algıladığını belirten bir uyarı algılar. Bu, {Account Name} hesabı altında {SID} SID KIMLIĞIYLE {Time} saatinde başlatıldı. Bağlantıya giden IP adresi {Kişiselleştiralip} idi.
Daha sonra, {TimeGenerated} konumundaki Palo Alto Networks güvenlik duvarı tarafından olağandışı etkinlik algılandı. Bu, ağınıza girilen kötü amaçlı trafiğin, ağ trafiği için hedef IP adresinin {Destinationıp} olduğunu gösterir.

Bu senaryo şu anda genel önizlemededir.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell, Palo Alto Networks güvenlik duvarı tarafından işaretlenen anormal trafik tarafından izlenen şüpheli bir ağ bağlantısı yaptı.

Bu senaryoda, Azure Sentinel önce, Microsoft Defender Gelişmiş tehdit koruması 'nın, PowerShell 'in bir Palo Alto ağı güvenlik duvarı tarafından algılanan anormal etkinlikte şüpheli bir ağ bağlantısı yaptığını algıladığını bildiren bir uyarı algılar. Bu, {Account Name} hesabı tarafından {SID} SID KIMLIĞIYLE {Time} saatinde başlatıldı. Bağlantıya giden IP adresi {Kişiselleştiralip} idi. Daha sonra, {TimeGenerated} konumundaki Palo Alto Networks güvenlik duvarı tarafından olağandışı etkinlik algılandı. Bu, kötü amaçlı trafiğin ağınıza girdiği anlamına gelir. Ağ trafiği için hedef IP adresi {Destinationıp}.

Bu senaryo şu anda genel önizlemededir.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>, Palo Alto Networks güvenlik duvarı tarafından işaretlenen anormal trafik tarafından izlenen, yetkisiz erişim girişimleri geçmişi ile IP 'ye giden bağlantı

Bu senaryoda, Azure Sentinel, Microsoft Defender Gelişmiş tehdit koruması 'nın, Palo Alto Networks güvenlik duvarı tarafından algılanan anormal etkinliklere yol açabilecek bir IP adresine giden bağlantı algıladığını belirten bir uyarı algılar. Bu, {Account Name} hesabı tarafından {SID} SID KIMLIĞIYLE {Time} saatinde başlatıldı. Bağlantıya giden IP adresi {Kişiselleştiralip} idi. Bundan sonra, {TimeGenerated} konumundaki Palo Alto Networks güvenlik duvarı tarafından olağan dışı etkinlik algılandı. Bu, kötü amaçlı trafiğin ağınıza girdiği anlamına gelir. Ağ trafiği için hedef IP adresi {Destinationıp}.

Bu senaryo şu anda genel önizlemededir.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Kimlik koruması ve Microsoft Cloud App Security kullanarak Fusion

Azure Sentinel, gelişmiş çok aşamalı saldırı algılama kullanarak, Azure Active Directory Kimlik Koruması ve Microsoft Cloud App Security anomali olaylarını birleştiren aşağıdaki senaryoları destekler:

- [Anormal Office 365 etkinliğinin ardından gelen bir konuma imkansız seyahat](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Bilinmeyen konum için oturum açma etkinliği ve anormal Office 365 etkinliği](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Virüslü cihazdan sonra gelen oturum açma etkinliği, anormal Office 365 etkinliği](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Anonim IP adresinden sonra, anormal Office 365 etkinliğinin oturum açma etkinliği](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Kullanıcı tarafından sızdırılan kimlik bilgilerine sahip ve anormal Office 365 etkinliğinin oturum açma etkinliği](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

[Azure AD kimlik koruması Data Connector](connect-azure-ad-identity-protection.md) ve [Cloud App Security](connect-cloud-app-security.md) bağlayıcıları yapılandırılmış olmalıdır.

Aşağıdaki açıklamalarda, Azure Sentinel, bu sayfada temsil edilen verilerinizde bulunan gerçek değeri köşeli ayraç içindeki değişken olarak görüntüler. Örneğin, yerine bir hesabın gerçek görünen adı \<*account name*> ve yerine gerçek sayı \<*number*> .

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Anormal Office 365 etkinliğinin ardından gelen bir konuma imkansız seyahat

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından gelen olağan konuma karşı olası bir yolculuızı birleştiren yedi Azure Sentinel olayı vardır:

- **Office 365 posta kutusu için önde gelen konumlara yönelik imkansız seyahat**
    
    Bu uyarı, bir \<*account name*> \<*location*> kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı tarafından, olağan dışı bir konuma kadar bir oturum açma olayının göstergesidir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı, \<*account name*> gelen tüm e-postaları dış adrese ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş \<*email address*> .

- **Şüpheli bulut uygulaması yönetim etkinliğine yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının \<*account name*> , imkansız bir yolculuya bir sıradan bir konuma kadar olan bir göstergesidir \<*location*> .
    
    Ardından, hesap, \<*account name*> \<*number*> tek bir oturumda yönetim etkinlikleri üzerinden gerçekleştirilir.

- **Toplu dosya silmeye yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, genel bir konuma göre bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyaları sildi.

- **Toplu dosyanın indirileceği sıradan konumlara yönelik imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının \<*account name*> , imkansız bir yolculuya bir sıradan bir konuma kadar olan bir göstergesidir \<*location*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden indirilir.

- **Office 365 kimliğe bürünme için önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının \<*account name*> , imkansız bir yolculuya bir sıradan bir konuma kadar olan bir göstergesidir \<*location*> . 
    
    Daha sonra hesap, \<*account name*> \<*number of activities*> tek bir oturumda kimliğe bürünme etkinliklerinin olağan dışı bir miktarını () gerçekleştirdi.

- **Toplu dosya paylaşımına önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının \<*account name*> , imkansız bir yolculuya bir sıradan bir konuma kadar olan bir göstergesidir \<*location*> . 
    
    Bundan sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının \<*account name*> , imkansız bir yolculuya bir sıradan bir konuma kadar olan bir göstergesidir \<*location*> . 
    
    Ardından, hesap \<*account name*> dosyaları karşıya yükledi \<*number of*> ve toplam \<*number of*> Dosya sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Bilinmeyen konum için oturum açma etkinliği ve anormal Office 365 etkinliği

Azure AD Kimlik Koruması ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarından bilmediğiniz konum uyarıları için oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır.

- **Bilinmeyen bir konumdan Exchange Online posta kutusu için bir oturum açma olayı lider**
    
    Bu uyarı, bilinmeyen bir konumdan, bir \<*account name*> \<*location*> kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı ayarlanmış olan bir oturum açma olayının göstergesidir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı, \<*account name*> gelen tüm e-postaları dış adrese ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş \<*email address*> . 

- **Bilinmeyen bir konumdan şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma olayı**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Ardından, hesap, \<*account name*> \<*number of*> tek bir oturumda yönetim etkinlikleri üzerinden gerçekleştirilir.

- **Bilmediğiniz bir konumdan toplu dosya silmeye kadar olan oturum açma etkinliği**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyaları sildi.

- **Bilinmeyen bir konumdan yığın dosya indirme için bir oturum açma olayı**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden indirilir.

- **Bilmediğiniz bir konumdan Office 365 kimliğe bürünmeye önde gelen oturum açma olayı**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> .
    
    Daha sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki farklı hesapların kimliğine büründü.

- **Bilinmeyen bir konumdan yığın dosya paylaşımına önde gelen oturum açma olayı**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Bundan sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden paylaşılır.

- **Tanınmayan bir konumdan bulut uygulamasındaki fidye 'ya lider olarak oturum açma etkinliği**
    
    Bu uyarı, bilinmeyen bir konumdan tarafından bir oturum açma olayının göstergesidir \<*account name*> \<*location*> . 
    
    Ardından, hesap \<*account name*> dosyaları karşıya yükledi \<*number of*> ve toplam \<*number of*> Dosya sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Virüslü cihazdan sonra gelen oturum açma etkinliği, anormal Office 365 etkinliği

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından etkilenen cihaz uyarılarından oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Virüslü bir cihazdan Office 365 posta kutusu için lider olarak oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlara karşı etkilenen bir cihazdan gelen ve bir kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı ayarlanmış olan bir oturum açma olayının göstergesidir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı, \<*account name*> gelen tüm e-postaları dış adrese ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş \<*email address*> . 

- **Virüslü bir cihazdan şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir.
    
    Ardından, hesap, \<*account name*> \<*number of*> tek bir oturumda yönetim etkinlikleri üzerinden gerçekleştirilir.

- **Virüslü bir cihazdan toplu dosya silmeye kadar lider olan oturum açma olayı**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir. 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyaları sildi.

- **Virüslü bir cihazdan toplu dosya indirme için lider olan oturum açma olayı**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir. 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden indirilir.

- **Virüslü bir cihazdan Office 365 kimliğe bürünmeye önde gelen oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir. 
    
    Daha sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki farklı hesapların kimliğine büründü.

- **Virüslü bir cihazdan toplu dosya paylaşımına lider olarak oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir. 
    
    Bundan sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden paylaşılır.

- **Etkilenen bir cihazdan, bulut uygulamasındaki fidye 'ya lider olarak oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan oturum açma olayının göstergesidir. 
    
    Ardından, hesap \<*account name*> dosyaları karşıya yükledi \<*number of*> ve toplam \<*number of*> Dosya sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Anonim IP adresinden sonra, anormal Office 365 etkinliğinin oturum açma etkinliği

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından anonim IP adresi uyarılarından oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Anonim bir IP adresinden Office 365 posta kutusu için önde gelen oturum açma olayı**
    
    Bu uyarı, bir \<*account name*> \<*IP address*> kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı tarafından izlenen bir anonım Proxy IP adresinden gelen oturum açma olayının göstergesidir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı, \<*account name*> gelen tüm e-postaları dış adrese ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş \<*email address*> . 

- **Anonim bir IP adresinden, şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Ardından, hesap, \<*account name*> \<*number of*> tek bir oturumda yönetim etkinlikleri üzerinden gerçekleştirilir.

- **Anonim bir IP adresinden toplu dosya silmeye kadar bir oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyaları sildi.

- **Anonim bir IP adresinden yığın dosya indirmelerine kadar bir oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden indirilir.

- **Anonim bir IP adresinden Office 365 kimliğe bürünmeye önde gelen oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Daha sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki farklı hesapların kimliğine büründü.

- **Anonim bir IP adresinden yığın dosya paylaşımına önde gelen oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Bundan sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya anonim IP adresinden oturum açma olayı**
    
    Bu uyarı, \<*account name*> anonim bir proxy IP adresinden bir oturum açma olayının göstergesidir \<*IP address*> . 
    
    Ardından, hesap \<*account name*> dosyaları karşıya yükledi \<*number of*> ve toplam \<*number of*> Dosya sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Kullanıcı tarafından sızdırılan kimlik bilgilerine sahip ve anormal Office 365 etkinliğinin oturum açma etkinliği

Azure AD Kimlik Koruması ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarından sızdırılan kimlik bilgileri uyarıları olan kullanıcıdan oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan Office 365 posta kutusu için oturum açma etkinliği**
    
    Bu uyarı, \<*account name*> kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı tarafından, sızdırılan kimlik bilgilerini kullanan oturum açma olayının ayarlandığını belirten bir göstergesidir. 
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı, \<*account name*> gelen tüm e-postaları dış adrese ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş \<*email address*> . 

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan gelen oturum açma olayı, şüpheli bulut uygulaması yönetim etkinliğine**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir.
    
    Ardından, hesap, \<*account name*> \<*number of*> tek bir oturumda yönetim etkinlikleri üzerinden gerçekleştirilir.

- **Sızdırılan kimlik bilgilerine sahip kullanıcı tarafından yığın dosyası silinmeye birlikte oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir.
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyaları sildi.

- **Sızdırılan kimlik bilgilerine sahip kullanıcı tarafından yığın dosyası indirme için oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir.
    
    Ardından, hesap \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden indirilir.

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan Office 365 kimliğe bürünme ile oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir. 
    
    Daha sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki farklı hesapların kimliğine büründü.

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan yığın dosya paylaşımına lider olarak oturum açma etkinliği**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir.
    
    Bundan sonra hesap, \<*account name*> \<*number of*> tek bir oturumdaki benzersiz dosyalar üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya sızdırılan kimlik bilgilerine sahip kullanıcıdan oturum açma olayı**
    
    Bu uyarı, oturum açma olayının \<*account name*> sızdırılan kimlik bilgilerini kullandığının göstergesidir. 
    
    Ardından, hesap \<*account name*> dosyaları karşıya yükledi \<*number of*> ve toplam \<*number of*> Dosya sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş çok aşamalı saldırı algılama hakkında daha fazla bilgi edindiniz, verilerinizi ve olası tehditleri nasıl algılayacağınızı öğrenmek için aşağıdaki hızlı başlangıç ile ilgileniyor olabilirsiniz: [Azure Sentinel ile çalışmaya](quickstart-get-visibility.md)başlayın.

Sizin için oluşturulan olayları araştırmaya hazırsanız aşağıdaki öğreticiye bakın: [Azure Sentinel ile olayları araştırın](tutorial-investigate-cases.md).

