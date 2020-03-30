---
title: Azure DevTest Labs'daki bir laboratuvara etiket ekleme | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvara etiket eklemeyi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: dc5b327a-62e4-41bc-80ef-deb3c23d51b2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2019
ms.author: spelluru
ms.openlocfilehash: 1d7175d000eebd3f68665e7f097bbe3f68025bc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72302788"
---
# <a name="add-tags-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara etiket ekleme

Kaynaklarınızı mantıksal olarak kategorilere ayırmak için özel etiketler oluşturabilir ve bunları DevTest Labs kaynaklarınıza uygulayabilirsiniz. Daha sonra, aboneliğinizde bu etikete sahip tüm kaynakları hızlı ve kolay bir şekilde görebilirsiniz. Etiketler, faturalandırma veya yönetim için kaynakları düzenlemeniz gerektiğinde yararlıdır.

Etiketlerle desteklenen kaynaklar şunlardır:

* İşlem Lim'leri
* NIC’ler
* IP adresleri
* Yük dengeleyiciler
* Depolama hesapları
* Yönetilen diskler

Bir [laboratuvar oluşturduğunuzda](devtest-lab-create-lab.md) etiketleri uygulayabilir ve daha sonra Yapılandırma ve ayarlar altında Etiketler bıçağı aracılığıyla yönetebilirsiniz.

Her etiket bir **ad**/**değeri** çiftinden oluşur. Örneğin, *34543*değerine sahip *costcenter* adında bir etiket oluşturabilirsiniz. Bunun gibi bir etiket, kuruluşunuzun bu özel alanı için faturalandırılabilen laboratuvar kaynaklarını daha sonra tanımlamanıza yardımcı olabilir. Aboneliğinizi nasıl düzenlemek istediğinize dair anlamlı adlar ve değerler seçebilirsiniz.

## <a name="steps-to-manage-tags-in-an-existing-lab"></a>Varolan bir laboratuvarda etiketleri yönetme adımları

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. Gerekirse Tüm **Hizmetler'i**seçin ve listeden **DevTest Labs'ı** seçin. Laboratuvarınız **Tüm Kaynaklar**altında Pano'da zaten gösterilmiş olabilir.
1. Laboratuvarlar listesinden, etiket eklemek veya yönetmek istediğiniz laboratuarı seçin.
1. Laboratuvarın Genel **Bakış** alanında **Yapılandırma ve ilkeler'i**seçin.

    ![Yapılandırma ve ilkeler düğmesi](./media/devtest-lab-add-tag/devtestlab-config-and-policies.png)

1. **YÖNET'in**altında solda, **Etiketler'i**seçin.
1. Bu laboratuvar için yeni bir etiket oluşturmak için bir **Ad**/**Değeri** çifti girin ve **Kaydet'i**seçin. Ayrıca, bu etiketle ilişkili kaynakları görüntülemek veya yönetmek için listeden varolan bir etiket de seçebilirsiniz.

    ![Etiketleri yönetme](./media/devtest-lab-add-tag/devtestlab-manage-tags.png)

> [!NOTE]
> Laboratuvar düzeyinde oluşturulan etiketler, aboneliğinizde döndürülen tüm faturalandırılabilir kaynaklar üzerinden akar. Örneğin, laboratuvar düzeyindeetiketler, laboratuvar VM'lerinin altında yatan bilgi işlem VM'lerine akar.Maliyet yönetimi bağlamında etiketleri kullanabilirsiniz. Laboratuvar düzeyi etiketleri, maliyet yönetimi için etiket filtresinde gösteriş gösterir.

## <a name="understanding-limitations-to-tags"></a>Etiketlere sınırlamaları anlama

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

[Azure kaynaklarınızı düzenlemek için etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) kullanma, PowerShell veya Azure CLI kullanarak etiketleri yönetme de dahil olmak üzere Azure'da etiketleri kullanma hakkında daha fazla ayrıntı sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş ilkeler kullanarak aboneliğiniz genelinde kısıtlamalar ve sözleşmeler uygulayabilirsiniz. Tanımladığınız bir ilke, tüm kaynakların belirli bir etiket için bir değeri olduğunu gerektirebilir. Daha fazla bilgi için [bkz.](devtest-lab-set-lab-policy.md)
* [DevTest Labs Azure Kaynak Yöneticisi hızlı başlangıç şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)keşfedin.
