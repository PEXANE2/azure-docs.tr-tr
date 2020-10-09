---
title: Azure Cosmos DB için Azure PowerShell örnekleri
description: Azure Cosmos DB ortak görevleri gerçekleştirmek için Azure PowerShell örnekleri alın
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 0c2d13ef49676917bc48dd5a2e682471558567f3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842980"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure PowerShell örnekleri

Aşağıdaki tabloda, Azure Cosmos DB için yaygın olarak kullanılan Azure PowerShell betiklerin bağlantıları yer almaktadır. API 'ye özel örneklere gitmek için sağdaki bağlantıları kullanın. Ortak örnekler tüm API 'lerde aynıdır. Tüm Azure Cosmos DB PowerShell cmdlet 'lerinin başvuru sayfaları [Azure PowerShell başvurusunda](/powershell/module/az.cosmosdb)bulunabilir. Lütfen düzenli olarak güncelleştirmeleri denetleyin `Az.CosmosDB` . GitHub depomızdan Cosmos DB için bu PowerShell örneklerini de çatalla Cosmos DB, [GitHub 'Da PowerShell örnekleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb)' ni kullanabilirsiniz.

## <a name="common-samples"></a>Ortak örnekler

|Görev | Açıklama |
|---|---|
|[Hesap güncelleştirme](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının varsayılan tutarlılık düzeyini güncelleştirin. |
|[Hesabın bölgelerini güncelleştirme](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cosmos DB hesabının bölgelerini güncelleştirin. |
|[Yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabının bölgesel yük devretme önceliğini değiştirin veya el ile yük devretme tetikleyin. |
|[Hesap anahtarları veya bağlantı dizeleri](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Birincil ve ikincil anahtarları, bağlantı dizelerini alın veya Azure Cosmos DB hesabının hesap anahtarını yeniden oluşturun. |
|[IP güvenlik duvarı ile Cosmos hesabı oluşturma](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı oluşturun. |
|||

## <a name="core-sql-api-samples"></a>Çekirdek (SQL) API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos DB bir hesap, veritabanı ve kapsayıcı oluşturun. |
|[Otomatik ölçeklendirme ile hesap, veritabanı ve kapsayıcı oluşturma](scripts/powershell/sql/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile Azure Cosmos DB bir hesap, veritabanı ve kapsayıcı oluşturun. |
|[Büyük bölüm anahtarı ile bir kapsayıcı oluşturma](scripts/powershell/sql/create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Büyük bölüm anahtarı ile bir kapsayıcı oluşturun. |
|[Dizin ilkesi olmadan kapsayıcı oluşturma](scripts/powershell/sql/create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Dizin ilkesi kapalıyken bir Azure Cosmos kapsayıcısı oluşturun.|
|[Veritabanları veya kapsayıcıları listeleme veya edinme](scripts/powershell/sql/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanı veya kapsayıcıları listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/sql/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya kapsayıcı için, otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/sql/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||

## <a name="cassandra-api-samples"></a>Cassandra API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, anahtar alanı ve tablo oluşturma](scripts/powershell/cassandra/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, anahtar alanı ve tablo oluşturur. |
|[Otomatik ölçeklendirme ile hesap, anahtar alanı ve tablo oluşturma](scripts/powershell/cassandra/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, anahtar alanı ve tablo oluşturur. |
|[Anahtar alanları veya tabloları listeleme veya edinme](scripts/powershell/cassandra/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Anahtar alanları veya tabloları listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/cassandra/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Anahtar uzayı veya tablo için, otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/cassandra/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||

## <a name="mongo-db-api-samples"></a>Mongo DB API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, veritabanı ve koleksiyon oluşturma](scripts/powershell/mongodb/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturur. |
|[Otomatik ölçeklendirme ile hesap, veritabanı ve koleksiyon oluşturma](scripts/powershell/mongodb/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, veritabanı ve koleksiyonu oluşturur. |
|[Veritabanları veya koleksiyonları listeleme veya edinme](scripts/powershell/mongodb/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanını veya toplamayı listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/mongodb/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya koleksiyon için, otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/mongodb/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||

## <a name="gremlin-api-samples"></a>Gremlin API örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap, veritabanı ve grafik oluşturma](scripts/powershell/gremlin/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı, veritabanı ve grafik oluşturur. |
|[Otomatik ölçeklendirme ile hesap, veritabanı ve grafik oluşturma](scripts/powershell/gremlin/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ile bir Azure Cosmos hesabı, veritabanı ve grafik oluşturur. |
|[Veritabanları veya grafikleri listeleme veya edinme](scripts/powershell/gremlin/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Veritabanını veya grafiği listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/gremlin/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir veritabanı veya grafiğe yönelik işleme, otomatik ölçeklendirme ve standart verimlilik arasında Get, Update ve Migrate işlemleri dahildir. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/gremlin/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||

## <a name="table-api-samples"></a>Tablo API'si örnekleri

|Görev | Açıklama |
|---|---|
|[Hesap ve tablo oluşturma](scripts/powershell/table/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Azure Cosmos hesabı ve tablosu oluşturur. |
|[Otomatik ölçeklendirme ile hesap ve tablo oluşturma](scripts/powershell/table/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Azure Cosmos hesabı ve tablo otomatik ölçeklendirmeyi oluşturur. |
|[Tabloları listeleme veya al](scripts/powershell/table/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tabloları listeleyin veya alın. |
|[Aktarım hızı işlemleri](scripts/powershell/table/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Otomatik ölçeklendirme ve standart verimlilik arasında al, Güncelleştir ve geçir dahil olmak üzere bir tablo için işleme işlemleri. |
|[Kaynakları silinmeye karşı kilitle](scripts/powershell/table/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Kaynakların kaynak kilitleri ile silinmesini engelleyin. |
|||
