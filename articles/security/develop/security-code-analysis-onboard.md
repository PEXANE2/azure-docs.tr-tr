---
title: Microsoft Azure Güvenlik kodu analizi ekleme Kılavuzu
description: Bu makale, güvenlik kodu analiz uzantısı 'nı yükleme hakkında
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
ms.openlocfilehash: 8a8eca73205d4f8f33d4d069fda72638af61d355
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718307"
---
# <a name="how-to-onboarding-and-installing"></a>Nasıl Yapılır: Ekleme ve yükleme

Microsoft Güvenlik kodu analizini kullanmaya başlama önkoşulları
  - Uygun Microsoft Birleştirilmiş Destek Hizmetleri teklifi (Ayrıntılar aşağıda verilmiştir)
  - Azure DevOps organizasyonu
  - Azure DevOps kuruluşuna uzantı yüklemek için izinler
  - Bulutta barındırılan bir Azure DevOps işlem hattı ile eşitlenebilir kaynak kodu


## <a name="onboarding-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik kodu analizi uzantısını ekleme

- Aşağıdaki destek tekliflerinden birine sahipseniz, bu uzantıya erişim sağlamak için teknik hesap yöneticinize başvurmanız ve mevcut saatlere purchase\swap.
   - Birleşik destek – gelişmiş ve performans katmanı, geliştiriciler için Premier Destek, Iş ortakları için Premier Destek veya kuruluş için Premier Destek.
- Aşağıdaki destek hizmetlerinden birine sahipseniz veya Microsoft ile herhangi bir destek planı yoksa, uygun bir destek teklifine yükseltmeniz gerekir:
   - Iş ortakları, Azure temel, Azure geliştiricisi, Azure Standard, Azure Profesyonel Doğrudan veya Birleşik destek için Azure desteği – çekirdek katman
- Uygun bir destek teklifi satın almak için lütfen [Destek Hizmetleri giriş sayfamızı](https://www.microsoft.com/enterprise/services/support) ziyaret edin
- Bir destek sözleşmesi olduktan sonra, başlamanıza yardımcı olabilecek teknik hesap yöneticinize başvurun ve gerekli tüm ayrıntıları toplayın.
 
>[!NOTE]
> Yalnızca Microsoft Iş ortakları ağı 'ndaki kayıtlı iş ortakları Iş ortakları için Premier Destek satın almaya uygundur, aksi takdirde daha önce bahsedilen uygun destek tekliflerinden birini satın alın

## <a name="installing-microsoft-security-code-analysis-extension"></a>Microsoft Güvenlik kodu analiz uzantısı yükleniyor

1. Uzantı Azure DevOps kuruluşunuzla paylaşıldıktan sonra, Azure DevOps kuruluş sayfanıza gidin (örneğin http://dev.azure.com/contoso)
2. Adınızın yanındaki sağ üst köşedeki alışveriş çantası simgesine tıklayın ve ardından Uzantıları Yönet ' e tıklayın. 
3. Microsoft Güvenlik kodu analiz uzantısı ' na tıklayın ve yüklemeyi başlatmak için Azure DevOps Kullanıcı Arabirimi Sihirbazı 'nı başlatın.
4. Uzantıyı açılan listeden yüklemek için Azure DevOps organizasyonunu seçin
5. Install 'a tıklayın. Tamamlandıktan sonra, uzantıyı kullanmaya devam edebilirsiniz

>[!NOTE]
> Erişiminiz olmasa bile, yüklemeye devam edin ve işlem sırasında Azure DevOps kuruluş yöneticinizden erişim isteyebileceksiniz.
>
Uzantı yüklendikten sonra, güvenli geliştirme derleme görevleri görünür olur ve Azure Pipelines eklenebilir.

## <a name="adding-specific-build-tasks-to-your-devops-pipeline"></a>DevOps ardışık düzenine belirli derleme görevleri ekleme

1. Azure DevOps kuruluşunuzdan takım projenizi açın.
2. İşlem **hatları** altındaki **derlemeler** sekmesine gidin 
3. Uzantı derleme görevlerini eklemek istediğiniz işlem hattını seçin. 
   - Yeni **' ye** tıklayın ve yeni bir işlem hattı oluşturmak için ayrıntılı adımları izleyin.
   - Düzenle-işlem hattını seçin ve var olan bir işlem hattını düzenlemeye başlamak için **Düzenle** ' ye tıklayın.
4. **Görevler Ekle** bölmesine gitmek için + ' ye tıklayın.
5. Listeden ya da arama kutusunu kullanarak eklemek istediğiniz derleme görevini bulun ve ardından **Ekle**' ye tıklayın. 
6. Artık görev için gereken parametreleri belirtmeye devam edebilirsiniz.
>[!NOTE]
>Dosya veya dizin yolları, kaynak havuzlarınızın köküne göredir ve çıkış klasörünü/dosyalarını belirten parametreler, derleme aracısında tanımlamış olduğumuz ortak konumla birlikte değişir.

7. Yeni bir derlemeyi kuyruğa al.
> [!TIP]
>  - Derlemeden sonra Analizi çalıştırmak için, derlemenize ait derleme yapıtlarını Yayımla adımından sonra Microsoft Güvenlik kodu çözümleme derleme görevlerini koyun. Bu şekilde, derlemeniz statik analiz araçlarını çalıştırmadan önce, sonuçları bitirebilirler ve gönderebilir.
>  - Güvenli Geliştirme derlemesi görevlerinin her zaman **' hatada devam et '** seçeneğini işaretleyin. Araçlardan biri başarısız olsa bile, diğerleri çalışabilir. Bağımlılıkları yoktur.
>  - Microsoft Güvenlik kodu Analizi derleme görevleri yalnızca araç başarıyla çalışamazsa başarısız olur, ancak araç koddaki sorunları tanımlarsa başarısız olmaz. Bir araç, **Analiz sonrası** derleme görevini kullanarak koddaki sorunları belirlediğinde, derlemenizi bölmek için yapılandırabilirsiniz.
>  - Bazı Azure DevOps derleme görevleri bir "Release" işlem hattı aracılığıyla çalıştırıldığında desteklenmez. Bu bir Azure DevOps kısıtlamasıdır. Yayın işlem hattı içinden yapıları yayınlayan görevleri desteklemezler.
>  - Azure DevOps ekip derlemesinde önceden tanımlanmış değişkenlerin bir listesi için, parametre olarak belirtebileceğiniz için bkz. [Azure DevOps derleme değişkenleri](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts)

## <a name="next-steps"></a>Sonraki adımlar

Yapı görevlerini yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma](security-code-analysis-customize.md) kılavuzumuzu

Uzantı ve sunulan araçlar hakkında başka sorularınız varsa, [SSS sayfamızı inceleyin.](security-code-analysis-faq.md)


