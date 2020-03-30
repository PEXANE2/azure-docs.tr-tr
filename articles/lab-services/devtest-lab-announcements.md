---
title: Azure DevTest Labs'daki bir laboratuvara bir erteleme gönderin | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvara nasıl duyuru ekleyeceğinizi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 84120b07de3a03a049493eb973b6dc46f8668387
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75976442"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara duyuru gönderin

Laboratuvar yöneticisi olarak, kullanıcıları laboratuvardaki son değişiklikler veya eklemeler hakkında bilgilendirmek için varolan bir laboratuvarda özel bir duyuru yayınlayabilirsiniz. Örneğin, kullanıcıları şu lar hakkında bilgilendirmek isteyebilirsiniz:

- Kullanılabilen yeni VM boyutları
- Şu anda kullanılamaz olan görüntüler
- Laboratuvar ilkelerinde güncelleştirmeler

Duyuruldu, duyurulduktan sonra laboratuvarın Genel Bakış sayfasında görüntülenir ve kullanıcı daha fazla ayrıntı için duyuruyu seçebilir.

Duyuru özelliği geçici bildirimler için kullanılmak üzere hazırdır.  Artık ihtiyaç duyulmadıktan sonra bir duyuruyu kolayca devre dışı kullanabilirsiniz.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Varolan bir laboratuvarda duyuru gönderme adımları

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. Gerekirse Tüm **Hizmetler'i**seçin ve listeden **DevTest Labs'ı** seçin. (Laboratuvarınız **Tüm Kaynaklar**altında Pano'da zaten gösterilmiş olabilir).
1. Laboratuvarlar listesinden, bir duyuru göndermek istediğiniz laboratuarı seçin.
1. Laboratuvarın Genel **Bakış** alanında **Yapılandırma ve ilkeler'i**seçin.

    ![Yapılandırma ve ilkeler düğmesi](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. **SETTINGS**altında solda, **Lab duyurusunu**seçin.

    ![Laboratuvar duyuru düğmesi](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Bu laboratuvardaki kullanıcılar için bir ileti oluşturmak için **Etkin'i** **Evet'e**ayarlayın.

1. Duyurunun artık kullanıcılara gösterilmediği bir tarih ve saat belirtmek için bir **Son Kullanma tarihi** girebilirsiniz. Bir son kullanma tarihi girmezseniz, siz devre dışı kalana kadar duyuru kalır.

   > [!NOTE]
   > Duyuru nun süresi dolduktan sonra, artık kullanıcılara gösterilmez, ancak yine de **Laboratuvar duyuru** bölmesinde bulunur. Bunu yeniden etkinleştirebilir ve yeniden etkin hale getirmek için yeniden etkinleştirebilirsiniz.
   >
   >

1. Duyuru **başlığını** ve **Duyuru metnini**girin.

   Başlık en fazla 100 karakter olabilir ve kullanıcıya laboratuvarın Genel Bakış sayfasında gösterilir. Kullanıcı başlığı seçerse, duyuru metni görüntülenir.

   Duyuru metni işaretlemeyi kabul eder. Duyuru metnini girdiğinizde, ekranın altındaki Önizleme alanında iletiyi görüntüleyebilirsiniz.

    ![İletiyi oluşturmak için laboratuvar duyuru ekranı.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Duyurunuz yayına hazır olduğunda **Kaydet'i** seçin.

Bu duyuruyu artık laboratuvar kullanıcılarına göstermek istemiyorsanız, **Laboratuvar duyuru** sayfasına dönün ve **Etkin'i** **Hayır**olarak ayarlayın. Bir son kullanma tarihi belirttiyseniz, duyuru o tarih ve saatte otomatik olarak devre dışı bırakılır.

## <a name="steps-for-users-to-view-an-announcement"></a>Kullanıcıların bir duyuruyu görüntüleme adımları

1. Azure [portalından](https://go.microsoft.com/fwlink/p/?LinkID=525040)bir laboratuvar seçin.

1. Laboratuvarda bunun için bir duyuru yayınlanmışsa, laboratuvarın Genel Bakış sayfasının üst kısmında bir bilgi bildirimi gösterilir. Bu bilgi bildirimi, duyuru oluşturulduğunda belirtilen duyuru başlığıdır.

    ![Genel Bakış sayfasında laboratuvar duyurusu](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Kullanıcı, duyurunun tamamını görüntülemek için iletiyi seçebilir.

    ![Laboratuvar duyurusu için daha fazla bilgi](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bir duyuruyu aşağıdaki örnekte gösterildiği gibi Azure Kaynak Yöneticisi şablonunun bir parçası olarak belirtebilirsiniz:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string",
            "defaultValue": "devtestlabfromarm"
        },
        "regionId": {
            "type": "string",
            "defaultValue": "eastus"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-26-preview",
            "name": "[parameters('name')]",
            "type": "Microsoft.DevTestLab/labs",
            "location": "[parameters('regionId')]",
            "tags": {},
            "properties": {
                "labStorageType": "Premium",
                "announcement":
                {
                    "title": "Important! Three images are currently inaccessible. Click for more information.",
                    "markdown":"# Images are inaccessible\n\nThe following 3 images are currently not available for use: \n\n- image1\n- image2\n- image3\n\nI am working to fix the problem ASAP.",
                    "enabled": "Enabled",
                    "expirationDate":"2018-12-31T06:00:00+00:00",
                    "expired": "false"
                },
                "support": {
                    "markdown": "",
                    "enabled": "Enabled"
                }
            },
            "resources": [
                {
                    "apiVersion": "2017-04-26-preview",
                    "name": "LabVmsShutdown",
                    "location": "[parameters('regionId')]",
                    "type": "schedules",
                    "dependsOn": [
                        "[resourceId('Microsoft.DevTestLab/labs', parameters('name'))]"
                    ],
                    "properties": {
                        "status": "Enabled",
                        "timeZoneId": "Eastern Standard Time",
                        "dailyRecurrence": {
                            "time": "1900"
                        },
                        "taskType": "LabVmsShutdownTask",
                        "notificationSettings": {
                            "status": "Disabled",
                            "timeInMinutes": 30
                        }
                    }
                },
                {
                    "apiVersion": "2017-04-26-preview",
                    "name": "[concat('Dtl', parameters('name'))]",
                    "type": "virtualNetworks",
                    "location": "[parameters('regionId')]",
                    "dependsOn": [
                        "[resourceId('Microsoft.DevTestLab/labs', parameters('name'))]"
                    ]
                }
            ]
        }
    ]
}
```

Aşağıdaki yollardan birini kullanarak bir Azure Kaynak Yöneticisi şablonu dağıtabilirsiniz:

- [Azure portalında](../azure-resource-manager/templates/deploy-portal.md)
- [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md)
- [Azure CLI](../azure-resource-manager/templates/deploy-cli.md)
- [REST API](../azure-resource-manager/templates/deploy-rest.md)

## <a name="next-steps"></a>Sonraki adımlar
* Bir laboratuvar ilkesini değiştirir veya ayarlarsanız, kullanıcıları bilgilendirmek için bir duyuru göndermek isteyebilirsiniz. [İlkeler ve zamanlamaları ayarlayın,](devtest-lab-set-lab-policy.md) özelleştirilmiş ilkeler kullanarak aboneliğiniz genelinde kısıtlamalar ve sözleşmeler uygulama hakkında bilgi sağlar.
* [DevTest Labs Azure Kaynak Yöneticisi QuickStart şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)keşfedin.
