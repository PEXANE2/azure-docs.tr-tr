---
title: Microsoft Güvenlik Kodu Çözümlemesi bültenleri
description: Bu makalede, Microsoft Güvenlik Kodu Çözümlemesi uzantısı için gelecek sürümler açıklanmaktadır
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 04/14/2020
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: a07ce7437d664baca0cfdc310dbc2631f41fdbcc
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81462041"
---
# <a name="microsoft-security-code-analysis-releases-and-roadmap"></a>Microsoft Güvenlik Kodu Analizi sürümleri ve yol haritası

Geliştirici Desteği ile işbirliği içinde Microsoft Güvenlik Kodu Analizi ekibi, MSCA uzantımıza en son ve gelecek geliştirmeleri duyurmaktan gurur duyar. Lütfen aşağıdaki Yol Haritası'na bakın.

![Yayınlar](./media/security-code-analysis-releases/releases.png)

## <a name="credential-scanner-v20-released-on-april-1-2020"></a>Kimlik Bilgisi Tarayıcı v2.0: 1 Nisan 2020 tarihinde yayınlandı

### <a name="innovations--improvements"></a>Yenilikler & Geliştirmeler

- **Çekirdek Motoru**

   - Yakın doğrusal çalışma süreleri ile %25'lik ortalama performans yükseltmesi
   - Daha fazla doğruluk için bağlam/kanıt ala¤› arama ve sıralama
   - Genel parola algılamalarında iyileştirmeler ve bariz yer tutucular için eşleşen mantık (örneğin, fakePassword)

- **Kapsam** - Aşağıdaki üst talep dahil olmak üzere 25 + gizli türleri için destek:

   - Kumaş hesap sertifikası Passphrase
   - İstemci Secret/API Anahtarı
   - HTTP yetkilendirme üstbilgi
   - Amazon S3 İstemci Gizli Erişim Anahtarı
   - Azure Active Directory İstemci Erişim Belirteci
   - Azure İşlev Yöneticisi/API Anahtarı
   - Güç BI Erişim Anahtarı
   - Azure Kaynak Yöneticisi şablon uyşu

- **Çıkışlar**

   - SARIF 2.1 ve CSV dosya çıktısı dosya biçimleri desteği

## <a name="binskim-v160-to-be-released-on-april-2020"></a>BinSkim v1.6.0: Nisan 2020'de piyasaya sürülecek

### <a name="improvements"></a>Gelişmeler

- ÖZELLİk: Son SARIF v2 (sürüm 2.1.16) güncelleştirme. Bu, komut satırında -iş haşlamaları, scan hedeflerinin birden çok kopyasıyla dizinleri özyinelemeyle çözümlediğinde önemli bir performans artışı olan - komut satırında geçerken sonuçlar önbelleğe alma sağlar.
- HATA DÜZELTME: BA2021 yılında yazım hatası düzeltme. DoNotMarkWritableSectionsAsExecutable çıktı.
- PERFORMANS: IL Kitaplığı (önceden derlenmiş) ikilileri de dahil olmak üzere yönetilen derlemeler için karışık olmayan mod lar için PDB yüklemesini ortadan kaldırın.
- YANLIŞ NEGATIF DÜZELTME: Bir ikilinin yanına yerleştirilen bir PDB'nin analiz altında ikiliyle eşleştiğini doğrulayın
- ÖZELLİk: Ek (yerel, sembol olmayan sunucu) PDB arama konumlarını belirtmek için --yerel-sembol-dizinler bağımsız değişkeni sağlayın
- YANLIŞ POZITIF DÜZELTME: Oluşturulan .NET çekirdekli bootstrap exe (kullanıcı tarafından kontrol edilebilir kod değildir) için PDB odaklı analizi atlayın.

## <a name="whats-next-in-fy20"></a>FY20'de sırada ne var?

- Java Güvenlik Analizi aracı
- Python Güvenlik Analizi aracı
- ES Lint TypeScript ve JavaScript için TS Lint yerine

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Güvenlik Kodu Analizi'nin gemiye nasıl yüklenir ve yüklenir, ilgili talimatları için [Onboarding ve yükleme kılavuzumuza](security-code-analysis-onboard.md)bakın.

Uzantı ve sunulan araçlar hakkında daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.
