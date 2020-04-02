---
title: Azure Veri Gezgini'ndeki verileri çözümlemek için Azure Not Defterleri'ni kullanma
description: Bu konu, Azure Not Defteri kullanarak sorgu oluşturmanızı gösterir
author: orspod
ms.author: orspodek
ms.reviewer: adieldar
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 0f99e11be99f22feec73b72397b27522b90dbf49
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522413"
---
# <a name="use-azure-notebooks-to-analyze-data-in-azure-data-explorer"></a>Azure Veri Gezgini'ndeki verileri çözümlemek için Azure Not Defterleri'ni kullanma

[Azure Dizüstü Bilgisayarlar,](https://notebooks.azure.com/) [Jupyter Notebook'ları](https://jupyter.org/)oluşturmayı ve paylaşmayı kolaylaştıran bir Azure bulut hizmetidir. Azure Dizüstü Bilgisayarlar, belgeleri, kodu ve kodu çalıştırmanın sonuçlarını birleştirmeyi kolaylaştırır.

> [!Note]
> * Aşağıdaki yordam, Azure Veri Gezgini'ni sorgulamak için Azure Not Defterleri ortamındaki Python istemcisini kullanır. Ancak, Azure Veri Gezgini'ni sorgulamak için [KQLmagic'i](https://docs.microsoft.com/azure/data-explorer/kqlmagic) de kullanabilirsiniz.
> * Bazı kullanıcılar Edge kullanarak kimlik doğrulama sorunları bildirdi; Bu tür sorunlar çözülene kadar farklı bir tarayıcı kullanın.

## <a name="create-a-project"></a>Proje oluşturma

1. [Azure Not Defterlerini](https://notebooks.azure.com/) tarayıcınızda açın ve oturum açın.

1. Üstbilgide **Projelerim** sekmesini seçin. 

    [![](media/azurenotebooks/an-myprojects.png "My projects")](media/azurenotebooks/an-myprojects.png#lightbox)

1. Select **+ Yeni Projeler**.
    
1. Yeni **Proje Oluştur** iletişim kutusunda:
    1. Proje **Adını**girin.
    1. **Genel** onay kutusunu temizleyin.
        >[!Important]
        > Genel onay kutusunu temizlemezseniz, projeniz açık Internet'te açık olarak açıkta kalır.
    1. **Oluştur'u**seçin.
    
    ![Yeni bir proje oluşturma](media/azurenotebooks/an-create-new-project-blank.png)

    Proje Kimliği otomatik olarak oluşturulur.

## <a name="create-a-notebook"></a>Not defteri oluşturma

1. Yeni projenizde bir not defteri oluşturun. Not defteri desteklenen bir [dil](https://github.com/Azure/azure-kusto-python#minimum-requirements)kullanmalıdır.
Not defteri oluşturmak için **+ Yeni'yi** seçin ve **Not Defteri'ni**seçin.

    ![Yeni not defteri oluşturma](media/azurenotebooks/an-create-new-notebook-menu.png) 

1. Yeni **Not Defteri Oluştur** iletişim kutusuna not defteri adını girin.

1. **Python 3.6'yı** seçin ve **Yeni'yi**seçin.
    
    ![Yeni not defteri oluşturma](media/azurenotebooks/an-create-new-notebook.png) 
    
1. Projenizde yeni not defterinizi seçin.

    ![Yeni not defteri seçin](media/azurenotebooks/an-select-notebook.png)

    Python çekirdeğiniz inişe geçmesini bekleyin. Doldurulmuş daire simgesi içi boş bir daire simgesine değiştiğinde, devam edebilirsiniz.

    ![Çekirdek baş harflerini](media/azurenotebooks/an-python-init-icon.png)

1. Sistem modülünü almak için aşağıdaki komutları girin ve **Çalıştır'ı**seçin:
    ```python
    import sys
    sys.version
    ```

    > [!Note]
    > Bir hücreyi çalıştırmak için Shift+Enter tuşuna da basabilirsiniz.

1.  SDK sınıflarını almak için aşağıdaki komutları girin ve **Çalıştır'ı**seçin:
    ```python
    from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
    ```

1.  Bağlantı dizesini oluşturmak ve Kusto istemcisini başlatmak için aşağıdaki komutları girin ve **Çalıştır'ı**seçin:  
    ```python
    kcsb = KustoConnectionStringBuilder.with_aad_device_authentication("https://help.kusto.windows.net")
    kc = KustoClient(kcsb)
    kc.execute("Samples", ".show version")
    ```
1. İsteyden, [https://aka.ms/devicelogin](https://aka.ms/devicelogin)yeni bir tarayıcı sekmesini açın. 
   
1. Yetkilendirme kodunu girin ve AAD (@microsoft.com) hesabınızı oturum açın. Kusto istemcisi `kc` artık kimliğinizi kullanarak Azure Veri Gezgini'ne kimlik doğrulayabilir.

1. Kimlik doğrulamanın sonucunu görmek için not defterinize dönün. 

[![](media/azurenotebooks/an-python-commands.png "Python commands")](media/azurenotebooks/an-python-commands.png#lightbox)

## <a name="execute-a-kusto-query"></a>Bir Kusto sorgusu yürütme

1. Kusto sorgunuzu girin ve **Çalıştır'ı**seçin. Örnek:

    ```python
    query= "StormEvents | project State, EventType | take 10"
    response = kc.execute("Samples", query)
    for row in response.primary_results[0]:
        print(", ".join(row))
    ```    

[![](media/azurenotebooks/an-commands.png "Python commands")](media/azurenotebooks/an-commands.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [Kusto-python GitHub repo](https://github.com/Azure/azure-kusto-python)
