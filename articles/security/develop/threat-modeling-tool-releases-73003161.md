---
title: Microsoft Threat Modeling Tool Release 03/22/2020-Azure
description: Tehdit modelleme aracı için sürüm notlarını belgeleme
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 03/22/2020
ms.openlocfilehash: 6fea4d48b62d6ea429d37924ffd15855db6294cc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80146868"
---
# <a name="threat-modeling-tool-update-release-73003161---03222020"></a>Threat Modeling Tool güncelleştirme sürümü 7.3.00316.1-03/22/2020

Microsoft Threat Modeling Tool (TMT) sürümü 7.3.00316.1 Mart 22 2020 ' de yayımlanmıştır ve aşağıdaki değişiklikleri içerir:

- Erişilebilirlik geliştirmeleri
- Hata düzeltmeleri
- Yeni DiagramReader özelliği

## <a name="notable-bug-fixes"></a>Notable hata düzeltmeleri

### <a name="exporting-the-threat-list-to-csv"></a>Tehdit listesini CSV 'ye aktarma

CSV 'ye dışarı aktar işlevi tehdit listesindeki hangi alanların dışarı aktarılacağını sürekli olarak seçti. Artık tehdit listesindeki tüm alanlar CSV dosyasına aktaralınacaktır. 

### <a name="ux-bugs"></a>UX hataları

- Birincil iş akışındaki (Oluştur/Aç/çözümle) ve Şablon Düzenleyicisi deneyiminde yardım menüleri artık tutarlı menü seçeneklerine sahiptir.
- Kalıplar bölmesindeki arama çubuğu artık standart bir imlece sahiptir ve uygun Etiketler eklenmiştir.

## <a name="new-features"></a>Yeni özellikler

### <a name="diagramreader-feature-has-been-added"></a>DiagramReader özelliği eklendi

Bir model açıkken ana menüye yeni bir DiagramReader özelliği eklenmiştir. Bu özellik, modelin grafik temsilini metin tabanlı bir anlatıcı olarak dönüştürür. 

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen işletim sistemleri:
  - [Microsoft Windows 10 yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- Gerekli .NET sürümü:
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek gereksinimler:
  - Araç ve şablonların güncelleştirmelerini almak için internet bağlantısı

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Threat Modeling Tool belgeleri [docs.Microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)konumunda bulunur ve [Aracı kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started)bilgiler içerir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)en son sürümünü indirin.
