---
title: Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama
description: Uyarı fatıg 'yi azaltmak ve gelişmiş çok aşamalı saldırı algılamasına dayalı eylem yapılabilir olaylar oluşturmak için Azure Sentinel 'de Fusion teknolojisini kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/12/2020
ms.author: rkarlin
ms.openlocfilehash: ada2ad67bc3634d8e6a31d3c8a69fc0c8b08a93a
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2020
ms.locfileid: "77369698"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Azure Sentinel 'de gelişmiş çok aşamalı saldırı algılama

Azure Sentinel, Machine Learning 'i temel alan Fusion teknolojisini kullanarak çok aşamalı saldırıları otomatik olarak algılayabilir ve bu da sonlandırma zincirinin çeşitli aşamalarında gözlemlenen anormal davranışları ve şüpheli etkinlikleri birleştirerek. Azure Sentinel, daha sonra yakalamak çok zor olan olaylar oluşturur. Bu olaylar iki veya daha fazla uyarıyı veya etkinliği büyük bir şekilde kaydeder. Tasarım yaparak, bu olaylar düşük hacimdir, yüksek uygunlukta ve yüksek öneme sahiptir.

Ortamınız için özelleştirilmiş, bu algılama yalnızca hatalı pozitif oranları azaltmıyor ancak sınırlı veya eksik bilgilere sahip saldırıları tespit edebilir.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Gelişmiş çok aşamalı saldırı algılama yapılandırması

Bu algılama, Azure Sentinel 'de varsayılan olarak etkinleştirilmiştir. Durumu denetlemek veya birden çok uyarıyı temel alan olaylar oluşturmak için alternatif bir çözüm kullandığınız için belki de devre dışı bırakmak için aşağıdaki yönergeleri kullanın:

1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın

2. **Azure Sentinel** > **Configuration** > **Analytics** 'e gidin

3. **Etkin kurallar** ' ı seçin ve **ad** sütununda **Gelişmiş çok aşamalı saldırı algılamayı** bulun. Bu algılamanın etkin veya devre dışı olduğunu doğrulamak için **durum** sütununu kontrol edin.

4. Durumu değiştirmek için bu girişi seçin ve **Gelişmiş çok aşamalı saldırı algılama** dikey penceresinde **Düzenle**' yi seçin.

5. **Kural oluşturma Sihirbazı** dikey penceresinde durum değişikliği sizin için otomatik olarak seçilir, bu nedenle **İleri**' yi seçin ve ardından **Kaydet**' i seçin. 

Kural şablonları gelişmiş çok aşamalı saldırı algılama için geçerli değildir.

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Palo Alto Networks ve Microsoft Defender ATP kullanarak Fusion

- XML 'nin anonim olarak oluşturulmasına yönelik ağ isteği, Palo Alto Networks güvenlik duvarı tarafından işaretlenen anormal trafik tarafından izlenebilir

- PowerShell, Palo Alto Networks güvenlik duvarı tarafından bayrak eklenmiş anormal trafik tarafından izlenen şüpheli bir ağ bağlantısı yaptı

- , Palo Alto Networks güvenlik duvarı tarafından işaretlenen anormal trafik tarafından izlenen, yetkisiz erişim girişimleri geçmişi ile IP 'ye giden bağlantı



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Kimlik koruması ve Microsoft Cloud App Security kullanarak Fusion

Azure Sentinel, gelişmiş çok aşamalı saldırı algılama kullanarak, Azure Active Directory Kimlik Koruması ve Microsoft Cloud App Security anomali olaylarını birleştiren aşağıdaki senaryoları destekler:

