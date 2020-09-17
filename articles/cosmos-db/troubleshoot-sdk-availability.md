---
title: Multiregional ortamlarında Azure Cosmos SDK 'larının kullanılabilirliğini tanılama ve sorunlarını giderme
description: Çoklu bölgesel ortamlarda çalışırken Azure Cosmos SDK kullanılabilirlik davranışı hakkında bilgi edinin.
author: ealsur
ms.service: cosmos-db
ms.date: 09/16/2020
ms.author: maquaran
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 0c717aca88095df05fc7927f3c3d6e2d481925d2
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709111"
---
# <a name="diagnose-and-troubleshoot-the-availability-of-azure-cosmos-sdks-in-multiregional-environments"></a>Multiregional ortamlarında Azure Cosmos SDK 'larının kullanılabilirliğini tanılama ve sorunlarını giderme

Bu makalede, belirli bir bölgede veya bir bölge yük devretmesinin gerçekleştiği zaman bir bağlantı sorunu gördüğünüzde Azure Cosmos SDK 'larının en son sürümünün davranışı açıklanmaktadır.

Tüm Azure Cosmos SDK 'Ları, bölgesel tercihi özelleştirmek için size bir seçenek sunar. Aşağıdaki özellikler farklı SDK 'larda kullanılır:

* .NET v2 SDK 'daki [Connectionpolicy. PreferredLocations](/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations) özelliği.
* .NET v3 SDK 'daki [Cosmosclientoptions. ApplicationRegion](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion) veya [Cosmosclientoptions. applicationpreferredregion](/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationpreferredregions) özellikleri.
* Java v4 SDK 'sında [Cosmosclientbuilder. Preferredregion](/java/api/com.azure.cosmos.cosmosclientbuilder.preferredregions) yöntemi.
* Python SDK 'daki [CosmosClient. preferred_locations](/python/api/azure-cosmos/azure.cosmos.cosmos_client.cosmosclient) parametresi.

Tek bir yazma bölgesi hesabında, tüm yazma işlemleri her zaman yazma bölgesine gider, bu nedenle tercih edilen bölgeler listesi yalnızca okuma işlemleri için geçerlidir. Birden fazla yazma bölgesi hesabı için, tercih listesi okuma ve yazma işlemlerini etkiler.

Tercih edilen bölge ayarlamazsanız, bölgesel tercih sırası [Azure Cosmos DB bölge listesi sırası](distribute-data-globally.md)tarafından tanımlanır.

Aşağıdaki senaryolardan herhangi biri gerçekleştiğinde, Azure Cosmos SDK 'sını kullanan istemci günlükleri kullanıma sunar ve **işlem tanılama bilgilerinin**bir parçası olarak yeniden deneme bilgilerini içerir.

## <a name="removing-a-region-from-the-account"></a><a id="remove region"></a>Hesaptan bölge kaldırma

Bir Azure Cosmos hesabından bölge kaldırdığınızda, hesabı etkin olarak kullanan herhangi bir SDK istemcisi, bir arka uç yanıt kodu aracılığıyla bölge kaldırma işlemini algılar. İstemci daha sonra bölgesel uç noktasını kullanılamaz olarak işaretler. İstemci geçerli işlemi yeniden dener ve gelecekteki tüm işlemler, tercih sırasına göre bir sonraki bölgeye kalıcı olarak yönlendirilir.

## <a name="adding-a-region-to-an-account"></a>Hesaba bölge ekleme

Azure Cosmos SDK istemcisi, 5 dakikada bir, hesap yapılandırmasını okur ve farkında olan bölgeleri yeniler.

Bir bölgeyi kaldırır ve daha sonra yeniden hesaba eklerseniz, eklenen bölgenin daha yüksek bir tercihi varsa SDK bu bölgeyi kalıcı olarak kullanacak şekilde geri dönecektir. Eklenen bölge algılandıktan sonra, gelecekteki tüm istekler buna yönlendirilir.

İstemcisini tercihen Azure Cosmos hesabının sahip olmadığı bir bölgeye bağlamak üzere yapılandırırsanız, tercih edilen bölge yok sayılır. Bu bölgeyi daha sonra eklerseniz, istemci bunu algılar ve kalıcı olarak bu bölgeye geçer.

## <a name="failover-the-write-region-in-a-single-write-region-account"></a><a id="manual-failover-single-region"></a>Tek bir yazma bölgesi hesabındaki yazma bölgesinin yükünü devretme

Geçerli yazma bölgesinin yük devretmesini başlatırsanız, sonraki yazma isteği bilinen bir arka uç yanıtıyla başarısız olur. Bu yanıt algılandığında, istemci yeni yazma bölgesini öğrenmekte olan hesabı sorgular ve geçerli işlemi yeniden denemeye devam eder ve gelecekteki tüm yazma işlemlerini yeni bölgeye kalıcı olarak yönlendirir.

## <a name="regional-outage"></a>Bölgesel kesinti

Hesap tek bir yazma bölgedeyse ve bir yazma işlemi sırasında bölgesel kesinti oluşursa, davranış [el ile yük devretmeyle](#manual-failover-single-region)benzerdir. Okuma istekleri veya birden çok yazma bölgesi hesabı için, davranış [bir bölgeyi kaldırmaya](#remove region)benzerdir.

## <a name="session-consistency-guarantees"></a>Oturum tutarlılığı garantisi

[Oturum tutarlılığı](consistency-levels.md#guarantees-associated-with-consistency-levels)kullanılırken, istemcinin kendi yazma işlemlerini okuyabileceğinizin garantisi vardır. Okuma bölgesi tercihinin yazma bölgesinden farklı olduğu tek bir yazma bölgesi hesabında, kullanıcının bir yazma yaptığını ve yerel bir bölgeden okuma yaparken, yerel bölge henüz veri çoğaltmasını (hafif kısıtlama hızı kısıtlaması) almamış olabilir. Bu gibi durumlarda SDK, okuma işleminde belirli bir hatayı algılar ve oturum tutarlılığını sağlamak için hub bölgesindeki okumayı yeniden dener.

## <a name="transient-connectivity-issues-on-tcp-protocol"></a>TCP protokolünde geçici bağlantı sorunları

Azure Cosmos SDK istemcisinin, belirli bir istek için TCP protokolünü kullanacak şekilde yapılandırıldığı senaryolarda, ağ koşullarının belirli bir uç nokta ile iletişimi geçici olarak etkilediği durumlar olabilir. Bu geçici ağ koşulları, TCP zaman aşımları olarak yüzeysel olabilir. İstemci, isteği bir saniye boyunca aynı uç noktada yerel olarak yeniden deneyecek.

Kullanıcı, birden fazla bölgeyle bir tercih edilen bölge listesi yapılandırmışsa ve Azure Cosmos hesabı birden fazla yazma bölgesi veya tek yazma bölgesidir ve işlem bir okuma isteği ise, istemci, sonraki bölgedeki tek işlemi tercih listesinden yeniden dener.

## <a name="next-steps"></a>Sonraki adımlar

* En son [.NET SDK 'sını](sql-api-sdk-dotnet-standard.md) kullanma
* En son [Java SDK 'sını](sql-api-sdk-java-v4.md) kullanma
* En son [Python SDK 'sını](sql-api-sdk-python.md) kullanma
* En son [node SDK 'sını](sql-api-sdk-node.md) kullanma
