---
title: Microsoft Güvenlik Kodu Analizi onboarding kılavuzu
description: Bu makalede, Microsoft Güvenlik Kodu Çözümlemesi uzantısı nın yüklenmesi açıklanmaktadır
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
ms.openlocfilehash: 3ef111817b6351277f975b9b7e454f9a89982451
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460214"
---
# <a name="onboarding-and-installing"></a>Onboarding ve yükleme

Microsoft Güvenlik Kodu Analizi'ne başlamak için ön koşullar:

- Aşağıdaki bölümde ayrıntılı olarak belirtildiği gibi uygun bir Microsoft Birleşik Destek teklifi.
- Azure DevOps kuruluşu.
- Azure DevOps kuruluşuna uzantıyükleme izni.
- Bulut tarafından barındırılan Azure DevOps ardışık bir düzenek ile senkronize edilebilen kaynak kodu.

## <a name="onboarding-the-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik Kodu Çözümlemesi uzantısıonboarding

### <a name="interested-in-purchasing-the-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik Kodu Analizi uzantısını satın almak ister misiniz?

Aşağıdaki destek tekliflerinden birine sahipseniz, uzantıya erişmek için mevcut saatleri satın almak veya değiştirmek için Teknik Hesap Yöneticinize başvurun:

- Birleşik Destek Gelişmiş katmanı
- Birleşik Destek Performansı katmanı
- Geliştiriciler için Premier Destek
- Ortaklar için Premier Destek
- Kurumsal için Premier Destek

Yukarıda belirtilen destek anlaşmalarından birine sahip değilseniz, uzantıyı Ortaklarımızdan birinden satın alabilirsiniz.

**Sonraki Adımlar:**

Aşağıdaki listeden bir İş Ortağına başvurun ve Microsoft Güvenlik Kodu Çözümlemesi uzantısını satın almak isteyin.

>**Ortak:**

- Bölgeler - İletişim Bilgileri:cloudsupport@zones.com
- Wortell – İletişim Bilgileri:info@wortell.nl

### <a name="become-a-partner"></a>Ortak Olun

Microsoft Güvenlik Kodu Çözümlemesi ekibi, iş ortaklarının iş ortakları için Bir Premier Destek anlaşması na sahip olmasını istiyor. İş ortakları, uzantıyı satın almak isteyen ancak Microsoft ile Kurumsal Destek sözleşmesi olmayan müşterilere satarak Azure DevOps müşterilerinin daha güvenli bir şekilde geliştirmesini sağlamaya yardımcı olur. İlgilenen ortaklar [buradan](http://www.microsoftpartnersupport.com/msrd/opin)kayıt yaptırabilirsiniz.

## <a name="installing-the-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik Kodu Çözümlemesi uzantısını yükleme

1. Uzantı Azure DevOps kuruluşunuzla paylaşıldıktan sonra Azure DevOps kuruluş sayfanıza gidin. Böyle bir sayfa için `https://dev.azure.com/contoso`örnek bir URL' dir.
1. Adınızın yanındaki sağ üst köşedeki alışveriş çantası simgesini seçin ve **ardından uzantıları yönet'i**seçin.
1. **Paylaşılan'u**seçin.
1. Microsoft Güvenlik Kodu Analizi uzantısını seçin, **yükle'yi**seçin.
1. Açılan listeden, uzantıyı yüklemek için Azure DevOps kuruluşunu seçin.
1. **Yükle**’yi seçin. Yükleme tamamlandıktan sonra uzantıyı kullanmaya başlayabilirsiniz.

>[!NOTE]
> Uzantıyı yüklemek için erişiminiz olmasa bile, yükleme adımlarını devam edin. Yükleme işlemi sırasında Azure DevOps kuruluş yöneticinizden erişim isteğinde bulunabilirsiniz.

Uzantıyı yükledikten sonra, güvenli geliştirme oluşturma görevleri görünür ve Azure Ardışık Hatlarınıza eklenecek şekilde kullanılabilir.

## <a name="adding-specific-build-tasks-to-your-azure-devops-pipeline"></a>Azure DevOps ardışık sisteminize belirli yapı görevleri ekleme

1. Azure DevOps kuruluşunuzdan ekip projenizi açın.
1. Boru Hatları > **Oluşturur'ı**seçin. **Pipelines**
1. Uzantı oluşturma görevlerini eklemek istediğiniz ardışık yapı yı seçin:
   - Yeni ardışık kaynak: **Yeni'yi** seçin ve yeni bir ardışık kaynak oluşturmak için ayrıntılı adımları izleyin.
   - Ardışık düzenle: Varolan bir **Edit** ardışık düzenle ve ardışık hattı düzenlemeye başlamak için Düzenle'yi seçin.
1. Görevler **+** **Ekle** bölmesini seçin ve gidin.
1. Listeden veya arama kutusunu kullanarak eklemek istediğiniz yapı görevini bulun. **Add (Ekle)** seçeneğini belirleyin.
1. Görev için gereken parametreleri belirtin.
1. Yeni bir yapıyı sıraya n için sıraya girin.
   >[!NOTE]
   >Dosya ve klasör yolları kaynak deponuzun köküne göredir. Çıktı dosyalarını ve klasörlerini parametre olarak belirtirseniz, bunlar yapı aracısı üzerinde tanımladığımız ortak konumla değiştirilir.

> [!TIP]
>
> - Yapınızdan sonra bir çözümleme çalıştırmak için, yapınızın Yapı Yapı Larını Yayımla adımından sonra Microsoft Güvenlik Kodu Çözümlemesi yapı görevlerini yerleştirin. Bu şekilde, yapınız statik çözümleme araçlarını çalıştırmadan önce sonuçları bitirebilir ve yayınlayabilir.
> - Güvenli geliştirme oluşturma görevleri için her zaman **Hataya Devam** Et'i seçin. Bir araç başarısız olsa bile, diğerleri çalıştırabilirsiniz. Araçlar arasında karşılıklı bağımlılık yoktur.
> - Microsoft Güvenlik Kodu Çözümlemesi yapı görevleri yalnızca bir araç başarılı bir şekilde çalışmazsa başarısız olur. Ancak, bir araç koddaki sorunları tanımlasa bile başarılı olurlar. Analiz Sonrası yapı görevini kullanarak, bir araç koddaki sorunları tanımladığında yapınızı başarısız olacak şekilde yapılandırabilirsiniz.
> - Bazı Azure DevOps yapı görevleri, bir sürüm ardışık hattı üzerinden çalıştırıldığında desteklenmez. Daha ayrıntılı olarak, Azure DevOps'ler bir sürüm ardışık alanı içinde yapı yayımlayan görevleri desteklemez.
> - Azure DevOps Takım Yapısı'nda parametre olarak belirtebileceğiniz önceden tanımlanmış [değişkenlerin](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts)listesi için bkz.

## <a name="next-steps"></a>Sonraki adımlar

Yapı görevlerini yapılandırma hakkında daha fazla bilgi için [Yapılandırma kılavuzumuza](security-code-analysis-customize.md) veya [YAML Yapılandırma kılavuzumuza](yaml-configuration.md)bakın.

Uzantı ve sunulan araçlar hakkında daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.
