---
title: Azure AD Connect bulut sağlama dönüşümleri
description: Bu belgede, varsayılan öznitelik eşlemelerini değiştirmek için dönüşümlerin nasıl kullanılacağı açıklanmaktadır.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d37fdb4ad0d385914aecd4ca62be498c5c0e7c5
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794477"
---
# <a name="transformations"></a>Dönüşümler

Bir dönüşüm, bir özniteliğin bulut sağlaması kullanılarak Azure AD ile nasıl eşitlendiğine ilişkin varsayılan davranışı değiştirmenize izin verir.  

Bu görevi yapmak için şemayı düzenlemeniz ve bir Web isteği aracılığıyla yeniden göndermeniz gerekir.

Bulut sağlama öznitelikleri hakkında daha fazla bilgi için bkz. [Azure AD şemasını anlama](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Şemayı alma
Şemayı almak için [Şemayı görüntüleme](concept-attributes.md#viewing-the-schema)bölümünde özetlenen adımları kullanın. 


## <a name="custom-attribute-mapping"></a>Özel öznitelik eşleme
Özel bir öznitelik eşlemesi eklemek için aşağıdaki yordamı kullanın.

1. Şemayı, [Visual Studio Code](https://code.visualstudio.com/)gibi bir metin veya kod düzenleyicisine kopyalayın.  
2. Şemada güncelleştirmek istediğiniz nesneyi bulun ![](media/how-to-transformation/transform1.png)</br>
3. Kullanıcı nesnesi altındaki **ExtensionAttribute3** kodunu bulun.

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
 4.  Şirket özniteliğinin ExtensionAttribute3 ile eşlenmesi için kodu düzenleyin.
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
 5. Şemayı Graph Explorer 'a geri kopyalayın, sorgu koymak ve **çalıştırmak**Için istek türünü değiştirin.  
 ![](media/how-to-transformation/transform2.png)</br>
 6.  Artık Azure portal, bulut sağlama yapılandırmasına gidin ve **sağlama Işlemini yeniden başlatın**.
 ![](media/how-to-transformation/transform3.png)</br>
 7.  Biraz sonra, Graph Explorer 'da aşağıdaki sorguyu çalıştırarak özniteliklerin doldurulduğundan emin olun: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`.
 8.  Şimdi değeri görmeniz gerekir.
 ![](media/how-to-transformation/transform4.png)</br>

## <a name="custom-attribute-mapping-with-function"></a>İşlev ile özel öznitelik eşleme
Daha gelişmiş eşleme için, verileri işlemenizi ve kuruluşunuzun ihtiyaçlarına uyacak şekilde öznitelik için değerler oluşturmanızı sağlayacak olan işlevleri kullanabilirsiniz.

Bu görevi yapmak için yukarıdaki adımları izlemeniz ve son değeri oluşturmak için kullanılan işlevi düzenlemeniz yeterlidir.

Sözdizimi ve ifade örnekleri hakkında daha fazla bilgi için, bkz. [Azure Active Directory öznitelik eşlemeleri Için Ifadeler yazma](reference-expressions.md)


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
