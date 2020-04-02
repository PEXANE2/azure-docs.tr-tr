---
title: Azure Veri Gezgini kümenizde dil uzantılarını yönetin.
description: Azure Veri Gezgini KQL sorgularınızda diğer dilleri tümleştirmek için dil uzantısını kullanın.
author: orspod
ms.author: orspodek
ms.reviewer: orhasban
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 88c3047afebf3d31b50145c6df47776fdc1ddcd4
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522478"
---
# <a name="manage-language-extensions-in-your-azure-data-explorer-cluster-preview"></a>Azure Veri Gezgini kümenizde dil uzantılarını yönetme (Önizleme)

Dil uzantıları özelliği, diğer dilleri Azure Veri Gezgini KQL sorgularınıza entegre etmek için dil uzantısı eklentilerini kullanmanıza olanak tanır. İlgili bir komut dosyası kullanarak kullanıcı tanımlı bir işlev (UDF) çalıştırdığınızda, komut dosyası girdiolarak tabular veri alır ve tabular çıktı sıyrık oluşturması beklenir. Eklentinin çalışma süresi, kümenin düğümlerinde çalışan, yalıtılmış ve güvenli bir ortamda bulunan bir [kum havuzunda](/azure/kusto/concepts/sandboxes)barındırılır. Bu makalede, Azure portalındaki Azure Veri Gezgini kümenizdeki dil uzantıları eklentisini yönetirsiniz.

> [!NOTE]
> Şu anda desteklenen Azure Veri Gezgini dil uzantıları Python ve R'dir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)oluşturun.

## <a name="enable-language-extensions-on-your-cluster"></a>Kümenizde dil uzantılarını etkinleştirme

> [!WARNING]
> Lütfen bir dil uzantısını etkinleştirmeden önce [sınırlamaları](#limitations) gözden geçirin.

Kümenizde dil uzantılarını etkinleştirmek için aşağıdaki adımları yapın:

1. Azure portalında Azure Veri Gezgini kümenize gidin. 
1. **Ayarlar'da** **Yapılandırmalar'ı**seçin. 
1. **Yapılandırmalar** bölmesinde, dil uzantısını etkinleştirmek için **Açık'ı** seçin.
1. **Kaydet'i**seçin.
 
    ![üzerinde dil uzantısı](media/language-extensions/configurations-enable-extension.png)

> [!NOTE]
> Dil uzatma işlemini etkinleştirmek birkaç dakika sürebilir. Bu süre zarfında kümeniz askıya alınır.
 
## <a name="run-language-extension-integrated-queries"></a>Dil uzantısı tümleşik sorguları çalıştırma

* [Python tümleşik KQL sorgularını nasıl çalıştırılanmayı](/azure/kusto/query/pythonplugin)öğrenin.
* [R tümleşik KQL sorgularını nasıl çalıştırılacıla](/azure/kusto/query/rplugin) 

## <a name="disable-language-extensions-on-your-cluster"></a>Kümenizdeki dil uzantılarını devre dışı

> [!NOTE]
> Dil uzantılarını devre dışı bırakmak birkaç dakika sürebilir.

Kümenizdeki dil uzantılarını devre dışı katmak için aşağıdaki adımları yapın:

1. Azure portalında Azure Veri Gezgini kümenize gidin. 
1. **Ayarlar'da** **Yapılandırmalar'ı**seçin. 
1. **Yapılandırmalar** bölmesinde, dil uzantısını devre dışı ksaymak için **Kapalı'yı** seçin.
1. **Kaydet'i**seçin.

    ![Dil uzantısı kapalı](media/language-extensions/configurations-disable-extension.png)

## <a name="limitations"></a>Sınırlamalar

* Dil uzantıları özelliği [Disk şifrelemeyi](manage-cluster-security.md)desteklemez. 
* Dil uzantıları çalışma zamanı sandbox, ilgili dilkapsamında hiçbir sorgu çalışmasa bile disk alanı ayırır.

