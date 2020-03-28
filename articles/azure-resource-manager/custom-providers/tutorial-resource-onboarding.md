---
title: Öğretici - kaynak onboarding
description: Özel sağlayıcılar aracılığıyla binilen kaynak, varolan Azure kaynaklarını işlemenize ve genişletmenize olanak tanır.
ms.topic: tutorial
ms.author: jobreen
author: jjbfour
ms.date: 09/17/2019
ms.openlocfilehash: 22d1dcd997a4ddb94aba184c5dace4c00509054d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75649942"
---
# <a name="tutorial-resource-onboarding-with-azure-custom-providers"></a>Öğretici: Azure Özel Sağlayıcıları ile biniş kaynağı

Bu öğreticide, Microsoft.CustomProviders/associations kaynak türüyle Azure Kaynak Yöneticisi API'sini genişleten özel bir kaynak sağlayıcısını Azure'a dağıtırsınız. Öğretici, özel sağlayıcı örneğinin bulunduğu kaynak grubunun dışında olan varolan kaynakların nasıl genişletileceğigösterilmektedir. Bu öğreticide, özel kaynak sağlayıcısı bir Azure mantık uygulaması tarafından desteklenmektedir, ancak herhangi bir genel API bitiş noktasını kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları bilmeniz gerekir:

* Azure Özel [Sağlayıcılarının](overview.md)özellikleri.
* [Özel sağlayıcılarla onboarding kaynak](concepts-resource-onboarding.md)hakkında temel bilgiler.

## <a name="get-started-with-resource-onboarding"></a>Kaynak onboarding ile başlayın

Bu öğreticide, dağıtılması gereken iki parça vardır: özel sağlayıcı ve ilişkilendirme. İşlemi kolaylaştırmak için isteğe bağlı olarak her ikisini de dağıtan tek bir şablon kullanabilirsiniz.

Şablon bu kaynakları kullanır:

* Microsoft.CustomProviders/resourceProviders
* Microsoft.Logic/iş akışları
* Microsoft.CustomProviders/associations

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

Şablonun ilk bölümü özel sağlayıcı altyapısını dağıtir. Bu altyapı, ilişkilendirme kaynağının etkisini tanımlar. Özel sağlayıcıları bilmiyorsanız, [Özel sağlayıcı temellerine](overview.md)bakın.

Özel sağlayıcı altyapısını dağıtalım. Önceki şablonu kopyalayın, kaydedin ve dağıtın veya Azure portalını kullanarak altyapıyı takip edin ve dağıtın.

