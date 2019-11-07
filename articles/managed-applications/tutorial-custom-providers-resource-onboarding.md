---
title: Azure özel sağlayıcılarıyla kaynak ekleme
description: Özel sağlayıcılar aracılığıyla kaynak ekleme, mevcut Azure kaynaklarının yönetilmesine ve uzantısına izin verir.
services: managed-applications
ms.service: managed-applications
ms.topic: tutorial
ms.author: jobreen
author: jjbfour
ms.date: 09/17/2019
ms.openlocfilehash: c722b4dc3219f76f8c7c571af3613996fec9e69c
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608683"
---
# <a name="tutorial-resource-onboarding-with-azure-custom-providers"></a>Öğretici: Azure özel sağlayıcılarıyla kaynak ekleme

Bu öğretici, Azure 'a, Azure Resource Manager API 'sini "Microsoft. CustomProviders/Association" kaynak türüyle genişleten özel bir kaynak sağlayıcısı dağıtacaktır. Bu örnek, özel sağlayıcı örneğinin yaşadığı kaynak grubunun dışındaki mevcut kaynakların nasıl genişletileceğini gösterir. Bu örnekte, özel kaynak sağlayıcısı bir Azure Logic App tarafından desteklenir, ancak tüm ortak API uç noktaları kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için şunları bilmeniz gerekir:

* [Azure özel sağlayıcı](custom-providers-overview.md) özellikleri.
* [Özel sağlayıcılarla kaynak ekleme](concepts-custom-providers-resourceonboarding.md).

## <a name="getting-started-with-resource-onboarding"></a>Kaynak ekleme ile çalışmaya başlama

Bu örnekte, dağıtılması gereken iki parça vardır: özel sağlayıcı ve ilişkilendirme. Örneği basitleştirmek için, isteğe bağlı olarak her ikisini de dağıtabilecek tek bir şablon vardır.

Şablon aşağıdaki kaynakları kullanacaktır:

* Microsoft. CustomProviders/resourceProviders
* Microsoft. Logic/iş akışları
* Microsoft. CustomProviders/ilişkilendirmeleri

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "allowedValues": [
                "australiaeast",
                "eastus",
                "westeurope"
            ],
            "metadata": {
                "description": "Location for the resources."
            }
        },
        "logicAppName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the logic app to be created."
            }
        },
        "customResourceProviderName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
            "metadata": {
                "description": "Name of the custom provider to be created."
            }
        },
        "customResourceProviderId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The resource ID of an existing custom provider. Provide this to skip deployment of new logic app and custom provider."
            }
        },
        "associationName": {
            "type": "string",
            "defaultValue": "myAssociationResource",
            "metadata": {
                "description": "Name of the custom resource that is being created."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2017-05-10",
            "condition": "[empty(parameters('customResourceProviderId'))]",
            "name": "customProviderInfrastructureTemplate",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "logicAppName": {
                            "type": "string",
                            "defaultValue": "[parameters('logicAppName')]"
                        }
                    },
                    "resources": [
                        {
                            "type": "Microsoft.Logic/workflows",
                            "apiVersion": "2017-07-01",
                            "name": "[parameters('logicAppName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "state": "Enabled",
                                "definition": {
                                    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
                                    "actions": {
                                        "Switch": {
                                            "cases": {
                                                "Case": {
                                                    "actions": {
                                                        "CreateCustomResource": {
                                                            "inputs": {
                                                                "body": {
                                                                    "properties": "@addProperty(triggerBody().Body['properties'], 'myDynamicProperty', 'myDynamicValue')"
                                                                },
                                                                "statusCode": 200
                                                            },
                                                            "kind": "Http",
                                                            "type": "Response"
                                                        }
                                                    },
                                                    "case": "CREATE"
                                                }
                                            },
                                            "default": {
                                                "actions": {
                                                    "DefaultHttpResponse": {
                                                        "inputs": {
                                                            "statusCode": 200
                                                        },
                                                        "kind": "Http",
                                                        "type": "Response"
                                                    }
                                                }
                                            },
                                            "expression": "@triggerBody().operationType",
                                            "type": "Switch"
                                        }
                                    },
                                    "contentVersion": "1.0.0.0",
                                    "outputs": {},
                                    "parameters": {},
                                    "triggers": {
                                        "CustomProviderWebhook": {
                                            "inputs": {
                                                "schema": {}
                                            },
                                            "kind": "Http",
                                            "type": "Request"
                                        }
                                    }
                                }
                            }
                        },
                        {
                            "type": "Microsoft.CustomProviders/resourceProviders",
                            "apiVersion": "2018-09-01-preview",
                            "name": "[parameters('customResourceProviderName')]",
                            "location": "[parameters('location')]",
                            "properties": {
                                "resourceTypes": [
                                    {
                                        "name": "associations",
                                        "mode": "Secure",
                                        "routingType": "Webhook,Cache,Extension",
                                        "endpoint": "[[listCallbackURL(concat(resourceId('Microsoft.Logic/workflows', parameters('logicAppName')), '/triggers/CustomProviderWebhook'), '2017-07-01').value]"
                                    }
                                ]
                            }
                        }
                    ],
                    "outputs": {
                        "customProviderResourceId": {
                            "type": "string",
                            "value": "[resourceId('Microsoft.CustomProviders/resourceProviders', parameters('customResourceProviderName'))]"
                        }
                    }
                }
            }
        },
        {
            "type": "Microsoft.CustomProviders/associations",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('associationName')]",
            "location": "global",
            "properties": {
                "targetResourceId": "[if(empty(parameters('customResourceProviderId')), reference('customProviderInfrastructureTemplate').outputs.customProviderResourceId.value, parameters('customResourceProviderId'))]",
                "myCustomInputProperty": "myCustomInputValue",
                "myCustomInputObject": {
                    "Property1": "Value1"
                }
            }
        }
    ],
    "outputs": {
        "associationResource": {
            "type": "object",
            "value": "[reference(parameters('associationName'), '2018-09-01-preview', 'Full')]"
        }
    }
}
```

### <a name="deploy-the-custom-provider-infrastructure"></a>Özel sağlayıcı altyapısını dağıtma

Şablonun ilk bölümü özel sağlayıcı altyapısını dağıtmakta. Bu altyapı, "ilişkilendirmeler" kaynağının etkisini tanımlar. Özel sağlayıcılar hakkında bilginiz yoksa bkz. [özel sağlayıcı temelleri](custom-providers-overview.md).

Özel sağlayıcı altyapısını dağıtarak, yukarıdaki şablonu kopyalayabilir, kaydedebilir ve dağıtabilir ya da Azure portal aracılığıyla uygulayın ve dağıtın.

1. https://portal.azure.comAzure portal açın.

2. `All Services` veya ortadaki arama çubuğunda "Şablonlar" araması yapın. 

![Şablon arama](media/custom-providers-resource-onboarding/templates.png)

3. "Şablonlar" dikey penceresinin sol üst kısmındaki `+ Add` düğmesine basın.

![Şablon yeni Ekle](media/custom-providers-resource-onboarding/templatesadd.png)

4. Yeni şablon için "ad" ve "Açıklama" alanlarını "genel" altında girin.

![Şablon adı ve açıklaması](media/custom-providers-resource-onboarding/templatesdescription.png)

5. JSON şablonunu "kaynak ekleme ile çalışmaya başlama" öğesinden kopyalayarak Kaynak Yöneticisi şablonunu doldur

![Kaynak Yöneticisi şablonu doldur](media/custom-providers-resource-onboarding/templatesarmtemplate.png)

6. Yeni bir şablon oluşturmak için `Add` düğmesine basın. Yeni şablon görünmüyorsa, `Refresh`isabet edin.

7. Yeni oluşturulan şablonu seçin ve `Deploy` düğmesine basın.

![Yeni oluşturulan şablonu seçin ve dağıtın](media/custom-providers-resource-onboarding/templateselectspecific.png)

8. Gerekli alanlar için ayar parametrelerini girin ve abonelik ve kaynak grubunu seçin. "Özel kaynak sağlayıcısı KIMLIĞI" boş bırakılabilir.

| Ayar Adı | Gerekli | Açıklama |
| ------------ | -------- | ----------- |
| Konum | *Yes* | Şablondaki kaynakların konumu. |
| Mantıksal uygulama adı | *eşleşen* | Mantıksal uygulama adı. |
| Özel kaynak sağlayıcısı adı | *eşleşen* | Özel kaynak sağlayıcısı adı. |
| Özel kaynak sağlayıcısı KIMLIĞI | *eşleşen* | "İlişkilendirme" kaynağını destekleyen mevcut bir özel kaynak sağlayıcısı. Bunu belirtmek, mantıksal uygulama ve özel sağlayıcı dağıtımını atlar. |
| İlişkilendirme adı | *eşleşen* | İlişkilendirme kaynağının adı. |

Örnek parametreler:

![Giriş şablonu parametreleri](media/custom-providers-resource-onboarding/templatescustomprovider.png)

9. Dağıtıma gidip işlemin bitmesini bekleyin. Başarılı ve yeni "ilişkilendirme" kaynağının çıkışını görüntülemesi gerekir.

Dağıtım başarısı:

![Özel sağlayıcı dağıtımı](media/custom-providers-resource-onboarding/customproviderdeployment.png)

"Gizli türleri göster" ile kaynak grubu:

![Özel sağlayıcı dağıtımı](media/custom-providers-resource-onboarding/showhidden.png)

10. "İlişkilendirme" oluşturma çağrılarını görmek için mantıksal uygulama "çalışma geçmişi" sekmesini inceleyin.

![Mantıksal uygulama çalıştırma geçmişi](media/custom-providers-resource-onboarding/logicapprun.png)

## <a name="deploy-additional-associations"></a>Ek ilişkilendirmeler dağıtma

Özel sağlayıcı altyapısı kurulduktan sonra, ek "ilişkilendirmeler" de kolayca dağıtılabilir. Ek "ilişkilendirmeler" için kaynak grubunun, özel sağlayıcı altyapısının dağıtıldığı kaynak grubuyla aynı olması gerekmez. Bir ilişki oluşturmak için, belirtilen "özel kaynak sağlayıcısı KIMLIĞI" için "Microsoft. CustomProviders/resourceproviders/Write" izinleri gerekir.

1. Önceki dağıtımın kaynak grubundaki "Microsoft. CustomProviders/resourceProviders" kaynak grubundaki özel sağlayıcıya gidin. "Gizli türleri göster" onay kutusunun seçilmesi gerekir.

![Özel sağlayıcı dağıtımı](media/custom-providers-resource-onboarding/showhidden.png)

2. Özel sağlayıcının "kaynak KIMLIĞI" özelliğini kopyalayın.

3. `All Services` veya ortadaki arama çubuğunda "Şablonlar" araması yapın. 

![Şablon arama](media/custom-providers-resource-onboarding/templates.png)

4. Önceden oluşturulan şablonu seçin ve `Deploy` düğmesine basın.

![Önceden oluşturulan şablonu seçin ve dağıtın](media/custom-providers-resource-onboarding/templateselectspecific.png)

5. Gerekli alanlar için ayar parametrelerini girin ve aboneliği ve farklı bir kaynak grubunu seçin. "Özel kaynak sağlayıcısı KIMLIĞI" ayarı için, önceden dağıtılan özel sağlayıcının kopyalanmış "kaynak KIMLIĞI" ni girin.

6. Dağıtıma gidip işlemin bitmesini bekleyin. Şimdi yalnızca yeni "ilişkilendirmeler" kaynağını dağıtmalıdır.

![İlişki oluşturuldu](media/custom-providers-resource-onboarding/createdassociationresource.png)

İsteğe bağlı olarak, "çalıştırma geçmişi" mantıksal uygulamasına geri gidebilir ve mantıksal uygulamada başka bir çağrının yapıldığını görebilirsiniz. Mantıksal uygulama, oluşturulan her ilişki için ek işlevselliği artırmak üzere güncelleştirilemeyebilir.

## <a name="looking-for-help"></a>Yardım aranıyor

Azure özel sağlayıcılar hakkında sorularınız varsa [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-custom-providers)yapmayı deneyin. Benzer bir soru zaten istendi ve yanıtlamış olabilir, bu nedenle göndermeden önce kontrol edin. Hızlı bir yanıt almak için etiketi `azure-custom-providers` ekleyin!