- [Anormal Office 365 etkinliğinin ardından gelen bir konuma imkansız seyahat](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Bilinmeyen konum için oturum açma etkinliği ve anormal Office 365 etkinliği](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Virüslü cihazdan sonra gelen oturum açma etkinliği, anormal Office 365 etkinliği](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Anonim IP adresinden sonra, anormal Office 365 etkinliğinin oturum açma etkinliği](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Kullanıcı tarafından sızdırılan kimlik bilgilerine sahip ve anormal Office 365 etkinliğinin oturum açma etkinliği](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

[Azure AD kimlik koruması Data Connector](connect-azure-ad-identity-protection.md) ve [Cloud App Security](connect-cloud-app-security.md) bağlayıcıları yapılandırılmış olmalıdır.

Aşağıdaki açıklamalarda, Azure Sentinel, bu sayfada temsil edilen verilerinizde bulunan gerçek değeri köşeli ayraç içindeki değişken olarak görüntüler. Örneğin, \<*Hesap adı*> yerine hesabın gerçek görünen adı ve \<*sayı*> yerine gerçek sayı.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Anormal Office 365 etkinliğinin ardından gelen bir konuma imkansız seyahat

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından gelen olağan konuma karşı olası bir yolculuızı birleştiren yedi Azure Sentinel olayı vardır:

- **Office 365 posta kutusu için önde gelen konumlara yönelik imkansız seyahat**
    
    Bu uyarı, bir oturum açma olayının bir göstergesi olan \<*Hesap adı*, \<*konuma*>, olağan dışı bir konum ve bir kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı ayarlanmış bir >.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı \<*Hesap adı*> gelen tüm e-postaları dış adrese \<*e-posta adresi*> ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş.

- **Şüpheli bulut uygulaması yönetim etkinliğine yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, \<*Hesap adı*tarafından, olağan dışı bir konuma \<*konuma*> mümkün olmayan bir gezden > bir oturum açma olayının göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki yönetim etkinlikleri > \<*numara*üzerinden gerçekleştirilir.

- **Toplu dosya silmeye yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, bir dizi \<*Hesap adının*> \<*konuma*>, genellikle tipik bir konum olduğunu belirtir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısını*sildi.

- **Toplu dosyanın indirileceği sıradan konumlara yönelik imkansız seyahat**
    
    Bu uyarı, \<*Hesap adı*tarafından, olağan dışı bir konuma \<*konuma*> mümkün olmayan bir gezden > bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*>, tek bir oturumda \<*sayıda*> benzersiz dosyanın üzerinden indirilir.

- **Office 365 kimliğe bürünme için önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, \<*Hesap adı*tarafından, olağan dışı bir konuma \<*konuma*> mümkün olmayan bir gezden > bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki kimliğe bürünme etkinliklerinin olağan dışı bir miktarını (\<*etkinlik*>) gerçekleştirdi.

- **Toplu dosya paylaşımına önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, \<*Hesap adı*tarafından, olağan dışı bir konuma \<*konuma*> mümkün olmayan bir gezden > bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısı*üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya yönelik olarak önde gelen konumlara imkansız seyahat**
    
    Bu uyarı, \<*Hesap adı*tarafından, olağan dışı bir konuma \<*konuma*> mümkün olmayan bir gezden > bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı* *> > dosya*\<karşıya yüklendi ve toplam \<*sayıda*> dosyasını sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Bilinmeyen konum için oturum açma etkinliği ve anormal Office 365 etkinliği

Azure AD Kimlik Koruması ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarından bilmediğiniz konum uyarıları için oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır.

- **Bilinmeyen bir konumdan Exchange Online posta kutusu için bir oturum açma olayı lider**
    
    Bu uyarı, \<*Hesap adı*> \<*konumdan*>, bilinmeyen bir konum ve bir kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı ayarlanmış olan bir oturum açma olayının göstergesidir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı \<*Hesap adı*> gelen tüm e-postaları dış adrese \<*e-posta adresi*> ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş. 

- **Bilinmeyen bir konumdan şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma olayı**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir *oturumda \<>* yönetim etkinliği üzerinden yapılır.

- **Bilmediğiniz bir konumdan toplu dosya silmeye kadar olan oturum açma etkinliği**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısını*sildi.

- **Bilinmeyen bir konumdan yığın dosya indirme için bir oturum açma olayı**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*>, tek bir oturumda \<*sayıda*> benzersiz dosyanın üzerinden indirilir.

- **Bilmediğiniz bir konumdan Office 365 kimliğe bürünmeye önde gelen oturum açma olayı**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > farklı hesapların \<*sayısı*üzerinden kimliğe büründü.

- **Bilinmeyen bir konumdan yığın dosya paylaşımına önde gelen oturum açma olayı**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısı*üzerinden paylaşılır.

- **Tanınmayan bir konumdan bulut uygulamasındaki fidye 'ya lider olarak oturum açma etkinliği**
    
    Bu uyarı, \<*konumdan*>, bilmediğiniz bir konumdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı* *> > dosya*\<karşıya yüklendi ve toplam \<*sayıda*> dosyasını sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Virüslü cihazdan sonra gelen oturum açma etkinliği, anormal Office 365 etkinliği

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından etkilenen cihaz uyarılarından oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Virüslü bir cihazdan Office 365 posta kutusu için lider olarak oturum açma etkinliği**
    
    Bu uyarı, kötü amaçlı yazılımlara karşı etkilenen bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir ve bir kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralı ayarlanmış olabilir.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı \<*Hesap adı*> gelen tüm e-postaları dış adrese \<*e-posta adresi*> ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş. 

- **Virüslü bir cihazdan şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma etkinliği**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir *oturumda \<>* yönetim etkinliği üzerinden yapılır.

- **Virüslü bir cihazdan toplu dosya silmeye kadar lider olan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısını*sildi.

- **Virüslü bir cihazdan toplu dosya indirme için lider olan oturum açma olayı**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*>, tek bir oturumda \<*sayıda*> benzersiz dosyanın üzerinden indirilir.

- **Virüslü bir cihazdan Office 365 kimliğe bürünmeye önde gelen oturum açma etkinliği**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > farklı hesapların \<*sayısı*üzerinden kimliğe büründü.

- **Virüslü bir cihazdan toplu dosya paylaşımına lider olarak oturum açma etkinliği**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısı*üzerinden paylaşılır.

- **Etkilenen bir cihazdan, bulut uygulamasındaki fidye 'ya lider olarak oturum açma etkinliği**
    
    Bu uyarı, kötü amaçlı yazılımlardan etkilenme olasılığı bulunan bir cihazdan \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı* *> > dosya*\<karşıya yüklendi ve toplam \<*sayıda*> dosyasını sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Anonim IP adresinden sonra, anormal Office 365 etkinliğinin oturum açma etkinliği

Microsoft Cloud App Security tarafından oluşturulan Azure AD Kimlik Koruması ve anormal Office 365 uyarılarından anonim IP adresi uyarılarından oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Anonim bir IP adresinden Office 365 posta kutusu için önde gelen oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresinden \<*Hesap adı*> bir oturum açma olayının göstergesidir \<*IP adresi*>, sonrasında şüpheli bir gelen kutusu iletme kuralı bir kullanıcının gelen kutusunda ayarlanmış.
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı \<*Hesap adı*> gelen tüm e-postaları dış adrese \<*e-posta adresi*> ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş. 

- **Anonim bir IP adresinden, şüpheli bulut uygulaması yönetim etkinliğine yönelik oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir *oturumda \<>* yönetim etkinliği üzerinden yapılır.

- **Anonim bir IP adresinden toplu dosya silmeye kadar bir oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısını*sildi.

- **Anonim bir IP adresinden yığın dosya indirmelerine kadar bir oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*>, tek bir oturumda \<*sayıda*> benzersiz dosyanın üzerinden indirilir.

- **Anonim bir IP adresinden Office 365 kimliğe bürünmeye önde gelen oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > farklı hesapların \<*sayısı*üzerinden kimliğe büründü.

- **Anonim bir IP adresinden yığın dosya paylaşımına önde gelen oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısı*üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya anonim IP adresinden oturum açma olayı**
    
    Bu uyarı, anonim bir proxy IP adresi \<*IP adresi*> \<*Hesap adı*> bir oturum açma olayının göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı* *> > dosya*\<karşıya yüklendi ve toplam \<*sayıda*> dosyasını sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Kullanıcı tarafından sızdırılan kimlik bilgilerine sahip ve anormal Office 365 etkinliğinin oturum açma etkinliği

Azure AD Kimlik Koruması ve Microsoft Cloud App Security tarafından oluşturulan anormal Office 365 uyarılarından sızdırılan kimlik bilgileri uyarıları olan kullanıcıdan oturum açma etkinliğini birleştiren yedi Azure Sentinel olayı vardır:

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan Office 365 posta kutusu için oturum açma etkinliği**
    
    Bu uyarı, \<*Hesap adı*ile oturum açma olayının, bir kullanıcının gelen kutusunda şüpheli bir gelen kutusu iletme kuralının kullanıldığı > sızdırılan kimlik bilgilerini kullandığının göstergesidir. 
    
    Bu, hesabın güvenliğinin aşıldığını ve bu posta kutusunun kuruluşunuzdaki bilgileri almak için kullanıldığını gösteriyor olabilir. Kullanıcı \<*Hesap adı*> gelen tüm e-postaları dış adrese \<*e-posta adresi*> ileten bir gelen kutusu iletme kuralı oluşturdu veya güncelleştirmiş. 

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan gelen oturum açma olayı, şüpheli bulut uygulaması yönetim etkinliğine**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir *oturumda \<>* yönetim etkinliği üzerinden yapılır.

- **Sızdırılan kimlik bilgilerine sahip kullanıcı tarafından yığın dosyası silinmeye birlikte oturum açma etkinliği**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısını*sildi.

- **Sızdırılan kimlik bilgilerine sahip kullanıcı tarafından yığın dosyası indirme için oturum açma etkinliği**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*>, tek bir oturumda \<*sayıda*> benzersiz dosyanın üzerinden indirilir.

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan Office 365 kimliğe bürünme ile oturum açma etkinliği**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > farklı hesapların \<*sayısı*üzerinden kimliğe büründü.

- **Sızdırılan kimlik bilgilerine sahip kullanıcıdan yığın dosya paylaşımına lider olarak oturum açma etkinliği**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir.
    
    Daha sonra hesap \<*Hesap adı*> tek bir oturumdaki > benzersiz dosya \<*sayısı*üzerinden paylaşılır.

- **Bulut uygulamasındaki fidye 'ya sızdırılan kimlik bilgilerine sahip kullanıcıdan oturum açma olayı**
    
    Bu uyarı \<*Hesap adı*ile oturum açma olayının, sızdırılan kimlik bilgilerini > kullandığı bir göstergesidir. 
    
    Daha sonra hesap \<*Hesap adı* *> > dosya*\<karşıya yüklendi ve toplam \<*sayıda*> dosyasını sildi. 
    
    Bu etkinlik deseninin olası bir fidye yazılımı saldırısı göstergesi vardır.

## <a name="next-steps"></a>Sonraki adımlar

Gelişmiş çok aşamalı saldırı algılama hakkında daha fazla bilgi edindiniz, verilerinizi ve olası tehditleri nasıl algılayacağınızı öğrenmek için aşağıdaki hızlı başlangıç ile ilgileniyor olabilirsiniz: [Azure Sentinel ile çalışmaya](quickstart-get-visibility.md)başlayın.

Sizin için oluşturulan olayları araştırmaya hazırsanız aşağıdaki öğreticiye bakın: [Azure Sentinel ile olayları araştırın](tutorial-investigate-cases.md).

