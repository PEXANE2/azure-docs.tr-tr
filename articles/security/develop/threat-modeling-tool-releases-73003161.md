---
title: Microsoft Tehdit Modelleme Aracı sürümü 22/03/2020 - Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 03/22/2020
ms.openlocfilehash: 6fea4d48b62d6ea429d37924ffd15855db6294cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80146868"
---
# <a name="threat-modeling-tool-update-release-73003161---03222020"></a>Tehdit Modelleme Aracı güncelleme sürümü 7.3.00316.1 - 22.03.2020

Microsoft Tehdit Modelleme Aracı'nın (TMT) 7.3.00316.1 sürümü 22 Mart 2020'de yayımlanmış ve aşağıdaki değişiklikleri içermektedir:

- Erişilebilirlik geliştirmeleri
- Hata düzeltmeleri
- Yeni DiagramReader özelliği

## <a name="notable-bug-fixes"></a>Önemli hata düzeltmeleri

### <a name="exporting-the-threat-list-to-csv"></a>Tehdit listesini CSV'ye dışa aktarma

CSV işlevine yapılan dışa aktarım, tehdit listesinden hangi alanların dışa aktarılacağı tutarsız bir şekilde seçiliydi. Artık tehdit listesindeki tüm alanlar CSV dosyasına aktalacaktır. 

### <a name="ux-bugs"></a>UX hataları

- Birincil iş akışındaki yardım menüleri (oluşturma/açma/analiz et) ve şablon düzenleyicisi deneyimi artık tutarlı menü seçeneklerine sahiptir.
- Şablonlar bölmesindeki arama çubuğu artık standart bir imleç içerir ve uygun etiketler eklenmiştir.

## <a name="new-features"></a>Yeni özellikler

### <a name="diagramreader-feature-has-been-added"></a>DiagramReader özelliği eklendi

Bir model açıkken ana menüye yeni bir DiagramReader özelliği eklendi. Bu özellik, modelin grafik gösterimini metin tabanlı bir anlatıya dönüştürür. 

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen işletim sistemleri:
  - [Microsoft Windows 10 Yıldönümü Güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya sonrası
- .NET sürümü gereklidir:
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya sonrası
- Ek gereksinimler:
  - Şablonların yanı sıra araçla ilgili güncellemeleri de almak için bir internet bağlantısı

## <a name="documentation-and-feedback"></a>Dokümantasyon ve geri bildirim

- Tehdit Modelleme Aracı için Dokümantasyon [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)üzerinde yer alır ve [aracı kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started)bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Tehdit Modelleme Aracı'nın en son sürümünü [indirin.](https://aka.ms/threatmodelingtool)
