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
ms.openlocfilehash: d4281d3b6132e551283a71cd1801ef462fbfc68c
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146135"
---
# <a name="microsoft-security-code-analysis-releases-and-roadmap"></a>Microsoft Güvenlik kodu analiz sürümleri ve yol haritası

Geliştirici Desteği ile iş ortaklığında Microsoft Güvenlik kodu analizi ekibi, MSCA uzantımızda son ve yaklaşan geliştirmeleri duyurmaktan gurur duyuyor. Lütfen aşağıdaki yol haritasını inceleyin.

![Yayınlar](./media/security-code-analysis-releases/releases.png)

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

- ÖZELLIK: son SARIF v2 (sürüm 2.1.16) sürümüne güncelleştirin. Bu, birden çok tarama hedefi kopyası olan dizinleri yinelemeli olarak analiz edilirken önemli bir performans geliştirmesi olan komut satırında geçiş yaparken sonuçların önbelleğe alınmasına izin vermez.
- Hata düzelme: BA2021 'de yazım hatası 'ı onarın. DoNotMarkWritableSectionsAsExecutable çıkışı.
- PERFORMANS: Il kitaplığı (derlenmiş zaman derlenen) ikili dosyaları dahil olmak üzere yönetilen derlemeler için tüm karışık mod için PDB yüklemeyi kaldırın.
- YANLıŞ negatıf düzelme: bir ikilinin birlikte yerleştirildiği bir PDB 'nin, analiz altındaki ikiliye uyduğundan emin olun
- ÖZELLIK: ek (yerel, simge dışı sunucu) PDB arama konumları belirtmek için--Local-symbol-dizinlerinde bağımsız değişkenini sağlayın
- YANLıŞ pozıtıf onarım: oluşturulan .NET Core Native Bootstrap exe (Kullanıcı tarafından denetlenebilir kod olmayan) için PDB odaklı çözümlemeyi atlayın.

## <a name="whats-next-in-fy20"></a>FY20 ' deki yenilikler nelerdir?

- Java güvenlik çözümleme aracı
- Python güvenlik çözümleme aracı
- TypeScript ve JavaScript için TS Lint 'i değiştirme

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Güvenlik kodu analizini ekleme ve yükleme yönergeleri için [ekleme ve yükleme kılavuzumuza](security-code-analysis-onboard.md)bakın.

Uzantı ve sunulan araçlar hakkında daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.
