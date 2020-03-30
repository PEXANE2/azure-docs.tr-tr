---
title: Microsoft Tehdit Modelleme Aracı sürümü 09.12.2018
titleSuffix: Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 01/15/2019
ms.openlocfilehash: a921310f14f0d48922165a46d750366e170ee374
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78269908"
---
# <a name="threat-modeling-tool-ga-release-71509112---9122018"></a>Tehdit Modelleme Aracı GA sürümü 7.1.50911.2 - 9/12/2018

Microsoft Tehdit Modelleme Aracı'nın artık desteklenen genel olarak kullanılabilir (GA) sürümü olarak indirilebilen bir sürüm olduğunu duyurmaktan heyecan duyuyoruz. Bu sürüm, önemli gizlilik ve güvenlik güncelleştirmelerinin yanı sıra hata düzeltmeleri, özellik güncelleştirmeleri ve kararlılık iyileştirmeleri içerir. 2017 Preview sürümünün mevcut kullanıcıları, istemciyi açtıktan sonra ClickOnce teknolojisi aracılığıyla en son sürümde güncellenmeleri istenecektir. Aracın yeni kullanıcıları için [istemciyi indirebilirsiniz.](https://aka.ms/threatmodelingtool)

Bu sürümle, 2017 Önizleme desteğini sona erdiriyoruz ve Önizleme güncelleştirmesinin tüm kullanıcılarını GA sürümüne tavsiye ediyoruz. 15 Ekim 2018 tarihinde veya sonrasında, Tehdit Modelleme Aracı için gerekli en az ClickOnce sürümünü ayarlayacaktır ve tüm Önizleme istemcilerinin yükseltmesi gerekecektir.

