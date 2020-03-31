---
title: Sorun Giderme VM ve ortam hataları Azure DevTest Labs
description: Azure DevTest Labs'da sanal makine (VM) ve ortam oluşturma hatalarını nasıl gidereceklerini öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 155a359608cf6d846578306545f5ce0b4003949c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76166340"
---
# <a name="troubleshoot-virtual-machine-vm-and-environment-creation-failures-in-azure-devtest-labs"></a>Azure DevTest Labs'da sanal makine (VM) ve ortam oluşturma hatalarını giderme
DevTest Labs, bir makine adı geçersizse veya bir laboratuvar ilkesini ihlal etmek üzereyseniz size uyarılarda bulunur. Bazen, bir `X` şeylerin ters gittiğini bildiren laboratuar VM veya çevre durumunun yanında kırmızı görürsünüz.  Bu makalede, altta yatan sorunu bulmak ve umarım, gelecekte sorunu önlemek için kullanabileceğiniz birkaç hileler sağlar.

## <a name="portal-notifications"></a>Portal bildirimleri
Azure portalını kullanıyorsanız, ilk olarak **bildirimler paneline**bayılAbilirsiniz.  Ana komut çubuğunda **bulunan bildirimler paneli, bell simgesine**tıklayarak, laboratuvar VM veya ortam oluşturma başarılı olup olmadığını size söyleyecektir.  Bir hata varsa, oluşturma hatasıyla ilişkili hata iletisini görürsünüz. Ayrıntılar genellikle sorunu çözmenize yardımcı olmak için daha fazla bilgi verir. Aşağıdaki örnekte, sanal makine oluşturma çekirdekleri tükeniyor nedeniyle başarısız oldu. Ayrıntılı ileti, sorunu nasıl düzeltip çekirdek kota artışı istediğinizi söyler.

![Azure portalı bildirimi](./media/troubleshoot-vm-environment-creation-failures/portal-notification.png)

### <a name="vm-in-corruption-state"></a>Yolsuzluk durumunda VM
VM'nizin durumunu laboratuvarda **Bozuk**olarak görürseniz, altta yatan VM, kullanıcının **Sanal Makineler** sayfasından (DevTest Labs sayfasından değil) gidebileceği **Sanal Makine** sayfasından silinmiş olabilir. DevTest Labs'daki laboratuarınızı, VM'yi laboratuvardan silerek temizleyin. Sonra, laboratuvarda VM yeniden oluşturun. 

![Bozuk durumda VM](./media/troubleshoot-vm-environment-creation-failures/vm-corrupted-state.png)



## <a name="activity-logs"></a>Etkinlik günlükleri
VM'nizin veya ortamınızın oluşturulmasını denedikten sonra bir arızayı araştırıyorsanız etkinlik günlüklerine bakın. Bu bölümde, VM'ler ve ortamlar için günlükleri nasıl bulacağınızı gösterir.

## <a name="activity-logs-for-virtual-machines"></a>Sanal makineler için etkinlik günlükleri

1. Laboratuvarınızın ana sayfasında Sanal **Makine** sayfasını başlatmak için VM'yi seçin.
2. Sanal **Makine** sayfasında, sol menünün **İzleme** bölümünde, VM ile ilişkili tüm günlükleri görmek için **Etkinlik günlükünü** seçin.
3. Etkinlik günlüğü öğelerinde, başarısız olan işlemi seçin. Genellikle, başarısız işlem denir. `Write Virtualmachines`
4. Sağ bölmede JSON sekmesine geçin. Günlüğün JSON görünümünde ayrıntıları görüyorsunuz.

    ![VM için etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/vm-activity-log.png)
5. Mülkü bulana kadar JSON `statusMessage` günlüğüne bak. Varsa, size ana hata iletisi ve daha fazla ayrıntı bilgisi verir. Aşağıdaki JSON çekirdek bu makalede daha önce görülen aşıldı hata alıntı için bir örnektir.

    ```json
    "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"OperationNotAllowed\",\"message\":\"Operation results in exceeding quota limits of Core. Maximum allowed: 100, Current in use: 100, Additional requested: 8. Please read more about quota increase at https://aka.ms/corequotaincrease.\"}]}}",
    },
    ```

## <a name="activity-log-for-an-environment"></a>Bir ortam için etkinlik günlüğü

Ortam oluşturma nın etkinlik günlüğünü görmek için aşağıdaki adımları izleyin:

1. Laboratuvarınızın ana sayfasında, sol menüde **Yapılandırma ve ilkeler'i** seçin.
2. Yapılandırma **ve ilkeler** sayfasında menüdeki **Etkinlik günlüklerini** seçin.
3. Günlükteki etkinlik listesindeki hatayı arayın ve seçin.
4. Sağ bölmede, JSON sekmesine geçin ve **statusMessage'ı**arayın.

    ![Ortam etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/envirionment-activity-log.png)

## <a name="resource-manager-template-deployment-logs"></a>Kaynak Yöneticisi şablon dağıtım günlükleri
Ortamınız veya sanal makineniz otomasyon aracılığıyla oluşturulduysa, hata bilgilerine bakmak için son bir yer vardır. Bu, Azure Kaynak Yöneticisi şablon dağıtım günlüğü. Bir laboratuvar kaynağı otomasyon yoluyla oluşturulduğunda, genellikle bir Azure Kaynak Yöneticisi şablon dağıtımı yoluyla yapılır. DevTest Labs kaynaklarını oluşturan örnek Azure Kaynak Yöneticisi şablonlarına bakın.[https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)

Laboratuvar şablonu dağıtım günlüklerini görmek için aşağıdaki adımları izleyin:

1. Laboratuvarın bulunduğu kaynak grubu için sayfayı başlatın.
2. **Ayarlar'ın**altındaki sol menüde **Dağıtımlar'ı** seçin.
3. Başarısız durumu olan dağıtımları arayın ve seçin.
4. **Dağıtım** sayfasında, başarısız olan işlem için **İşlem ayrıntıları** bağlantısını seçin.
5. **İşlem ayrıntıları** penceresinde başarısız olan işlemle ilgili ayrıntıları görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
[Bkz. Sorun giderme yapı tonu hataları](devtest-lab-troubleshoot-artifact-failure.md)
