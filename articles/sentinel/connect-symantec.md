---
title: Symantec ICDx verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Symantec ICDx verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d068223f-395e-46d6-bb94-7ca1afd3503c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: cac63aee5f9ebf3859b138e6444e40b1e2dd30f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588102"
---
# <a name="connect-your-symantec-icdx-appliance"></a>Symantec ICDx cihazınızı bağlayın 



Symantec ICDx konektörü, tüm Symantec güvenlik çözüm günlüklerinizi Azure Sentinel'inizle kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Bu, kuruluşunuzun ağı hakkında daha fazla bilgi verir ve güvenlik işlem yeteneklerinizi geliştirir. Symantec ICDx ve Azure Sentinel arasındaki tümleştirme, REST API'den yararlanır.


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-symantec-icdx"></a>Symantec ICDx'i yapılandırma ve bağlama 

Symantec ICDx günlükleri doğrudan Azure Sentinel'e entegre edebilir ve dışa aktarabilir.

1. Microsoft Azure Sentinel (Log Analytics) iletmelerini eklemek için ICDx Yönetim Konsolu'nu açın.
2. ICDx gezinti çubuğunda **Yapılandırma'yı**tıklatın. 
3. **Yapılandırma** ekranının üst **kısmında, İlericiler'i**tıklatın.
4. **Forwarder'lar**altında, Microsoft Azure Sentinel (Log Analytics) yanında **Ekle'yi**tıklatın. 
4. Microsoft **Azure Sentinel (Log Analytics)** penceresinde **Gelişmiş'i Göster'i**tıklatın. 
5. Genişletilmiş Microsoft Azure Sentinel (Log Analytics) penceresinin üst kısmında aşağıdakileri yapın:
    -   **Adı**: En fazla 30 karakterli iletme için bir ad yazın. Benzersiz, anlamlı bir ad seçin. Bu ad, **Yapılandırma** ekranındaki iletmeler listesinde ve **Pano** ekranındaki panolarda görünür. Örneğin: Microsoft Azure Log Analytics East. Bu alan gereklidir.
    -   **Açıklaması**: İletme için bir açıklama yazın. Bu açıklama, **Yapılandırma** ekranındaki iletmeler listesinde de görünür. İlilen olay türü ve verileri incelemesi gereken grup gibi ayrıntıları ekleyin.
    -   **Başlangıç Türü**: İletme yapılandırması için başlangıç yöntemini seçin. Seçenekleriniz manuel ve otomatiktir.<br>Varsayılan otomatiktir. 
6. **Olaylar**altında, aşağıdakileri yapın: 
    - **Kaynak**: Olayları iletmek için bir veya daha fazla arşiv seçin. Etkin toplayıcı arşivlerini (Ortak Arşiv dahil), yetim toplayıcı arşivlerini (yani sildiğiniz koleksiyoncuların arşivlerini), ICDx alıcı arşivlerini veya Sistem Arşivi'ni seçebilirsiniz. <br>Varsayılan, Ortak Arşiv'dir.
      > [!NOTE]
      > ICDx alıcı arşivleri ada göre ayrı olarak listelenir. 
 
    - **Filtre**: İlerletilmesi için olayların alt kümesini belirten bir filtre ekleyin. Aşağıdakilerden birini yapın:
        - Filtre koşulu seçmek için Tür, Öznitelik, İşleç ve Değer'i tıklatın. 
        - Filtre alanında, filtre durumunuzu gözden geçirin. Doğrudan alana düzenleyebilir veya gerektiğinde silebilirsiniz.
        - Filtre durumunuza eklemek için AND veya OR'u tıklatın.
        - Kaydedilmiş bir sorgu uygulamak için Kaydedilmiş Sorgular'ı da tıklatabilirsiniz.
    - **Dahil Öznitelikler**: İlerleyen verilere eklenecek virgüllü öznitelikler listesini yazın. Dahil edilen öznitelikler dışlanan özniteliklere göre önceliklidir.
    - **Dışlanan Öznitelikler**: İlerleyen verilerden hariç tutmak için virgülle sınırlandırılmış öznitelikler listesini yazın.
    - **Toplu İşlem Boyutu**: Toplu iş başına gönderilecek olay sayısını seçin. Seçenekleriniz 10, 50, 100, 500 ve 1000'dir.<br>Varsayılan değer 100'dür. 
    - **Oran Sınırı**: Olayların saniyedeki olay olarak ifade edilen iletildiği oranı seçin. Seçenekleriniz Sınırsız, 500, 1000, 5000, 10000. <br> Varsayılan değer 5000'dir. 
7. **Azure Hedefi**altında aşağıdakileri yapın: 
    - **Çalışma Alanı Kimliği**: Çalışma Alanı Kimliğini aşağıdan yapıştırın. Bu alan gereklidir.
    - **Birincil Anahtar**: Birincil Anahtarı aşağıdan yapıştırın. Bu alan gereklidir.
    - **Özel Günlük Adı**: Etkinlikleri ileteceğiniz Microsoft Azure portalı Log Analytics çalışma alanına özel günlük adını yazın. Varsayılan SymantecICDx olduğunu. Bu alan gereklidir.
8. İletme yapılandırmasını tamamlamak için *Kaydet'i* tıklatın. 
9. İlileden başlamak için **Seçenekler**altında, **Daha fazla** tıklatın ve sonra **Başlat.**
10. Symantec ICDx etkinlikleri için Log Analytics'teki ilgili şemayı kullanmak için **SymantecICDx_CL.**


## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Symantec ICDx'i Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


