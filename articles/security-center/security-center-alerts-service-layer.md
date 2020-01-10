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
ms.openlocfilehash: d4033989830323856ac14ed06eea7df74806f128
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665703"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Azure hizmet katmanı için tehdit algılama

Bu konu, aşağıdaki Azure hizmeti katmanlarını izlerken kullanılabilen Azure Güvenlik Merkezi uyarılarını gösterir:

* [Azure ağ katmanı](#network-layer)
* [Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)](#management-layer)
* [Azure Anahtar Kasası.](#azure-keyvault)

## Azure ağ katmanı<a name="network-layer"></a>

Güvenlik Merkezi ağ katmanı analizi, Azure çekirdek yönlendiricileri tarafından toplanan paket üstbilgileri olan örnek [ıpdüzeltme verilerini](https://en.wikipedia.org/wiki/IP_Flow_Information_Export)temel alır. Güvenlik Merkezi, bu veri akışına bağlı olarak kötü amaçlı trafik etkinliklerini belirlemek ve işaretlemek için makine öğrenimi modellerini kullanır. Ayrıca, Güvenlik Merkezi, IP adreslerini zenginleştirmek için Microsoft Threat Intelligence veritabanını kullanır.

Bazı ağ yapılandırmalarında, güvenlik merkezi 'nin şüpheli ağ etkinliğinde Uyarı oluşturması kısıtlanabilir. Güvenlik Merkezi 'nin ağ uyarıları oluşturması için şunları doğrulayın:

- Sanal makineniz genel bir IP adresine sahiptir (veya genel IP adresine sahip bir yük dengeleyicide bulunur).

- Sanal makinenizin ağ çıkış trafiği bir dış KIMLIK çözümü tarafından engellenmiyor.

- Sanal makinenize, şüpheli iletişimin gerçekleştiği saatin tamamında aynı IP adresi atandı. Bu, yönetilen bir hizmetin parçası olarak oluşturulan VM 'Ler için de geçerlidir (örneğin, AKS, Databricks).

Azure ağ katmanı uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurenetlayer)bakın.

Güvenlik Merkezi 'nin tehdit koruması uygulamak için ağla ilgili sinyalleri nasıl kullanabileceği hakkında ayrıntılı bilgi için bkz. [Güvenlik Merkezi 'Nde BULUŞSAL DNS algılamaları](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/).


## Azure Yönetim Katmanı (Azure Resource Manager) (Önizleme)<a name ="management-layer"></a>

Güvenlik Merkezi 'nin Azure Resource Manager tabanlı koruma katmanı Şu anda önizleme aşamasındadır.

Güvenlik Merkezi, Azure için denetim düzlemi olarak kabul edilen Azure Resource Manager olaylarını kullanarak ek bir koruma katmanı sunar. Güvenlik Merkezi, Azure Resource Manager kayıtlarını analiz ederek Azure abonelik ortamında olağandışı veya potansiyel olarak zararlı işlemleri algılar.

Azure Resource Manager (Önizleme) uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureresourceman)bakın.



>[!NOTE]
> Yukarıdaki analizler Microsoft Cloud App Security tarafından desteklenmektedir. Bu analizlerden yararlanmak için bir Cloud App Security lisansını etkinleştirmeniz gerekir. Cloud App Security lisansınız varsa, bu uyarılar varsayılan olarak etkinleştirilir. Devre dışı bırakmak için:
>
> 1. **Güvenlik Merkezi** dikey penceresinde **güvenlik ilkesi**' ni seçin. Değiştirmek istediğiniz abonelik için **Ayarları Düzenle**' yi seçin.
> 2. **Tehdit algılamayı**seçin.
> 3. **Tümleştirmelere**izin ver ' in altında, **verilerinize erişim Microsoft Cloud App Security izin ver**' i temizleyin ve **Kaydet**' i seçin

>[!NOTE]
>Güvenlik Merkezi, güvenlikle ilgili müşteri verilerini kaynakla aynı coğrafi bölgede depolar. Microsoft, kaynak coğrafi bölgede henüz Güvenlik Merkezi dağıtmamışsa, verileri Birleşik Devletler depolar. Cloud App Security etkinleştirildiğinde, bu bilgiler Cloud App Security coğrafi konum kurallarına uygun olarak depolanır. Daha fazla bilgi için bkz. [bölgesel olmayan hizmetler Için veri depolama](https://azuredatacentermap.azurewebsites.net/).

## Azure Key Vault (Önizleme)<a name="azure-keyvault"></a>

Azure Key Vault, şifreleme anahtarlarını ve sertifikalar, bağlantı dizeleri ve parolalar gibi gizli dizileri koruyan bir bulut hizmetidir. 

Azure Güvenlik Merkezi, ek bir güvenlik zekası katmanı sağlayan Azure Key Vault için Azure yerel, Gelişmiş tehdit koruması içerir. Güvenlik Merkezi, Key Vault hesaplarına erişme veya açıktan yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri algılar. Bu koruma katmanı, güvenlik uzmanı olmadan ve üçüncü taraf güvenlik izleme sistemlerini yönetmeye gerek kalmadan tehditleri ele almanıza olanak sağlar.  

Anormal etkinlikler gerçekleştiğinde, güvenlik merkezi uyarıları gösterir ve isteğe bağlı olarak bunları abonelik yöneticilerine e-posta ile gönderir. Bu uyarılar, şüpheli etkinliğin ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir. 

> [!NOTE]
> Bu hizmet şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Azure Key Vault uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azurekv)bakın.
