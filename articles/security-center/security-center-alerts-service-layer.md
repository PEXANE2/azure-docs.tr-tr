---
title: Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit algılama | Microsoft Docs
description: Bu konu başlığı altında, Azure Güvenlik Merkezi 'nde bulunan Azure hizmet katmanı uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/18/2019
ms.author: v-mohabe
ms.openlocfilehash: aae2270417e0bb3bdf0dd847e2e09e7cb1999d39
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972461"
---
# <a name="threat-detection-for-azure-service-layer-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit algılama

Bu konu, aşağıdaki Azure hizmeti katmanlarını izlerken kullanılabilen güvenlik merkezi uyarılarını gösterir.

* [Azure ağ katmanı](#network-layer)
* [Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)](#management-layer)

>[!NOTE]
>Güvenlik Merkezi 'nin Azure iç akışlarına dokunmadan yararlandığından, aşağıda sunulan analizler tüm kaynak türleri için geçerlidir.

## Azure ağ katmanı<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analizi, Azure çekirdek yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [ıpdüzeltme verilerini](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)temel alır. Bu veri akışına bağlı olarak, güvenlik merkezi makine öğrenimi modelleri kötü amaçlı trafik etkinliklerini belirler ve işaretlerdir. Güvenlik Merkezi, IP adreslerini zenginleştirmek için Microsoft 'un tehdit bilgileri veritabanını kullanır.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Şüpheli giden RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikede olduğunu ve şu anda dış RDP uç noktası zorla deneme için kullanıldığını gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Birden çok hedefe şüpheli giden RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan birden çok hedefe kaynaklı anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikede olduğunu ve artık dış RDP uç noktalarını deneme yanılma için kullanıldığını gösteriyor olabilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Şüpheli giden SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan kaynaklanan anormal giden Secure Shell (SSH) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikede olduğunu ve artık dış SSH uç noktası deneme yanılma için kullanıldığını gösteriyor olabilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Birden çok hedefe şüpheli giden SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan birden fazla hedefe kaynaklı anormal giden Secure Shell (SSH) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikede olduğunu ve artık dış SSH uç noktalarına deneme yanılma için kullanıldığını gösterebilir. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Birden çok kaynaktan şüpheli gelen SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, birden çok kaynaktan dağıtımınızdaki bir kaynağa anormal gelen SSH iletişimleri algıladı. Bu ortamda, kaynağına bağlanan çeşitli benzersiz IP 'Ler anormal olarak kabul edilir. Bu etkinlik, birden çok ana bilgisayardan (botnet) SSH arabiriminizi zorlamaya yönelik bir girişim olduğunu gösterebilir.|
|**Şüpheli gelen SSH ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynağa anormal gelen SSH iletişimi algıladı. Görece yüksek sayıda gelen bağlantı, bu ortam için anormal olarak kabul edilir. Bu etkinlik, SSH arabiriminizi zorlamaya yönelik bir girişim olduğunu gösterebilir.
|**Birden çok kaynaktan şüpheli gelen RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, birden çok kaynaktan dağıtımınızdaki bir kaynağa anormal gelen RDP iletişimleri algıladı. Bu ortamda, kaynağına bağlanan çeşitli benzersiz IP 'Ler anormal olarak kabul edilir. Bu etkinlik, birden çok ana bilgisayardan (botnet) RDP arabiriminizi zorlamaya yönelik bir girişim olduğunu gösterebilir.|
|**Şüpheli gelen RDP ağ etkinliği**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynağa anormal gelen RDP iletişimi algıladı. Görece yüksek sayıda gelen bağlantı, bu ortam için anormal olarak kabul edilir. Bu etkinlik, SSH arabiriminizi zorlamaya yönelik bir girişim olduğunu gösterebilir.|
|**Kötü amaçlı bir adresle ağ iletişimi algılandı**|Örneklenmiş ağ trafiği analizi, olası bir komut ve denetim (C & C) sunucusuyla dağıtımınızdaki bir kaynaktan kaynaklanan iletişimi algıladı. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|

Güvenlik Merkezi 'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceğini anlamak için bkz. [Azure Güvenlik Merkezi 'Nde BULUŞSAL DNS algılamaları](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).

>[!NOTE]
>Azure Güvenlik Merkezi 'nde Azure ağ katmanı tehdit algılama uyarıları, VM oluşturulduktan sonra ilk saat boyunca oluşturulmaz.

## Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)<a name ="management-layer"></a>

>[!NOTE]
>Azure Resource Manager tabanlı güvenlik merkezi koruma katmanı Şu anda önizleme aşamasındadır.