1. [Azure portalına](https://portal.azure.com)gidin.

2. **Tüm Hizmetler'de** veya ana arama kutusunu kullanarak **şablonları** arayın:

   ![Şablon arama](media/tutorial-resource-onboarding/templates.png)

3. **Şablonlar** bölmesine **Ekle'yi** seçin:

   ![Ekle’yi seçin](media/tutorial-resource-onboarding/templatesadd.png)

4. **Genel**altında, yeni şablon için bir **Ad** ve **Açıklama** girin:

   ![Şablon adı ve açıklaması](media/tutorial-resource-onboarding/templatesdescription.png)

5. Bu makalenin "Kaynak onboarding ile başlayın" bölümünden JSON şablonunda kopyalayarak Kaynak Yöneticisi şablonu oluşturun:

   ![Kaynak Yöneticisi şablonu oluşturma](media/tutorial-resource-onboarding/templatesarmtemplate.png)

6. Şablonu oluşturmak için **Ekle'yi** seçin. Yeni şablon görünmüyorsa, **Yenile'yi**seçin.

7. Yeni oluşturulan şablonu seçin ve sonra **Dağıt'ı**seçin:

   ![Yeni şablonu seçin ve sonra Dağıt'ı seçin](media/tutorial-resource-onboarding/templateselectspecific.png)

8. Gerekli alanların ayarlarını girin ve ardından abonelik ve kaynak grubunu seçin. Özel Kaynak **Sağlayıcı Kimlik** kutusunu boş bırakabilirsiniz.

   | Ayar adı | Gerekli mi? | Açıklama |
   | ------------ | -------- | ----------- |
   | Konum | Evet | Şablondaki kaynakların konumu. |
   | Mantık Uygulama Adı | Hayır | Mantık uygulamasının adı. |
   | Özel Kaynak Sağlayıcı Adı | Hayır | Özel kaynak sağlayıcı adı. |
   | Özel Kaynak Sağlayıcı Kimliği | Hayır | İlişkilendirme kaynağını destekleyen varolan bir özel kaynak sağlayıcısı. Burada bir değer belirtirseniz, mantık uygulaması ve özel sağlayıcı dağıtımı atlanır. |
   | Dernek Adı | Hayır | İlişkilendirme kaynağının adı. |

   Örnek parametreler:

   ![Şablon parametrelerini girin](media/tutorial-resource-onboarding/templatescustomprovider.png)

9. Dağıtıma gidin ve bitmesini bekleyin. Aşağıdaki ekran görüntüsü gibi bir şey görmeniz gerekir. Yeni ilişkilendirme kaynağını çıktı olarak görmeniz gerekir:

   ![Başarılı dağıtım](media/tutorial-resource-onboarding/customproviderdeployment.png)

   Gizli **türleri göster'in** seçildiği kaynak grubu aşağıda verilmiştir:

   ![Özel sağlayıcı dağıtımı](media/tutorial-resource-onboarding/showhidden.png)

10. İlişkilendirme çağrılarının oluşturduğunu görmek için mantık uygulamasını **çalışır tarih** sekmesini keşfedin:

    ![Mantık uygulaması Tarihi çalıştırAn](media/tutorial-resource-onboarding/logicapprun.png)

## <a name="deploy-additional-associations"></a>Ek çağrışımlar dağıtma

Özel sağlayıcı altyapısını ayarladıktan sonra, daha fazla ilişkilendirme yi kolayca dağıtabilirsiniz. Ek ilişkilendirmeler için kaynak grubunun, özel sağlayıcı altyapısını dağıttığınız kaynak grubuyla aynı olması gerekir. Bir ilişkilendirme oluşturmak için, belirtilen Özel Kaynak Sağlayıcı Kimliği'nde Microsoft.CustomProviders/resourceproviders/write izinlerine sahip olmanız gerekir.

1. Önceki dağıtımın kaynak grubunda özel sağlayıcı **Microsoft.CustomProviders/resourceProviders** kaynağına gidin. **Gizli türleri göster** onay kutusunu seçmeniz gerekir:

   ![Kaynağa git](media/tutorial-resource-onboarding/showhidden.png)

2. Özel sağlayıcının Kaynak Kimliği özelliğini kopyalayın.

3. **Tüm Hizmetler'de** veya ana arama kutusunu kullanarak **şablonları** arayın:

   ![Şablon arama](media/tutorial-resource-onboarding/templates.png)

4. Önceden oluşturulmuş şablonu seçin ve sonra **Dağıt'ı**seçin:

   ![Önceden oluşturulmuş şablonu seçin ve sonra Dağıt'ı seçin](media/tutorial-resource-onboarding/templateselectspecific.png)

5. Gerekli alanların ayarlarını girin ve ardından aboneliği ve farklı bir kaynak grubunu seçin. Özel **Kaynak Sağlayıcı Kimliği** ayarı için, daha önce dağıttığınız özel sağlayıcıdan kopyaladığınız Kaynak Kimliği'ni girin.

6. Dağıtıma gidin ve bitmesini bekleyin. Şimdi yalnızca yeni ilişkilendirmeler kaynağını dağıtmalıdır:

   ![Yeni ilişkilendirme kaynağı](media/tutorial-resource-onboarding/createdassociationresource.png)

İsterseniz, mantık uygulaması **Run geçmişine** geri dönebilir ve mantık uygulamasına başka bir arama yapıldığını görebilirsiniz. Oluşturulan her ilişkilendirme için ek işlevselliği artırmak için mantık uygulamasını güncelleştirebilirsiniz.

## <a name="getting-help"></a>Yardım alma

Azure Özel Sağlayıcıları hakkında sorularınız varsa, [Bunları Yığın Taşma'da](https://stackoverflow.com/questions/tagged/azure-custom-providers)sormayı deneyin. Benzer bir soru zaten yanıtlanmış olabilir, bu nedenle göndermeden önce önce kontrol edin. Hızlı yanıt `azure-custom-providers` almak için etiketi ekleyin!

