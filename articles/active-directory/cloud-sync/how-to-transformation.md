---
title: Azure AD Connect bulut eşitleme dönüşümleri
description: Bu makalede, varsayılan öznitelik eşlemelerini değiştirmek için dönüşümlerin nasıl kullanılacağı açıklanır.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: how-to
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1c81ee0ebace65e50cdab5b5b223d5e5eae4ff9a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98614064"
---
# <a name="transformations"></a>Dönüşümler

Bir dönüşümle, bir özniteliğin bulut eşitlemesini kullanarak Azure Active Directory (Azure AD) ile nasıl eşitlendiğine ilişkin varsayılan davranışı değiştirebilirsiniz.

Bu görevi yapmak için şemayı düzenlemeniz ve bir Web isteği aracılığıyla yeniden göndermeniz gerekir.

Bulut eşitleme öznitelikleri hakkında daha fazla bilgi için bkz. [Azure AD şemasını anlama](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Şemayı alma
Şemayı almak için [Şemayı görüntüleme](concept-attributes.md#view-the-schema)bölümündeki adımları izleyin. 

## <a name="custom-attribute-mapping"></a>Özel öznitelik eşleme
Özel bir öznitelik eşlemesi eklemek için aşağıdaki adımları izleyin.

1. Şemayı, [Visual Studio Code](https://code.visualstudio.com/)gibi bir metin veya kod düzenleyicisine kopyalayın.
1. Şemada güncelleştirmek istediğiniz nesneyi bulun.

   ![Şemadaki nesne](media/how-to-transformation/transform-1.png)</br>
1. `ExtensionAttribute3`Kullanıcı nesnesinin altındaki kodunu bulun.

    ```
                            {
                                "defaultValue": null,
                                "exportMissingReferences": false,
                                "flowBehavior": "FlowWhenChanged",
                                "flowType": "Always",
                                "matchingPriority": 0,
                                "targetAttributeName": "ExtensionAttribute3",
                                "source": {
                                    "expression": "Trim([extensionAttribute3])",
                                    "name": "Trim",
                                    "type": "Function",
                                    "parameters": [
                                        {
                                            "key": "source",
                                            "value": {
                                                "expression": "[extensionAttribute3]",
                                                "name": "extensionAttribute3",
                                                "type": "Attribute",
                                                "parameters": []
                                            }
                                        }
                                    ]
                                }
                            },
    ```
1. Şirket özniteliğinin eşlendiği şekilde kodu düzenleyin `ExtensionAttribute3` .

   ```
                                    {
                                        "defaultValue": null,
                                        "exportMissingReferences": false,
                                        "flowBehavior": "FlowWhenChanged",
                                        "flowType": "Always",
                                        "matchingPriority": 0,
                                        "targetAttributeName": "ExtensionAttribute3",
                                        "source": {
                                            "expression": "Trim([company])",
                                            "name": "Trim",
                                            "type": "Function",
                                            "parameters": [
                                                {
                                                    "key": "source",
                                                    "value": {
                                                        "expression": "[company]",
                                                        "name": "company",
                                                        "type": "Attribute",
                                                        "parameters": []
                                                    }
                                                }
                                            ]
                                        }
                                    },
   ```
 1. Şemayı Graph Explorer 'a geri kopyalayın, **Istek türünü** **koymak** Için değiştirin ve **Sorguyu Çalıştır**' ı seçin.

    ![Sorguyu Çalıştır](media/how-to-transformation/transform-2.png)

 1. Artık Azure portal, bulut eşitleme yapılandırması ' na gidin ve **sağlamayı yeniden Başlat**' ı seçin.

    ![Sağlamayı yeniden Başlat](media/how-to-transformation/transform-3.png)

 1. Biraz sonra, Graph Explorer 'da aşağıdaki sorguyu çalıştırarak özniteliklerin doldurulduğundan emin olun: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}` .
 1. Şimdi değeri görmeniz gerekir.

    ![Değer görünür](media/how-to-transformation/transform-4.png)

## <a name="custom-attribute-mapping-with-function"></a>İşlev ile özel öznitelik eşleme
Daha gelişmiş eşleme için, verileri işlemenizi ve kuruluşunuzun ihtiyaçlarına uyacak şekilde özniteliklerin değerlerini oluşturmayı sağlayan işlevleri kullanabilirsiniz.

Bu görevi yapmak için önceki adımları izleyin ve son değeri oluşturmak için kullanılan işlevi düzenleyin.

Sözdizimi ve ifade örnekleri hakkında daha fazla bilgi için, bkz. [Azure Active Directory öznitelik eşlemeleri için Ifadeler yazma](reference-expressions.md).


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut eşitlemesi nedir?](what-is-cloud-sync.md)