[Microsoft Download Center'dan](https://www.microsoft.com/en-us/download/details.aspx?id=49168)kullanılabilen Microsoft Tehdit Modelleme Aracı 2016, yalnızca kritik güvenlik düzeltmeleri için 1 Ekim 2019'a kadar desteklenmeye devam etmektedir.

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="azure-stencil-updates"></a>Azure şablon güncellemeleri

Bu sürümle birlikte şablon seti gönderimine ek Azure şablonları ve bunlarla ilişkili tehditler ve azaltıcı etkenler eklendi. "Azure Uygulama Hizmetleri", "Azure Veritabanı Teklifleri" ve "Azure Depolama" odak alanlarında önemli değişiklikler yapıldı.

![Azure Şablon Güncellemeleri](./media/threat-modeling-tool-releases-71509112/tmt_azure_stencil_update-300x70.png)

### <a name="onedrive-integration-feature-removed"></a>OneDrive tümleştirme özelliği kaldırıldı

Önizlemenin "OneDrive'a Kaydet", "OneDrive'dan Aç" ve "Bağlantıyı Paylaş" özellikleri kaldırıldı. OneDrive kullanıcıları, standart dosya kaydetme ve açık iletişim günlükleri aracılığıyla OneDrive'da depolanan dosyalarına erişmek için Microsoft'un Windows için [OneDrive](https://onedrive.live.com/about/en-us/download/) istemcisini kullanmaları için teşvik edilir.

## <a name="notable-fixed-bugs-reported-by-customers"></a>Müşteriler tarafından bildirilen önemli sabit hatalar

### <a name="in-tmt-preview-the-tool-crashes-when-using-the-standard-template"></a>TMT Preview'da, araç standart şablonu kullanırken çöker

- Genel şablon (örneğin "Genel Veri Akışı") çizim yüzeyine eklendiğinde ve tehditler oluşturduğunda, araç kilitlenebilir. Bu sorun düzeltilmiştir.

### <a name="in-tmt-preview-when-i-save-a-report-or-copy-the-threats-the-risk-levels-are-incorrect"></a>TMT Preview'da, bir raporu kaydettiğimde veya tehditleri kopyaladığımda, risk düzeyleri yanlış

- Bir kullanıcı belirli tehditlerin Risk düzeyini değiştirir ve ardından bir rapor kaydeder veya riskleri kopyalarsa, risk düzeyi "Yüksek"e geri dönebilir. Bu sorun düzeltilmiştir.

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

### <a name="users-of-high-resolution-screens-may-experience-small-text-in-the-threat-properties"></a>Yüksek çözünürlüklü ekranların kullanıcıları tehdit özelliklerinde küçük metinlerle karşılaşabilir

#### <a name="issue"></a>Sorun

Aracın Çözümleme Görünümü'nde, kullanıcının Windows'da okunabilirlik için varsayılan olarak büyütülecek şekilde ayarlanmış yüksek çözünürlüklü bir ekranı varsa, tehdidin "Olası Azaltma(lar)" bölümü küçük metinle görünebilir.

![Yüksek çözünürlüklü ekranlarile bilinen sorun](./media/threat-modeling-tool-releases-71509112/tmt_screen_resolution-300x153.png)

#### <a name="workaround"></a>Geçici çözüm

Kullanıcı azaltma metnini tıklatabilir ve bu bölümün büyütmesini artırmak için standart Windows yakınlaştırma denetimini (Crtl-Mouse Wheel Up) kullanabilir.

### <a name="files-in-the-recently-opened-models-section-of-the-main-window-may-fail-to-open"></a>Ana pencerenin "Son Açılan Modeller" bölümündeki dosyalar açılmayabilir

#### <a name="issue"></a>Sorun

Önizleme sürümündeki "OneDrive'dan Aç" özelliği kaldırıldı. OneDrive'a kaydedilen "Son Açılan Modeller" olan kullanıcılar aşağıdaki hatayı alır.

![OneDrive özelliği kaldırıldı](./media/threat-modeling-tool-releases-71509112/tmt_save_error-300x131.png)

#### <a name="workaround"></a>Geçici çözüm

OneDrive kullanıcılarının, OneDrive'da depolanan dosyalarına standart ve "Model aç" iletişim kutusu aracılığıyla erişmek için Microsoft'un Windows için [OneDrive](https://onedrive.live.com/about/en-us/download/) istemcisini kullanmaları tavsiye edilir.

![OneDrive özelliği kaldırıldı](./media/threat-modeling-tool-releases-71509112/tmt_save_onedrive-300x149.png)

### <a name="my-organization-uses-the-2016-version-of-the-tool-can-i-use-the-azure-stencil-set"></a>Kuruluşum aracın 2016 sürümünü kullanıyor, Azure şablon kümesini kullanabilir miyim?

Evet, yapabilirsin! [Azure şablon seti GitHub'da kullanılabilir](https://github.com/Microsoft/threat-modeling-templates/)ve aracın 2016 sürümünde yüklenebilir. Azure şablon kümesiyle yeni bir model oluşturmak için ana menü ekranında "Yeni Modeller Için Şablon" iletişim kutusunu kullanın. TMT 2016, Azure şablon kümesinin "Olası Azaltıcı Etazaltmalar" alanlarında bulunan bağlantıları işleyemez, bu nedenle HTML etiketleri olarak görüntülenen bağlantılar görebilirsiniz.

![2016 İstemcisinde Azure Şablon Güncellemeleri](./media/threat-modeling-tool-releases-71509112/tmt_azure_stencils-300x212.png)

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - Microsoft Windows 10
- .NET Sürümü Gerekli
  - .NET 3.5.2
- Ek Gereksinimler
  - Şablonların yanı sıra araçla ilgili güncelleştirmeleri de almak için Internet bağlantısı gereklidir.

## <a name="documentation-and-feedback"></a>Dokümantasyon ve geri bildirim

- Tehdit Modelleme Aracı için Dokümantasyon [docs.microsoft.com](threat-modeling-tool.md)üzerinde yer alır ve [aracı kullanma hakkında](threat-modeling-tool-getting-started.md)bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Tehdit Modelleme Aracı'nın en son sürümünü [indirin.](https://aka.ms/threatmodelingtool)