Güvenlik Merkezi, Azure için denetim düzlemi olarak kabul edilen Azure Resource Manager olaylarını kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Resource Manager kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya potansiyel olarak zararlı işlemleri algılar.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Mikro patlama araç seti çalıştırması**|Ortamınızda bilinen bir Cloud-Environment keşif Toolkit çalıştırması algılandı. "Mikro patlama" Aracı (bkz https://github.com/NetSPI/MicroBurst) . bir saldırgan (veya sızma Sınayıcısı), abonelik kaynaklarınızı eşlemek, güvenli olmayan konfigürasyonları belirlemek ve gizli bilgileri anlamak için kullanılabilir.|
|**Azurite araç seti çalıştırma**|Ortamınızda bilinen bir Cloud-Environment keşif Toolkit çalıştırması algılandı. "Azurıite" Aracı (bkz https://github.com/mwrlabs/Azurite) . bir saldırgan (veya sızma Sınayıcısı), abonelik kaynaklarınızı eşlemek ve güvenli olmayan konfigürasyonları belirlemek için kullanılabilir.|
|**Etkin olmayan bir hesap kullanan şüpheli yönetim oturumu**|Abonelik etkinlik günlükleri Analizi şüpheli bir davranış algıladı. Uzun bir süre boyunca kullanımda olmayan bir sorumlu, artık bir saldırgan için kalıcılığı güvenli hale getirmek için işlem gerçekleştiriyor.|
|**PowerShell kullanarak şüpheli yönetim oturumu**|Abonelik etkinlik günlükleri Analizi şüpheli bir davranış algıladı. Abonelik ortamını yönetmek için düzenli olarak PowerShell kullanmayan bir sorumlu, artık PowerShell kullanıyor ve bir saldırganın kalıcılığını güvenli hale getirmeye yönelik eylemler gerçekleştiriyor.|
|**Gelişmiş Azure Kalıcılık teknikleri kullanımı**|Abonelik etkinlik günlükleri Analizi şüpheli bir davranış algıladı. Özelleştirilmiş rollere, yasallı kimlik varlıkları verildi. Bu, saldırganın bir Azure müşteri ortamında kalıcılığı sağlamasına yol açabilir.|
|**Sık erişilmeyen ülkeden etkinlik**|Son zamanlarda veya kuruluştaki hiçbir Kullanıcı tarafından hiç ziyaret edilmemiş bir konumdan gelen etkinlik oluştu.<br/>Bu algılama yöntemi, yeni ve sık erişilmeyen konumlarını belirlemek için etkinlik konumları dikkate alır. Anomali algılama altyapısında, kuruluşunuzdaki kullanıcılar tarafından kullanılan önceki konumları hakkında bilgi depolar. 
|**Anonim IP adreslerinden etkinliği**|Anonim bir proxy IP adresi olarak tanımlanmış bir IP adresinden gelen kullanıcılar etkinliği algılandı. <br/>Bu proxy'ler cihazlarının IP adresini gizlemek istediğiniz ve için kötü amaçlı kullanılan kişiler tarafından kullanılır. Bu algılama, kuruluştaki kullanıcılar tarafından yaygın olarak kullanılan hatalı etiketli IP adresleri gibi "yanlış pozitif sonuçları" azaltan bir makine öğrenimi algoritmasından yararlanır.|
|**Mümkün olmayan seyahat algılandı**|İki kullanıcı etkinliği (tek veya birden çok oturumdur), bir zaman dilimi içinde coğrafi olarak uzak konumlardan, kullanıcıdan ilk konumdan ikincisine giderken geçen süreden daha kısa bir süredir meydana gelir. Bu, farklı bir kullanıcının aynı kimlik bilgilerini kullandığını gösterir. <br/>Bu algılama, kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan VPN 'Ler ve konumlar gibi mümkün olmayan seyahat koşullarına katkıda bulunan bir makine öğrenimi algoritmasından yararlanır. Bu sırada yeni bir kullanıcının etkinlik deseninin öğrenir yedi günlük bir öğrenme dönemi algılama sahiptir.|

>[!NOTE]
> Yukarıdaki analizler Microsoft Cloud App Security (MCAS) tarafından desteklenir. Bu analizler açısından, etkinleştirilen bir MCAS lisansı gerekir. Bir MCAS lisansınız varsa, bu uyarılar varsayılan olarak etkinleştirilir. Devre dışı bırakmak için:
>
> 1. Güvenlik Merkezi dikey penceresinde **güvenlik ilkesi**' ni seçin. Değiştirmek istediğiniz abonelik için **Ayarları Düzenle**' ye tıklayın.
> 2. **Tehdit algılama**' ye tıklayın.
> 3. **Tümleştirmelere**izin ver ' in altında, **verilerinize erişim Microsoft Cloud App Security izin ver**onay kutusunu temizleyin ve **Kaydet**' e tıklayın

>[!NOTE]
>Azure Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynakla aynı coğrafi bölgede depolar. Microsoft, Azure Güvenlik Merkezi 'ni kaynağın coğrafi bölgede henüz dağıtmadıysa, verileri Birleşik Devletler depolar. Microsoft Cloud App Security (MCAS) etkinleştirildiğinde, bu bilgiler MCAS 'nin coğrafi konum kurallarına uygun olarak depolanır. [Daha fazla bilgi için bkz. bölgesel olmayan hizmetler Için veri depolama](https://azuredatacentermap.azurewebsites.net/).
