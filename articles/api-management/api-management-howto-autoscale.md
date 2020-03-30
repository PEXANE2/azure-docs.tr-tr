---
title: Azure API Yönetimi örneğinin otomatik ölçeğini yapılandırma | Microsoft Dokümanlar
description: Bu konu, Bir Azure API Yönetimi örneği için otomatik ölçeklendirme davranışının nasıl ayarlanır olduğunu açıklar.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 8c1c96fdb1f4f42c7592791881b855f74d411171
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70018280"
---
# <a name="automatically-scale-an-azure-api-management-instance"></a>Bir Azure API Management örneğini otomatik olarak ölçeklendirme  

Azure API Yönetimi hizmet örneği, bir dizi kurala göre otomatik olarak ölçeklenebilir. Bu davranış Azure Monitor aracılığıyla etkinleştirilebilir ve yapılandırılabilir ve yalnızca Azure API Yönetimi hizmetinin **Standart** ve **Premium** katmanlarında desteklenebilir.

Makale, otomatik ölçek yapılandırma sürecinde yürür ve otomatik ölçek kurallarının en iyi yapılandırmaönerir.

> [!NOTE]
> **Tüketim** katmanındaki API Yönetimi hizmeti, herhangi bir ek yapılandırma gerekmeden trafiğe göre otomatik olarak ölçeklendirilir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemek için şunları

+ Etkin bir Azure aboneliğine sahip olun.
+ Azure API Yönetimi örneğine sahip olun. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)
+ [Azure API Yönetimi örneğinin Kapasite](api-management-capacity.md)kavramını anlayın.
+ Maliyet sonuçları da dahil olmak üzere [bir Azure API Yönetimi örneğinin el ile ölçekleme işlemini](upgrade-and-scale.md)anlayın.

[!INCLUDE [premium-standard.md](../../includes/api-management-availability-premium-standard.md)]

## <a name="azure-api-management-autoscale-limitations"></a>Azure API Yönetimi otomatik ölçeklendirme sınırlamaları

Otomatik ölçeklendirme davranışını yapılandırmadan önce ölçeklendirme kararlarının belirli sınırlamaları ve sonuçlarının göz önünde bulundurulması gerekir.

+ Otomatik ölçeklendirme yalnızca Azure API Yönetimi hizmetinin **Standart** ve **Premium** katmanları için etkinleştirilebilir.
+ Fiyatlandırma katmanları, bir hizmet örneği için en fazla birim sayısını da belirtir.
+ Ölçekleme işlemi en az 20 dakika sürer.
+ Hizmet başka bir işlem tarafından kilitlenirse, ölçekleme isteği başarısız olur ve otomatik olarak yeniden dener.
+ Çok bölgesel dağıtımlara sahip bir hizmet olması durumunda, yalnızca **Birincil konumdaki** birimler ölçeklenebilir. Diğer konumlardaki birimler ölçeklendirilemiyor.

## <a name="enable-and-configure-autoscale-for-azure-api-management-service"></a>Azure API Yönetimi hizmeti için otomatik ölçeklendirmeyi etkinleştirme ve yapılandırma

Azure API Yönetimi hizmeti için otomatik ölçeklendirmeyi yapılandırmak için aşağıdaki adımları izleyin:

1. Azure portalındaki **Örnek'i İzle'ye** gidin.

    ![Azure İzleyici](media/api-management-howto-autoscale/01.png)

2. Soldaki menüden **Otomatik Ölçek'i** seçin.

    ![Azure Monitör otomatik ölçeklendirme kaynağı](media/api-management-howto-autoscale/02.png)

3. Açılan menüdeki filtrelere göre Azure API Yönetimi hizmetinizi bulun.
4. İstenilen Azure API Yönetimi hizmet örneğini seçin.
5. Yeni açılan bölümde, **otomatik ölçeklendir** düğmesini etkinleştir'i tıklatın.

    ![Azure Monitör otomatik ölçeklendirme etkinleştirme](media/api-management-howto-autoscale/03.png)

6. **Kurallar** bölümünde , **+ Kural ekle'yi**tıklatın.

    ![Azure Monitör otomatik ölçek ekleme kuralı](media/api-management-howto-autoscale/04.png)

