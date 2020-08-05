---
title: Azure Key Vault yenilikler | Microsoft Docs
description: Azure Key Vault için son güncelleştirmeler
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 6df9d5de88a3317c509279a7667c1a60eaf26a79
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563662"
---
# <a name="whats-new-for-azure-key-vault"></a>Azure Key Vault yenilikler

> [!WARNING]
> **2020 Temmuz**: Anahtar Kasası ' nın hizmetin uygulamalarını etkileme olasılığı olan iki güncelleştirmesi vardır: [Anahtar Kasası varsayılan olarak geçici SILME](#soft-delete-on-by-default) ve [Azure TLS sertifikası değişiklikleri](#azure-tls-certificate-changes). Ayrıntılar için aşağıya bakın.

Azure Key Vault yenilikleri aşağıda verilmiştir. [Azure updates Key Vault kanalında](https://azure.microsoft.com/updates/?category=security&query=Key%20vault)yeni özellikler ve geliştirmeler de duyurulur.

## <a name="soft-delete-on-by-default"></a>Varsayılan olarak geçici silme

2020 sonuna kadar, geçici silme, hem yeni hem de önceden mevcut olan **tüm anahtar kasaları için varsayılan olarak açık olacaktır**. Bu potansiyel değişiklik hakkında tam ayrıntılar ve etkilenen anahtar kasalarını bulma ve önceden güncelleştirme adımları için, [tüm anahtar kasalarında geçici silme](soft-delete-change.md)makalesine bakın. 

## <a name="azure-tls-certificate-changes"></a>Azure TLS sertifikası değişiklikleri  

Microsoft, Azure hizmetlerini farklı bir kök sertifika yetkilisi (CA) kümesinden TLS sertifikaları kullanacak şekilde güncelleştiriyor. Geçerli CA sertifikaları CA/tarayıcı Forum temel gereksinimlerinden biriyle uyumlu olmadığından bu değişiklik yapılıyor.

### <a name="when-will-this-change-happen"></a>Bu değişiklik ne zaman meydana gelir?

- Azure Active Directory (Azure AD) Hizmetleri, bu geçişe 7 Temmuz 2020 tarihinde başlamıştır.
- Yeni oluşturulan tüm Azure TLS/SSL uç noktaları, yeni kök CA 'Lara zincirleme olan güncelleştirilmiş sertifikaları içerir. 
- Mevcut Azure uç noktaları, 13 Ağustos 2020 ' den itibaren ve 26 Ekim 2020 ' ye kadar bir aşamalı olarak geçiş yapacaktır.

> [!IMPORTANT]
> Müşterilerin, Azure hizmetlerine bağlanmaya çalışırken bağlantı başarısızlıklarını engellemek için bu değişiklikten sonra uygulama (ler) i güncelleştirmeleri güncelleştirmesi gerekebilir. 

### <a name="what-is-changing"></a>Ne değişiyor?

Bugün, Azure hizmetleri zinciri tarafından aşağıdaki kök CA 'ya kadar kullanılan TLS sertifikalarının çoğu bugün:

| CA 'nın ortak adı | Parmak izi (SHA1) |
|--|--|
| [Baltimore Sitrust kökü](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |

Azure hizmetleri tarafından kullanılan TLS sertifikaları aşağıdaki kök CA 'Lardan birine zincirlenir:

| CA 'nın ortak adı | Parmak izi (SHA1) |
|--|--|
| [DigiCert genel kök G2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt) | df3c24f9bfd666761b268073fe06d1cc8d4f82a4 |
| [DigiCert genel kök CA 'sı](https://cacerts.digicert.com/DigiCertGlobalRootCA.crt) | a8985d3a65e5e5c4b2d7d66d40c6dd2fb19c5436 |
| [Baltimore Sitrust kökü](https://cacerts.digicert.com/BaltimoreCyberTrustRoot.crt) | d4de20d05e66fc53fe1a50882c78db2852cae474 |
| [D-TRUST kök sınıfı 3 CA 2 2009](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt) | 58e8abb0361533fb80f79b1b6d29d3ff8d5f00f0 |
| [Microsoft RSA kök sertifika yetkilisi 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20RSA%20Root%20Certificate%20Authority%202017.crt) | 73a5e64a3bff8316ff0edccc618a906e4eae4d74 | 
| [Microsoft EV ECC kök sertifika yetkilisi 2017](https://www.microsoft.com/pkiops/certs/Microsoft%20EV%20ECC%20Root%20Certificate%20Authority%202017.crt) | 6b1937abfd64e1e40daf2262a27857c015d6228d |

### <a name="when-can-i-retire-the-old-intermediate-thumbprint"></a>Eski ara parmak izini devre dışı bırakabilirim?

Geçerli CA sertifikaları 15 Şubat 2021 ' ye kadar *iptal edilmez.* Bu tarihten sonra, eski parmak izlerini kodınızdan kaldırabilirsiniz.

Bu tarih değişirse, yeni iptal tarihi hakkında bildirim alırsınız.

### <a name="will-this-affect-me"></a>Bu, beni etkileyecek mi?

**Azure müşterilerinin çoğu** etkilenmeyeceğini umuz.  Ancak, kabul edilebilir ca 'ların bir listesini açıkça belirtiyorsa uygulamanız etkilenebilir. Bu uygulama, sertifika sabitleme olarak bilinir.

Uygulamanızın etkilenip etkilendiğinin algılanması için bazı yollar şunlardır:

- Kaynak kodunuzda, bir Microsoft IT TLS CA 'sının parmak izi, ortak ad ve diğer CERT özellikleri için [burada](https://www.microsoft.com/pki/mscorp/cps/default.htm)bulunan bir arama yapın. Bir eşleşme varsa uygulamanız etkilenecektir. Bu sorunu çözmek için, kaynak kodu güncelleştirme yeni CA 'Ları içerir. En iyi uygulama olarak, CA 'Ların kısa bildirimde eklenebileceği veya düzenlenebildiğinden emin olun. Sektör düzenlemeleri, CA sertifikalarının 7 gün içinde değiştirilmesini gerektirir ve bu nedenle sabitleme kapsamındaki müşterilerin, hafif bir şekilde tepki sağlaması gerekir.

- Azure API 'Leri veya diğer Azure hizmetleriyle tümleşen bir uygulamanız varsa ve sertifika sabitleme kullanıyorsa emin değilseniz, uygulama satıcısına danışın.

- Azure hizmetleriyle iletişim kuran farklı işletim sistemleri ve dil çalışma zamanları, sertifika zincirini bu yeni köklerle doğru şekilde oluşturmak için ek adımlar gerektirebilir: 
    - **Linux**: birçok dağıtım, yukarıda listelenen CA 'ları/etc/SSL/certsa eklemenizi gerektirir. Belirli yönergeler için dağıtımın belgelerine bakın.
    - **Java**: Java anahtar deposunun yukarıda listelenen CA 'ları içerdiğinden emin olun.
    - **Bağlantısı kesilmiş ortamlarda çalışan Windows**: bağlantısı kesilmiş ortamlarda çalışan sistemlerin, yukarıda listelenen köklerin güvenilen kök sertifika yetkilileri deposuna eklenmesi ve Hammaddeleri ara sertifika yetkilileri deposuna eklenmiş olması gerekir.
    - **Android**: cihazınızın ve Android sürümünün belgelerini denetleyin.
    - **Diğer donanım aygıtları, özellikle IoT**: cihaz üreticisine başvurun. 

- Güvenlik Duvarı kurallarının yalnızca belirli sertifika Iptal listesi (CRL) indirme ve/veya çevrimiçi sertifika durumu Protokolü (OCSP) doğrulama konumlarına giden çağrılara izin verecek şekilde ayarlandığı bir ortamınız varsa. Aşağıdaki CRL ve OCSP URL 'Lerine izin vermeniz gerekir:

    - http://crl3&#46;d ıgicert&#46;com
    - http://crl4&#46;d ıgicert&#46;com
    - http://ocsp&#46;d ıgicert&#46;com
    - http://www&#46;d güveni&#46;net
    - http://root-c3-ca2-2009&#46; OCSP&#46;d-Trust&#46;net
    - http://crl&#46; Microsoft&#46;com
    - http://oneocsp&#46; Microsoft&#46;com
    - http://ocsp&#46; msocsp&#46;com

## <a name="june-2020"></a>Haziran 2020

Key Vault için Azure Izleyici artık önizlemededir.  Azure Izleyici, Key Vault isteklerinizin, performanlarınızın ve gecikmelerinden oluşan Birleşik bir görünüm sunarak anahtar kasalarınızın kapsamlı bir şekilde izlenmesini sağlar. Daha fazla bilgi için bkz [. Key Vault Için Azure izleyici (Önizleme).](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="may-2020"></a>Mayıs 2020

"Kendi anahtarını getir" (BYOK), artık genel kullanıma sunuldu. Key Vault [Azure Key Vault bYok belirtimine](../keys/byok-specification.md)BAKıN ve [HSM korumalı anahtarları Key Vault (bYok) öğesine nasıl içeri aktaracağınızı](../keys/hsm-protected-keys-byok.md)öğrenin.

## <a name="march-2020"></a>Mart 2020

Özel uç noktalar artık önizlemede kullanılabilir. Azure özel bağlantı hizmeti, sanal ağınızdaki özel bir uç nokta üzerinden Azure Key Vault ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine erişmenizi sağlar.  [Key Vault Azure özel bağlantısı Ile tümleştirmeyi](private-link-service.md)öğrenin.

## <a name="2019"></a>2019

- Yeni nesil Azure Key Vault SDK 'Ları sürümü. Kullanımları örnekleri için bkz. [Python](../secrets/quick-create-python.md), [.net](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md)ve [Node.js](../secrets/quick-create-node.md) için Azure Key Vault gizli hızlı başlangıç
- Anahtar Kasası sertifikalarını yönetmek için yeni Azure ilkeleri. [Key Vault Için Azure ilkesi yerleşik tanımlarına](../policy-samples.md)bakın.
- Azure Key Vault sanal makine uzantısı artık genel olarak kullanılabilir.  Bkz. Windows için [Linux için sanal makine uzantısı](../../virtual-machines/extensions/key-vault-linux.md) ve [Key Vault sanal makine uzantısı](../../virtual-machines/extensions/key-vault-windows.md)Key Vault.
- Azure Key Vault için olay odaklı gizli dizi yönetimi artık Azure Event Grid sunulmaktadır. Daha fazla bilgi için bkz. [Azure Key Vault olaylar için Event Grid şeması] (.. /.. /Event-Grid/Event-Schema-Key-Vault.exe) ve [Azure Event Grid ile Anahtar Kasası bildirimlerini nasıl alabileceğinizi ve yanıtlayacağınızı](event-grid-tutorial.md)öğrenin.

## <a name="2018"></a>2018

Bu yıldan yayınlanan yeni özellikler ve tümleştirmeler:

- Azure Işlevleri ile tümleştirme. Anahtar Kasası işlemlerine yönelik [Azure işlevleri](../../azure-functions/index.yml) ile ilgili örnek bir senaryo için bkz. [gizli dizi döndürmesini otomatikleştirme](../secrets/tutorial-rotation.md). 
- [Azure Databricks Ile tümleştirme](/azure/databricks/scenarios/store-secrets-azure-key-vault). Bu, Azure Databricks artık iki tür gizli kapsam desteklemektedir: Azure Key Vault-desteklenen ve Databricks-desteklenen. Daha fazla bilgi için bkz. [Azure Key Vault ile desteklenen gizli dizi kapsamı oluşturma](/azure/databricks/security/secrets/secret-scopes#--create-an-azure-key-vault-backed-secret-scope)
- [Azure Key Vault Için sanal ağ hizmeti uç noktaları](overview-vnet-service-endpoints.md).

## <a name="2016"></a>2016

Bu yıl yayımlanan yeni özellikler:

- Yönetilen depolama hesabı anahtarları. Depolama hesabı anahtarları özelliği, Azure depolama ile daha kolay tümleştirme ekledi. Daha fazla bilgi için bkz. genel bakış konusu, [yönetilen depolama hesabı anahtarlarına genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).
- Geçici silme. Geçici silme özelliği, Anahtar kasaları ve Anahtar Kasası nesnelerinizin veri korumasını geliştirir. Daha fazla bilgi için bkz. genel bakış konusu, [geçici silme genel bakış](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015"></a>2015

Bu yıl yayımlanan yeni özellikler:
- Sertifika yönetimi. 26 Eylül 2016 tarihinde GA sürüm 2015-06-01 ' e bir özellik olarak eklenmiştir.

Genel kullanılabilirlik (sürüm 2015-06-01), 24 Haziran 2015 tarihinde duyuruldu. Bu sürümde aşağıdaki değişiklikler yapılmıştır: 
- Anahtar "Use" alanını silme kaldırıldı.
- Anahtar "kullanım" alanı kaldırılmış şekilde bilgi alın.
- "Use" alanı kaldırılmış bir anahtarı içeri aktarın.
- Anahtar "kullanım" alanı kaldırılmış şekilde geri yükleyin.     
- RSA algoritmaları için "RSA_OAEP", "RSA-OAEP" olarak değiştirildi. [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-certificates.md)bilgi için bkz..    
 
İkinci önizleme sürümü (sürüm 2015-02-01-Önizleme), 20 Nisan 2015 ' de duyuruldu. Daha fazla bilgi için bkz. [REST API Update](https://docs.microsoft.com/archive/blogs/kv/rest-api-update) blog gönderisi. Aşağıdaki görevler güncelleştirildi:
 
- Anahtara kasaya eklenen ve işleme için bir sayfalama desteği olan anahtarları listeleyin.
- Anahtarın sürümlerini listelemek için anahtar ekleme işleminin sürümlerini listeleyin.  
- Kasalardan oluşan bir sayfalama desteğiyle gizli dizileri listeleyin.
- Gizli dizi sürümlerini listelemek için gizli-ekleme işleminin sürümlerini listeleyin.  
- Tüm işlemler-özniteliklere oluşturulan/güncellenen zaman damgaları eklendi.  
- Gizli dizileri için gizli bir Içerik türü oluşturun.
- İsteğe bağlı bilgiler olarak anahtar eklenmiş etiketler oluşturun.
- İsteğe bağlı bilgiler olarak gizli eklenen Etiketler oluşturun.
- Anahtar eklenmiş bir etiketi isteğe bağlı bilgiler olarak güncelleştirin.
- Gizli eklenen bir etiketi isteğe bağlı bilgiler olarak güncelleştirin.
- 10 K ile 25 K bayt arasındaki gizli dizileri için maksimum boyut değişti. [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-certificates.md)bilgi için bkz..    

## <a name="2014"></a>2014
 
İlk önizleme sürümü (sürüm 2014-12-08-önizleme), 8 Ocak 2015 tarihinde duyuruldu.  

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız varsa lütfen [destek](https://azure.microsoft.com/support/options/)aracılığıyla bizimle iletişim kurun.  
