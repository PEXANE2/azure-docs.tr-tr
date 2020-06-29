---
title: VM ve ortam hatalarında sorun giderme Azure DevTest Labs
description: Azure DevTest Labs ' de sanal makine (VM) ve ortam oluşturma hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: b7d3f3ad34d8a5bb48607816623c67121d21d78c
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85476487"
---
# <a name="troubleshoot-virtual-machine-vm-and-environment-creation-failures-in-azure-devtest-labs"></a>Azure DevTest Labs 'de sanal makine (VM) ve ortam oluşturma hatalarıyla ilgili sorunları giderme
DevTest Labs, bir makine adı geçersizse veya bir laboratuvar ilkesini ihlal etmek üzere olduğunuzda size uyarı verir. Bazen, `X` Laboratuvar sanal makinenizin veya ortam durumunun bir yanındaki kırmızı olduğunu bildiren bir sorun olduğunu bildiren bir hata olduğunu görürsünüz.  Bu makalede, temeldeki sorunu bulmak ve ileride sorunu önlemek için kullanabileceğiniz birkaç el yer almaktadır.

## <a name="portal-notifications"></a>Portal bildirimleri
Azure portal kullanıyorsanız, **bildirim paneli**' ne ilk baktığımız olur.  Ana komut çubuğunda bulunan bildirimler paneli, **Bell simgesine**tıklayarak, laboratuvar VM 'si veya ortam oluşturma işleminin başarılı olup olmadığını bildirir.  Bir hata oluşursa, oluşturma hatasıyla ilişkili hata iletisini görürsünüz. Ayrıntılar genellikle sorunu çözmenize yardımcı olacak daha fazla bilgi verir. Aşağıdaki örnekte, sanal makine oluşturma, çekirdekler tükentiğinden başarısız oldu. Ayrıntılı ileti, sorunu nasıl düzelteceğinizi ve çekirdek kota artışı isteyeceğini size bildirir.

![Azure portal bildirimi](./media/troubleshoot-vm-environment-creation-failures/portal-notification.png)

### <a name="vm-in-corruption-state"></a>Bozulma durumunda VM
Laboratuvardaki sanal makinenizin durumunu **bozuk**olarak görürseniz, temeldeki VM, kullanıcının **sanal makineler** sayfasından (DevTest Labs sayfasından değil) gidebileceği **sanal makine** sayfasından silinmiş olabilir. VM 'yi laboratuvardan silerek DevTest Labs 'de laboratuvarınızı temizleyin. Ardından, VM 'nizi laboratuvarda yeniden oluşturun. 

![VM bozuk durumda](./media/troubleshoot-vm-environment-creation-failures/vm-corrupted-state.png)



## <a name="activity-logs"></a>Etkinlik günlükleri
VM 'niz veya ortamınızın oluşturulması denendikten sonra bir hata araştırıyorsanız etkinlik günlüklerine bakın. Bu bölümde, VM 'Ler ve ortamlar için günlüklerin nasıl bulunacağı gösterilmektedir.

## <a name="activity-logs-for-virtual-machines"></a>Sanal makineler için etkinlik günlükleri

1. Laboratuvarınızın giriş sayfasında, **sanal makine** sayfasını başlatmak için VM 'yi seçin.
2. **Sanal makine** sayfasında, sol menünün **Izleme** bölümünde, VM ile ilişkili tüm günlükleri görmek için **etkinlik günlüğü** ' nü seçin.
3. Etkinlik günlüğü öğeleri ' nde, başarısız olan işlemi seçin. Genellikle, başarısız olan işlem çağrılır `Write Virtualmachines` .
4. Sağ bölmede JSON sekmesine geçin. Günlüğün JSON görünümünde ayrıntıları görürsünüz.

    ![Bir VM için etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/vm-activity-log.png)
5. Özelliği buluncaya kadar JSON günlüğüne bakın `statusMessage` . Bu, size ana hata iletisi ve varsa daha ayrıntılı bilgiler verir. Aşağıdaki JSON, bu makalede daha önce görülen çekirdek alıntılanmış hata hatası için bir örnektir.

    ```json
    "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"OperationNotAllowed\",\"message\":\"Operation results in exceeding quota limits of Core. Maximum allowed: 100, Current in use: 100, Additional requested: 8. Please read more about quota increase at https://aka.ms/corequotaincrease.\"}]}}",
    },
    ```

## <a name="activity-log-for-an-environment"></a>Bir ortam için etkinlik günlüğü

Bir ortam oluşturma etkinlik günlüğünü görmek için şu adımları izleyin:

1. Laboratuvarınızın giriş sayfasında sol menüdeki **yapılandırma ve ilkeler** ' i seçin.
2. **yapılandırma ve ilkeler** sayfasında, menüdeki **etkinlik günlükleri** ' ni seçin.
3. Günlükteki etkinlik listesinden hata olup olmadığına bakın ve bunu seçin.
4. Sağ bölmede JSON sekmesine geçin ve **StatusMessage**' ı arayın.

    ![Ortam etkinlik günlüğü](./media/troubleshoot-vm-environment-creation-failures/envirionment-activity-log.png)

## <a name="resource-manager-template-deployment-logs"></a>Kaynak Yöneticisi şablonu dağıtım günlükleri
Ortamınız veya sanal makineniz Otomasyon aracılığıyla oluşturulduysa, hata bilgilerini aramak için bir son yer vardır. Azure Resource Manager şablonu dağıtım günlüğü bu. Laboratuvar kaynağı Otomasyon aracılığıyla oluşturulduğunda, genellikle Azure Resource Manager şablon dağıtımı aracılığıyla yapılır. [https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)DevTest Labs kaynaklarını oluşturan örnek Azure Resource Manager şablonları için bkz..

Laboratuvar şablonu dağıtım günlüklerini görmek için şu adımları izleyin:

1. Laboratuvarın bulunduğu kaynak grubu için sayfayı başlatın.
2. **Ayarlar**' ın altında sol menüdeki **dağıtımlar** ' ı seçin.
3. Başarısız durumundaki dağıtımları arayın ve seçin.
4. **Dağıtım** sayfasında, başarısız olan Işlem için **işlem ayrıntıları** bağlantısı ' nı seçin.
5. **İşlem ayrıntıları** penceresinde başarısız olan işlemle ilgili ayrıntıları görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [yapıt hatalarında sorun giderme](devtest-lab-troubleshoot-artifact-failure.md)
