---
title: Azure AD Bulut sağlama dönüşümlerini bağlayın
description: Bu makalede, varsayılan öznitelik eşlemeleri değiştirmek için dönüşümleri nasıl kullanılacağı açıklanmaktadır.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ec12927b40096b7ff04fae6b7cbc69a7bc11e8f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75549304"
---
# <a name="transformations"></a>Dönüşümler

Dönüşümle, bulut sağlama özelliğini kullanarak bir özniteliğin Azure Etkin Dizin (Azure AD) ile nasıl eşitlendirilebildiğini varsayılan olarak değiştirebilirsiniz.

Bu görevi yapmak için şemayı yeniden ve ardından bir web isteği aracılığıyla yeniden göndermeniz gerekir.

Bulut sağlama özellikleri hakkında daha fazla bilgi için azure [AD şemasını anlama](concept-attributes.md)konusuna bakın.


## <a name="retrieve-the-schema"></a>Şeayı alın
Şemayı almak için [şemayı Görüntüle'deki](concept-attributes.md#view-the-schema)adımları izleyin. 

## <a name="custom-attribute-mapping"></a>Özel öznitelik eşleme
Özel bir öznitelik eşlemi eklemek için aşağıdaki adımları izleyin.

1. Şeayı [Visual Studio Code](https://code.visualstudio.com/)gibi bir metin veya kod düzenleyicisine kopyalayın.
1. Şemada güncelleştirmek istediğiniz nesneyi bulun.

   ![Şemadaki nesne](media/how-to-transformation/transform1.png)</br>
1. Kullanıcı nesnesinin `ExtensionAttribute3` altındaki kodu bulun.

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
1. Şirket özniteliğinin eşlenecek şekilde kodu `ExtensionAttribute3`edin.

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
 1. Şemayı Graph Explorer'a geri kopyalayın, **İstek Türünü** **PUT**olarak değiştirin ve **Sorguyu Çalıştır'ı**seçin.

    ![Sorguyu Çalıştır](media/how-to-transformation/transform2.png)

 1. Şimdi, Azure portalında bulut sağlama yapılandırmasına gidin ve **yeniden başlat'ı**seçin.

    ![Sağlamayı yeniden başlatma](media/how-to-transformation/transform3.png)

 1. Bir süre sonra, Grafik Gezgini'nde aşağıdaki sorguyu çalıştırarak `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`özniteliklerin doldurulmakta olduğunu doğrulayın: .
 1. Şimdi değeri görmelisiniz.

    ![Değer görüntülenir](media/how-to-transformation/transform4.png)

## <a name="custom-attribute-mapping-with-function"></a>İşlevli özel öznitelik eşleme
Daha gelişmiş eşleme için, verileri işlemenize ve kuruluşunuzun gereksinimlerine uygun öznitelikler için değerler oluşturmanıza olanak tanıyan işlevleri kullanabilirsiniz.

Bu görevi yapmak için önceki adımları izleyin ve sonra son değeri oluşturmak için kullanılan işlevi edin.

Sözdizimi ve ifade örnekleri hakkında bilgi için bkz: [Azure Etkin Dizini'ndeki öznitelik eşlemeleri için ifadeler yazma.](reference-expressions.md)


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
