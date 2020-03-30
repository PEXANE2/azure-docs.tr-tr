---
title: Azure İşlevler Çalışma Süresine Genel Bakış
description: Azure İşlerinden Çalışma Zamanı Önizlemesi'ne Genel Bakış
author: apwestgarth
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: ab04aa4ca7f54e8de120d078a313c3096a350aa5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74226622"
---
# <a name="azure-functions-runtime-overview-preview"></a>Azure İşlevler Çalışma Süresine Genel Bakış (önizleme)

[!INCLUDE [intro](../../includes/functions-runtime-preview-note.md)]

Azure İşlevleri Çalışma Süresi (önizleme), Azure İşlevleri programlama modelinin şirket içi basitliğinden ve esnekliğinden yararlanmanız için yeni bir yol sağlar. Azure İşlevler ile aynı açık kaynak kökleri üzerine inşa edilen Azure İşlevler Çalışma Süresi, bulut hizmetiyle neredeyse aynı geliştirme deneyimini sağlamak için şirket içinde dağıtılır.

![Azure İşlevler Çalışma Zamanı Önizleme Portalı][1]

Azure İşlevler Çalışma Süresi, buluta bağlanmadan önce Azure Işlevlerini deneyimlemeniz için bir yol sağlar. Bu şekilde, oluşturduğunuz kod varlıkları, geçiş yaptığınızda buluta götürebilir.  Çalışma süresi, şirket içi bilgisayarlarınızın toplu işlemleri bir gecede çalıştırmak için yedek işlem gücünü kullanmak gibi yeni seçenekler de açar. Kuruluşunuzdaki aygıtları, hem şirket içinde hem de buluttaki diğer sistemlere koşullu olarak veri göndermek için de kullanabilirsiniz.

Azure İşlevler Çalışma Süresi iki parçadan oluşur:

* Azure İşlevler Runtime Yönetim Rolü
* Azure İşlevler Çalışma Zamanı Çalışan Rolü

## <a name="azure-functions-management-role"></a>Azure İşlevyönetimi Rolü

Azure İşlev yönetimi rolü, İşlevlerinizin şirket içinde yönetimi için bir ana bilgisayar sağlar. Bu rol aşağıdaki görevleri gerçekleştirir:

* [Azure Portalı'nda](https://portal.azure.com)gördüğünüz Azure İşlevler Yönetimi Portalı'nın barındırılı ev sahipliği. Portal, işlevlerinizi Azure portalında olduğu gibi geliştirmenize olanak tanıyan tutarlı bir deneyim sağlar.
* İşlevleri birden çok Fonksiyon işçisi arasında dağıtma.
* Yayımlama profilini indirip içe aktararak işlevlerinizi doğrudan Microsoft Visual Studio'dan yayımlayabilmeniz için bir yayımlama bitiş noktası sağlamak.

## <a name="azure-functions-worker-role"></a>Azure Fonksiyonları İşçi Rolü

Azure İşlevler İşçi Rolleri Windows Kapsayıcılarında dağıtılır ve işlev kodunuz yürütüldüğü yerdir.  Kuruluşunuz genelinde birden çok İşçi Rolü dağıtabilirsiniz ve bu seçenek, müşterilerin yedek işlem gücünden yararlanabildiği önemli bir yoldur.  Birçok kuruluşta yedek işlem in bulunduğu yerlere bir örnek, sürekli olarak çalışan ancak uzun süre kullanılmayan makinelerdir.

## <a name="minimum-requirements"></a>Minimum Gereksinimler

Azure İşlevler Çalışma Zamanı'na başlamak için, Windows Server 2016 veya Windows 10 Creators Update'e sahip ve bir SQL Server örneğine erişen bir makineniz olması gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

Azure [İşlevleri Çalışma Zamanı önizlemesini](https://aka.ms/azafrdoc) yükleme

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
