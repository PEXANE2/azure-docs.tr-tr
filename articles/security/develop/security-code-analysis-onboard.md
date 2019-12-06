---
title: Microsoft Güvenlik kodu analizi ekleme Kılavuzu
description: Bu makalede, Microsoft Güvenlik kodu çözümleme uzantısı 'nı yükleme açıklanmaktadır
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 108d116500454605f33de201caffc11ae263f74c
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851511"
---
# <a name="onboarding-and-installing"></a>Ekleme ve yükleme

Microsoft Güvenlik kodu çözümlemesi ile çalışmaya başlama önkoşulları:

- Aşağıdaki bölümde açıklandığı gibi uygun bir Microsoft Birleştirilmiş Destek teklif.
- Azure DevOps organizasyonu.
- Azure DevOps kuruluşuna uzantı yüklemek için izin.
- Bulutta barındırılan bir Azure DevOps işlem hattı ile eşitlenebilir kaynak kodu.

## <a name="onboarding-the-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik kodu çözümleme uzantısını ekleme

- Aşağıdaki destek tekliflerinden birine sahipseniz, uzantıya erişim sağlamak için mevcut saatleri satın almak veya değiştirmek üzere teknik hesap yöneticinize başvurun:
  - Birleşik destek Gelişmiş katmanı
  - Birleşik destek performans katmanı
  - Geliştiriciler için Premier Destek
  - Iş ortakları için Premier Destek
  - Enterprise için Premier Destek
- Aşağıdaki destek hizmetlerinden birine sahipseniz veya Microsoft Destek planına sahip değilseniz, uygun bir destek teklifine yükseltmeniz gerekir:
  - İş Ortakları İçin Azure Desteği
  - Azure Temel Desteği
  - Azure Geliştirici Desteği
  - Azure Standart Destek
  - Azure Profesyonel Doğrudan
  - Birleşik destek Core katmanı
- Uygun bir destek teklifi satın almak için [Destek Hizmetleri giriş sayfamıza](https://www.microsoft.com/enterprise/services/support)gidin.
- Bir destek sözleşmesi olduktan sonra, başlamanıza ve tüm gerekli ayrıntıları toplamamıza yardımcı olması için teknik hesap yöneticinize başvurun.

>[!NOTE]
>Destek sözleşmeniz yoksa, Birleşik destek planını satın almak zorunda kalmadan uzantıyı satın almak için 3. taraf iş ortaklarından faydalanabileceğiniz bir Iş ortağı satın alma programı üzerinde de çalışıyoruz. Lütfen [bize e-posta gönderin](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Onboarding%20Request). Bu seçenekle ilgilendiğinizi bize bildirmek için. İlginizi çekeceğiz ve bu seçenek kullanılabilir duruma geldiğinde size geri dönebilirsiniz.

## <a name="installing-the-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik kodu analizi uzantısını yükleme

1. Uzantı Azure DevOps kuruluşunuzla paylaşıldıktan sonra Azure DevOps kuruluş sayfanıza gidin. Böyle bir sayfanın örnek URL 'SI `https://dev.azure.com/contoso`.
1. Adınızın yanındaki sağ üst köşede bulunan alışveriş çantası simgesini seçin ve ardından **Uzantıları Yönet**' i seçin.
1. **Paylaşılan**' ı seçin.
1. Microsoft Güvenlik kodu analiz uzantısı ' nı seçin, ardından **Install**' ı seçin.
1. Açılan listeden, uzantıyı yüklemek için Azure DevOps organizasyonunu seçin.
1. **Yükle**’yi seçin. Yükleme tamamlandıktan sonra, uzantısını kullanmaya başlayabilirsiniz.

>[!NOTE]
> Uzantıyı yükleme erişiminiz olmasa bile, yükleme adımlarıyla devam edin. Yükleme işlemi sırasında Azure DevOps kuruluş yöneticinizden erişim isteğinde bulabilirsiniz.

Uzantıyı yükledikten sonra, güvenli geliştirme derleme görevleri görünür ve Azure Pipelines eklemek için kullanılabilir.

## <a name="adding-specific-build-tasks-to-your-azure-devops-pipeline"></a>Azure DevOps işlem hattınızla belirli derleme görevleri ekleme

1. Azure DevOps kuruluşunuzda, takım projenizi açın.
1. **Derlemeler** > işlem **hatlarını** seçin.
1. Uzantı derleme görevlerini eklemek istediğiniz işlem hattını seçin:
   - Yeni işlem hattı: **Yeni** ' yi seçin ve yeni bir işlem hattı oluşturmak için ayrıntılı adımları izleyin.
   - İşlem hattını Düzenle: mevcut bir işlem hattını seçin ve ardından işlem hattını düzenlemeye başlamak için **Düzenle** ' yi seçin.
1. **+** ' yi seçin ve **Görevler Ekle** bölmesine gidin.
1. Listeden ya da arama kutusunu kullanarak eklemek istediğiniz derleme görevini bulun. **Add (Ekle)** seçeneğini belirleyin.
1. Görev için gereken parametreleri belirtin.
1. Yeni bir yapıyı sıraya alın.
   >[!NOTE]
   >Dosya ve klasör yolları, kaynak deponuzdaki köke göre değişir. Çıkış dosyalarını ve klasörlerini parametre olarak belirtirseniz, bunlar derleme aracısında tanımlamış olduğumuz ortak konumla değiştirilmiştir.

> [!TIP]
>
> - Derlemeden sonra bir analiz çalıştırmak için, derlemenize ait derleme yapıtlarını Yayımla adımını tamamladıktan sonra Microsoft Güvenlik kodu çözümleme derleme görevlerini koyun. Bu şekilde, derlemeniz statik analiz araçlarını çalıştırmadan önce, sonuçları bitirebilirler ve gönderebilir.
> - Güvenli Geliştirme derleme görevleri için her zaman **hata durumunda devam et '** i seçin. Bir araç başarısız olsa da, diğerleri çalıştırılabilir. Araçlar arasında bağımlılık yoktur.
> - Microsoft Güvenlik kodu Analizi derleme görevleri yalnızca bir araç başarıyla çalışamazsa başarısız olur. Ancak bir araç, koddaki sorunları tanımlarsa bile başarılı olur. Analiz sonrası derleme görevini kullanarak, bir araç koddaki sorunları belirlediğinde, derlemenizi başarısız olarak yapılandırabilirsiniz.
> - Bir yayın işlem hattı aracılığıyla çalıştırıldığında bazı Azure DevOps derleme görevleri desteklenmez. Daha belirgin olarak, Azure DevOps bir yayın işlem hattı içinden yapıtları yayınlayan görevleri desteklemez.
> - Azure DevOps ekip derlemesinde parametre olarak belirtebileceğiniz önceden tanımlanmış değişkenlerin bir listesi için bkz. [Azure DevOps derleme değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts).

## <a name="next-steps"></a>Sonraki adımlar

Yapı görevlerini yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma kılavuzumuzu](security-code-analysis-customize.md) veya [YAML yapılandırma kılavuzu](yaml-configuration.md).

Uzantı ve sunulan araçlar hakkında daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.