7. Yeni bir ölçek çıkış kuralı tanımlayın.

   Örneğin, bir ölçeklendirme kuralı, son 30 dakikadaki ortalama kapasite ölçümü %80'i aştığında bir Azure API Yönetimi biriminin eklenmesini tetikleyebilir. Aşağıdaki tabloda böyle bir kural için yapılandırma sağlar.

    | Parametre             | Değer             | Notlar                                                                                                                                                                                                                                                                           |
    |-----------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Ölçüm kaynağı         | Geçerli kaynak  | Geçerli Azure API Yönetimi kaynak ölçümlerini temel alan kuralı tanımlayın.                                                                                                                                                                                                     |
    | *Ölçütler*            |                   |                                                                                                                                                                                                                                                                                 |
    | Toplam süre      | Ortalama           |                                                                                                                                                                                                                                                                                 |
    | Ölçüm adı           | Kapasite          | Kapasite ölçümü, bir Azure API Yönetimi örneğinin kaynaklarının kullanımını yansıtan bir Azure API Yönetimi ölçüsüdür.                                                                                                                                                            |
    | Zaman dilimi istatistiği  | Ortalama           |                                                                                                                                                                                                                                                                                 |
    | İşleç              | Büyüktür      |                                                                                                                                                                                                                                                                                 |
    | Eşik             | %80               | Ortalama kapasite ölçümü için eşik.                                                                                                                                                                                                                                 |
    | Süre (dakika) | 30                | Kapasite ölçümünün ortalamasına göre zaman alameti kullanım kalıplarına özgüdür. Süre ne kadar uzun olursa, reaksiyon o kadar yumuşak olur - aralıklı ani artışlar ölçeklendirme kararı üzerinde daha az etkiye sahip olacaktır. Ancak, aynı zamanda ölçeklendirme tetikleyici geciktirecektir. |
    | *Eylem*              |                   |                                                                                                                                                                                                                                                                                 |
    | İşlem             | Sayıyı şu kadar artır |                                                                                                                                                                                                                                                                                 |
    | Örnek sayısı        | 1                 | Azure API Yönetimi örneğini 1 birim olarak ölçeklendirin.                                                                                                                                                                                                                          |
    | Soğuma (dakika)   | 60                | Azure API Yönetimi hizmetinin ölçeklendirmesi en az 20 dakika sürer. Çoğu durumda, 60 dakikalık soğuma süresi birçok ölçek çıkışını tetiklemesini önler.                                                                                                  |

8. Kuralı kaydetmek için **Ekle'yi** tıklatın.

    ![Azure Monitörü ölçeği kuralı](media/api-management-howto-autoscale/05.png)

9. + **Kural ekle'ye**tekrar tıklayın.

    Bu kez, kural bir ölçek tanımlanması gerekir. API kullanımı azaldığında kaynakların boşa harcanmasını sağlayacaktır.

10. Kuralda yeni bir ölçek tanımlayın.

    Örneğin, kurallı bir ölçek, son 30 dakikadaki ortalama kapasite ölçümü %35'in altında olduğunda, bir Azure API Yönetimi biriminin kaldırılmasını tetikleyebilir. Aşağıdaki tabloda böyle bir kural için yapılandırma sağlar.

    | Parametre             | Değer             | Notlar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    |-----------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Ölçüm kaynağı         | Geçerli kaynak  | Geçerli Azure API Yönetimi kaynak ölçümlerini temel alan kuralı tanımlayın.                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | *Ölçütler*            |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Toplam süre      | Ortalama           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Ölçüm adı           | Kapasite          | Ölçek dışı kuralı için kullanılan metrikle aynı.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Zaman dilimi istatistiği  | Ortalama           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | İşleç              | Küçüktür         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Eşik             | %35               | Ölçek dışı etme kuralına benzer şekilde, bu değer büyük ölçüde Azure API Yönetimi'nin kullanım kalıplarına bağlıdır. |
    | Süre (dakika) | 30                | Ölçek dışı kuralı için kullanılan değerle aynı değer.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | *Eylem*              |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | İşlem             | Sayıyı şu kadar azalt | Ölçek dışı kural için kullanılan ın tam tersi.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Örnek sayısı        | 1                 | Ölçek dışı kuralı için kullanılan değerle aynı değer.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Soğuma (dakika)   | 90                | Ölçek bir ölçek daha muhafazakar olmalıdır, bu nedenle soğuma süresi daha uzun olmalıdır.                                                                                                                                                                                                                                                                                                                                                                                                    |

11. Kuralı kaydetmek için **Ekle'yi** tıklatın.

    ![Kuralda Azure Monitör ölçeği](media/api-management-howto-autoscale/06.png)

12. En **fazla** Azure API Yönetimi birimi sayısını ayarlayın.

    > [!NOTE]
    > Azure API Yönetimi'nin bir örnek için ölçeklendirebileceği birimlerin bir sınırı vardır. Sınır bir hizmet katmanına bağlıdır.

    ![Kuralda Azure Monitör ölçeği](media/api-management-howto-autoscale/07.png)

13. **Kaydet**'e tıklayın. Otomatik ölçekyapılandırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

+ [Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md)
