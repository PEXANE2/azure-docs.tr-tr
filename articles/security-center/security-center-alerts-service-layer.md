---
title: Azure hizmet katmanı tehdit algılama-Azure Güvenlik Merkezi
description: Bu konu başlığı altında, Azure Güvenlik Merkezi 'nde bulunan Azure hizmet katmanı uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 08/25/2019
ms.author: memildin
ms.openlocfilehash: 7db9f50b4fb1a9309737f05db13a914f414372ed
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186497"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit algılama

Bu konu, aşağıdaki Azure hizmeti katmanlarını izlerken kullanılabilen Azure Güvenlik Merkezi uyarılarını gösterir:

* [Azure ağ katmanı](#network-layer)
* [Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)](#management-layer)
* [Azure Anahtar Kasası.](#azure-keyvault)

>[!NOTE]
>Aşağıdaki analiz tüm kaynak türleri için geçerlidir. Azure iç akışlarına dokunarak Güvenlik Merkezi 'nin sağladığı telemetrisi kullanır.

## Azure ağ katmanı<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analizi, Azure çekirdek yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [ıpdüzeltme verilerini](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)temel alır. Bu veri akışına bağlı olarak, güvenlik merkezi makine öğrenimi modelleri kötü amaçlı trafik etkinliklerini belirler ve işaretlerdir. Güvenlik Merkezi, IP adreslerini zenginleştirmek için Microsoft Threat Intelligence veritabanını kullanır.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Şüpheli giden RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir. Bu, kaynağınızın güvenliğinin aşıldığını gösterebilir ve artık bir dış RDP uç noktasına deneme yanılma saldırısı için kullanılmaktadır. Bu tür bir etkinlik, IP 'nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabilir.|
|**Birden çok hedefe şüpheli giden RDP ağ etkinliği**|Örnek ağ trafiği analizi, dağıtımınızdaki bir kaynaktan birden çok hedefe ait olan anormal giden RDP iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir. Bu, kaynağınızın güvenliğinin aşıldığını gösterebilir ve bu durumda, artık dış RDP uç noktalarına deneme saldırısı için kullanılmaktadır. Bu tür bir etkinlik, IP 'nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabilir.|
|**Şüpheli giden SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan kaynaklanan anormal giden Secure Shell (SSH) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir. Bu, kaynağınızın güvenliğinin aşıldığını gösterebilir ve artık bir dış SSH uç noktasına deneme yanılma saldırısı için kullanılmaktadır. Bu tür bir etkinlik, IP 'nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabilir.|
|**Birden çok hedefe şüpheli giden SSH ağ etkinliği**|Örnek ağ trafiği analizi, dağıtımınızdaki bir kaynaktan birden çok hedefe ait olan anormal giden SSH iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir. Bu, kaynağınızın güvenliğinin aşıldığını gösterebilir ve bu durumda, artık dış SSH uç noktaları deneme yanılma saldırısı için kullanılıyor olabilir. Bu tür bir etkinlik, IP 'nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabilir.|
|**Birden çok kaynaktan şüpheli gelen SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, birden çok kaynaktan dağıtımınızdaki bir kaynağa anormal gelen SSH iletişimleri algıladı. Bu ortamda, kaynağına bağlanan çeşitli benzersiz IP 'Ler anormal olarak kabul edilir. Bu etkinlik, birden çok ana bilgisayardan (botnet) SSH arabiriminize bir deneme yanılma girişimi olduğunu gösterebilir.|
|**Şüpheli gelen SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynağa anormal gelen SSH iletişimi algıladı. Bu ortamda, kaynağına görece yüksek sayıda gelen bağlantı, bu ortam için anormal olarak kabul edilir. Bu etkinlik, SSH arabiriminize bir deneme yanılma saldırısı denemesi olduğunu gösterebilir.
|**Birden çok kaynaktan şüpheli gelen RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, birden çok kaynaktan dağıtımınızdaki bir kaynağa anormal gelen RDP iletişimleri algıladı. Bu ortamda, kaynağına bağlanan çeşitli benzersiz IP 'Ler anormal olarak kabul edilir. Bu etkinlik, birden çok ana bilgisayardan (botnet) RDP arabiriminize bir deneme yanılma girişimi olduğunu gösterebilir.|
|**Şüpheli gelen RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynağa anormal gelen RDP iletişimi algıladı. Bu ortamda, kaynağına görece yüksek sayıda gelen bağlantı, bu ortam için anormal olarak kabul edilir. Bu etkinlik, SSH arabiriminize bir deneme yanılma saldırısı denemesi olduğunu gösterebilir.|
|**Kötü amaçlı bir adresle ağ iletişimi algılandı**|Örneklenmiş ağ trafiği analizi, olası bir komut ve denetim (C & C) sunucusuyla dağıtımınızdaki bir kaynaktan kaynaklanan iletişimi algıladı. Bu tür bir etkinlik, IP 'nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabilir.|

Güvenlik Merkezi 'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceğini anlamak için bkz. [Azure Güvenlik Merkezi 'Nde BULUŞSAL DNS algılamaları](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).

>[!NOTE]
>Azure Güvenlik Merkezi 'nde Azure ağ katmanı tehdit algılama uyarıları yalnızca, şüpheli bir iletişimin gerçekleştiği saatin tamamında aynı IP adresi atanmış sanal makinelerde oluşturulur. Bu, sanal makinelerin yanı sıra, yönetilen hizmetin bir parçası olarak müşterinin aboneliğinde oluşturulan sanal makineler için de geçerlidir (örneğin, AKS, Databricks).

## Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)<a name ="management-layer"></a>

>[!NOTE]
>Azure Resource Manager tabanlı güvenlik merkezi koruma katmanı Şu anda önizleme aşamasındadır.

Güvenlik Merkezi, Azure için denetim düzlemi olarak kabul edilen Azure Resource Manager olaylarını kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Resource Manager kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya potansiyel olarak zararlı işlemleri algılar.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Mikro patlama araç seti çalıştırması**|Ortamınızda bilinen bir Cloud-Environment keşif Toolkit çalıştırması algılandı. Araç [mikro patlama](https://github.com/NetSPI/MicroBurst) , aboneliklerinin kaynaklarını eşlemek, güvenli olmayan konfigürasyonları belirlemek ve gizli bilgileri sızan bir saldırgan (veya sızma Sınayıcısı) tarafından kullanılabilir.|
|**Azurite araç seti çalıştırma**|Ortamınızda bilinen bir Cloud-Environment keşif Toolkit çalıştırması algılandı. Aracı [Azurite](https://github.com/mwrlabs/Azurite) , aboneliklerinin kaynaklarını eşlemek ve güvenli olmayan yapılandırmaların tanımlanması için bir saldırgan (veya sızma Sınayıcısı) tarafından kullanılabilir.|
|**Etkin olmayan bir hesap kullanan şüpheli yönetim oturumu**|Abonelik etkinlik günlükleri Analizi şüpheli davranış algıladı. Uzun bir süre boyunca kullanımda olmayan bir sorumlu, bir saldırgan için kalıcılığı güvenli hale getirmeye yönelik eylemler gerçekleştiriyor.|
|**PowerShell kullanarak şüpheli yönetim oturumu**|Abonelik etkinlik günlükleri Analizi şüpheli davranış algıladı. Abonelik ortamını yönetmek için düzenli olarak PowerShell kullanmayan bir sorumlu artık PowerShell kullanıyor ve bir saldırgan için kalıcılığı güvenli hale getirmenize yönelik eylemler gerçekleştiriyor.|
|**Gelişmiş Azure Kalıcılık teknikleri kullanımı**|Abonelik etkinlik günlükleri Analizi şüpheli davranış algıladı. Özelleştirilmiş rollere, yasallı kimlik varlıkları verildi. Bu, saldırganın bir Azure müşteri ortamında kalıcılığı sağlamasına yol açabilir.|
|**Sık erişilmeyen ülkeden etkinlik**|Kuruluştaki herhangi bir kullanıcı tarafından kısa süre önce veya hiç ziyaret edilmemiş bir konumdan gelen etkinlik oluştu.<br/>Bu algılama yöntemi, yeni ve sık erişilmeyen konumlarını belirlemek için etkinlik konumları dikkate alır. Anomali algılama altyapısında, kuruluşunuzdaki kullanıcılar tarafından kullanılan önceki konumları hakkında bilgi depolar. 
|**Anonim IP adreslerinden etkinliği**|Anonim bir proxy IP adresi olarak tanımlanmış bir IP adresinden gelen kullanıcılar etkinliği algılandı. <br/>Bu proxy 'ler, cihazının IP adreslerini gizlemek isteyen kişiler tarafından kullanılır ve kötü amaçlı olarak kullanılabilir. Bu algılama, kuruluştaki kullanıcılar tarafından yaygın olarak kullanılan hatalı etiketli IP adresleri gibi hatalı pozitif sonuçları azaltan bir makine öğrenimi algoritması kullanır.|
|**Mümkün olmayan seyahat algılandı**|Coğrafi olarak uzak konumlardan kaynaklanan iki kullanıcı etkinliği (tek veya birden çok oturumunda) oluştu. Bu, kullanıcının ilk konumdan ikincisine gitmesini sağlamak için geçen süreden daha kısa bir süre içinde gerçekleşir. Bu, farklı bir kullanıcının aynı kimlik bilgilerini kullandığını gösterir. <br/>Bu algılama, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan VPN 'Ler ve konumlar gibi imkansız seyahat koşullarına katkıda bulunan açık olmayan hatalı pozitif durumları yok sayan bir makine öğrenimi algoritması kullanır. Algılama işlemi, yedi günlük ilk öğrenme dönemine sahiptir ve bu sırada yeni bir kullanıcının etkinlik modelini öğrenir.|

>[!NOTE]
> Yukarıdaki analizler Microsoft Cloud App Security tarafından desteklenmektedir. Bu analizlerden yararlanmak için bir Cloud App Security lisansını etkinleştirmeniz gerekir. Cloud App Security lisansınız varsa, bu uyarılar varsayılan olarak etkinleştirilir. Devre dışı bırakmak için:
>
> 1. **Güvenlik Merkezi** dikey penceresinde **güvenlik ilkesi**' ni seçin. Değiştirmek istediğiniz abonelik için **Ayarları Düzenle**' yi seçin.
> 2. **Tehdit algılamayı**seçin.
> 3. **Tümleştirmelere**izin ver ' in altında, **verilerinize erişim Microsoft Cloud App Security izin ver**' i temizleyin ve **Kaydet**' i seçin

>[!NOTE]
>Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynakla aynı coğrafi bölgede depolar. Microsoft, kaynak coğrafi bölgede henüz Güvenlik Merkezi dağıtmamışsa, verileri Birleşik Devletler depolar. Cloud App Security etkinleştirildiğinde, bu bilgiler Cloud App Security coğrafi konum kurallarına uygun olarak depolanır. Daha fazla bilgi için bkz. [bölgesel olmayan hizmetler Için veri depolama](https://azuredatacentermap.azurewebsites.net/).

## Azure Key Vault<a name="azure-keyvault"></a>

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

Azure Güvenlik Merkezi, ek bir güvenlik zekası katmanı sağlayan Azure Key Vault için Azure yerel, Gelişmiş tehdit koruması içerir. Güvenlik Merkezi, Key Vault hesaplarına erişme veya açıktan yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanıza olanak sağlar.  

Anormal etkinlikler gerçekleştiğinde, güvenlik merkezi uyarıları gösterir ve isteğe bağlı olarak bunları abonelik yöneticilerine e-posta ile gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. 

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Bir TOR çıkış düğümünden Key Vault erişim**|Key Vault, konumunu gizlemek için TOR IP anonimleştirme sistemini kullanan birisi tarafından erişilir. Kötü amaçlı aktörler, internet 'e bağlı kaynaklara yetkisiz erişim kazanmaya çalışırken genellikle konumlarını gizlemeyi dener.|
|**Key Vault şüpheli ilke değişikliği ve gizli sorgu**|Key Vault bir ilke değişikliği yapılmıştır ve gizli dizileri listeleme ve/veya almaya yönelik işlemler oluştu. Buna ek olarak, bu işlem, normalde bu kasadaki Kullanıcı tarafından gerçekleştirilmez. Bu, Key Vault güvenliğinin aşıldığını ve içindeki parolaların kötü amaçlı bir aktör tarafından çalındığını oldukça önemlidir.|
|**Key Vault şüpheli gizli anahtar listesi ve sorgu**|Gizli bir liste işlemi daha sonra birçok gizli get işlemi tarafından gerçekleştirildi. Ayrıca, bu işlem kalıbı normalde bu kasadaki Kullanıcı tarafından gerçekleştirilmez. Bu, birisinin kötü amaçlı olabilecek amaçlar için Key Vault depolanan gizli dizileri dökme anlamına gelir.|
|**Olağan dışı Kullanıcı-uygulama çifti bir Key Vault erişti**|Key Vault, normalde ona erişebilen bir Kullanıcı-uygulama eşleştirmesi tarafından erişilir. Bu, meşru bir erişim denemesi (örneğin, bir altyapı veya kod güncelleştirmesini takip etmek) olabilir. Bu ayrıca altyapınızın güvenliğinin aşıldığını ve kötü niyetli bir aktörün Key Vault erişmeye çalıştığı bir göstergesidir.|
|**Olağan dışı uygulama bir Key Vault erişti**|Key Vault, normalde bu uygulamaya erişmeyen bir uygulama tarafından erişilir. Bu, meşru bir erişim denemesi (örneğin, bir altyapı veya kod güncelleştirmesini takip etmek) olabilir. Bu ayrıca altyapınızın güvenliğinin aşıldığını ve kötü niyetli bir aktörün Key Vault erişmeye çalıştığı bir göstergesidir.|
|**Olağan dışı Kullanıcı bir Key Vault erişti**|Key Vault, normalde bu kullanıcıya erişmeyen bir kullanıcı tarafından erişilmiştir. Bu, meşru bir erişim denemesi (örneğin, erişim gerektiren yeni bir Kullanıcı kuruluşa katılmış) olabilir. Bu ayrıca altyapınızın güvenliğinin aşıldığını ve kötü niyetli bir aktörün Key Vault erişmeye çalıştığı bir göstergesidir.|
|**Key Vault olağan dışı işlem kriteri**|Geçmiş verilerle karşılaştırıldığında olağan dışı bir Key Vault işlem kümesi gerçekleştirildi. Key Vault etkinlik genellikle zaman içinde aynıdır. Bu, etkinlikte meşru bir değişiklik olabilir. Alternatif olarak, altyapınız tehlikeye girebilir ve daha fazla araştırma yapılması gerekir.|
|**Key Vault yüksek hacimli işlemler**|Geçmiş verilerle karşılaştırıldığında Key Vault işlemlerin daha büyük bir hacmi gerçekleştirildi. Key Vault etkinlik genellikle zaman içinde aynıdır. Bu, etkinlikte meşru bir değişiklik olabilir. Alternatif olarak, altyapınız tehlikeye girebilir ve daha fazla araştırma yapılması gerekir.|
|**Kullanıcı, önemli kasaların yüksek hacminde erişti**|Bir kullanıcının veya uygulamanın eriştiği kasaların sayısı geçmiş verilerle karşılaştırıldığında değişmiştir. Key Vault etkinlik genellikle zaman içinde aynıdır. Bu, etkinlikte meşru bir değişiklik olabilir. Alternatif olarak, altyapınız tehlikeye girebilir ve daha fazla araştırma yapılması gerekir.|