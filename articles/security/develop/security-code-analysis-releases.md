---
title: Microsoft Güvenlik kodu analiz yayınları
description: Bu makalede Microsoft Güvenlik kodu analiz uzantısı için yaklaşan sürümler açıklanmaktadır
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 04/24/2020
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 33ce2a496caa52609d8bdf8c92e29064ca4ae349
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362049"
---
# <a name="microsoft-security-code-analysis-releases-and-roadmap"></a>Microsoft Güvenlik kodu analiz sürümleri ve yol haritası

Geliştirici Desteği ile iş ortaklığında Microsoft Güvenlik kodu analizi ekibi, MSCA uzantımızda son ve yaklaşan geliştirmeleri duyurmaktan gurur duyuyor.


## <a name="credential-scanner-v20-released-in-april-2020"></a>Kimlik bilgisi tarayıcısı v 2.0: Nisan 2020 ' de yayınlandı

### <a name="innovations--improvements"></a>Yeniliklere & Iyileştirmeleri

- **Çekirdek altyapısı**

   - Neredeyse doğrusal çalışma süreleriyle %25 oranında ortalama performans yükseltmesi
   - Daha fazla doğruluk için bağlam/kanıt tabanlı arama ve derecelendirme
   - Genel parola algılamaları ve açık yer tutucular için eşleşen mantık geliştirmeleri (örneğin, fakePassword)

- **Kapsam** -aşağıdaki en iyi istek dahil 25 + gizli türler için destek:

   - Fabric hesabı sertifika parolası
   - Gizli anahtar/API anahtarı
   - HTTP yetkilendirme üst bilgisi
   - Amazon S3 Istemci gizli dizi erişim anahtarı
   - Azure Active Directory Istemci erişim belirteci
   - Azure Işlevi ana/API anahtarı
   - Power BI erişim anahtarı
   - Azure Resource Manager şablonu parola stili

- **Çıkışlar**

   - SARIF 2,1 ve CSV dosyası çıkış dosyası biçimleri için destek

## <a name="binskim-v160-released-in-april-2020"></a>BinSkim v 1.6.0: Nisan 2020 ' de yayınlandı

### <a name="improvements"></a>Geliştirmeleri

- ÖZELLIK: son SARIF v2 (sürüm 2.1.16) sürümüne güncelleştirin. Bu güncelleştirme, birden çok tarama hedefi kopyası olan dizinleri yinelemeli olarak analiz edilirken önemli bir performans geliştirmesi olan komut satırındaki karmavaları geçirirken sonuçları önbelleğe almaya izin vermez.
- Hata düzelme: BA2021 'de yazım hatası 'ı onarın. DoNotMarkWritableSectionsAsExecutable çıkışı.
- PERFORMANS: Il kitaplığı (derlenmiş zaman derlenen) ikili dosyaları dahil olmak üzere yönetilen derlemeler için tüm karışık mod için PDB yüklemeyi kaldırın.
- YANLıŞ negatıf düzelme: bir ikilinin birlikte yerleştirildiği bir PDB 'nin, analiz altındaki ikiliye uyduğundan emin olun
- ÖZELLIK: ek (yerel, simge dışı sunucu) PDB arama konumları belirtmek için--Local-symbol-dizinlerinde bağımsız değişkenini sağlayın
- YANLıŞ pozıtıf onarım: oluşturulan .NET Core Native Bootstrap exe (Kullanıcı tarafından denetlenebilir kod olmayan) için PDB odaklı çözümlemeyi atlayın.

## <a name="whats-next-in-q3-cy20"></a>Q3 CY20 'in yanında ne var?

- Java güvenlik çözümleme aracı
- Python güvenlik çözümleme aracı
- TypeScript ve JavaScript için TS Lint 'i değiştirme
- Kaynak Yöneticisi şablonları çözümleme aracı

## <a name="tool-deprecation-notification"></a>Araç kullanımdan kaldırıldı bildirimi

### <a name="microsoft-security-risk-detection-msrd-is-deprecated-on-june-26-2020"></a>Microsoft güvenlik riski algılama (MSRD) Haziran 26 2020 ' de kullanımdan kaldırılmıştır.

Kullanım dışı bırakılmış olan MSRD belirsizlik hizmeti, Azure için açık kaynaklı, şirket içinde barındırılan bir geliştirici kendini geliştiren bir platform ile değiştirilmelidir. Bu platform şu anda Microsoft 'un temel ürün takımlarından birçoğu olan iş ortaklığında geliştirilmiştir ve test ediliyor. Bu platform, temizleme ve yazılım projeleriyle zaman içinde büyüyerek CI/CD işlem hatları içinde yerleşik olarak bulunan Uyarlamalı, öğrenme ve uyumlu bir test için izin verir. Bu platformun açık kaynak sürümü, 2020 ' nin son yarısında zamanlanır.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Güvenlik kodu analizini ekleme ve yükleme yönergeleri için [ekleme ve yükleme kılavuzumuza](security-code-analysis-onboard.md)bakın.

Uzantı ve sunulan araçlar hakkında daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.
