---
title: Komut satırı arabirimi-Bilgi Keşfetme Hizmeti API 'SI
titlesuffix: Azure Cognitive Services
description: Yapılandırılmış verilerden dizin ve dilbilgisi dosyaları oluşturmak için komut satırı arabirimini kullanın ve bunları Web Hizmetleri olarak dağıtın.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/24/2016
ms.author: paulhsu
ms.openlocfilehash: 018552982a8ece3bbbaea2d60e2a6e64f681f822
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78385642"
---
# <a name="command-line-interface"></a>Komut satırı arabirimi

Bilgi Keşfetme Hizmeti (KES) komut satırı arabirimi, yapılandırılmış verilerden dizin ve dilbilgisi dosyaları oluşturma ve bunları Web Hizmetleri olarak dağıtma olanağı sağlar.  Genel sözdizimini kullanır: `kes.exe <command> <required_args> [<optional_args>]`.  Komut listesini göstermek için bağımsız değişkenler olmadan `kes.exe` çalıştırabilir veya belirtilen komut için kullanılabilen bağımsız değişkenlerin bir listesini görüntüleyecek `kes.exe <command>`.  Kullanılabilir komutların listesi aşağıda verilmiştir:

* build_index
* build_grammar
* host_service
* deploy_service
* describe_index
* describe_grammar

<a name="build_index-command"></a>

## <a name="build_index-command"></a>build_index komutu

**Build_index** komutu bir şema tanım dosyasından bir ikili dizin dosyası ve dizine eklenecek nesnelerin bir veri dosyası oluşturur.  Elde edilen dizin dosyası, yapılandırılmış sorgu ifadelerini değerlendirmek ya da derlenmiş bir dilbilgisi dosyası ile birlikte doğal dil sorgularının yorumlamalarını oluşturmak için kullanılabilir.

`kes.exe build_index <schemaFile> <dataFile> <indexFile> [options]`

| Parametre      | Açıklama               |
|----------------|---------------------------|
| `<schemaFile>` | Giriş şeması yolu |
| `<dataFile>`   | Giriş verisi yolu   |
| `<indexFile>`  | Çıkış dizini yolu |
| `--description <description>` | Açıklama dizesi |
| `--remote <vmSize>`           | Uzak derleme için VM boyutu |

Bu dosyalar, Azure bloblarına yerel dosya yolları veya URL yolları tarafından belirtilebilir.  Şema dosyası, dizine eklenmekte olan nesnelerin yapısını ve desteklenecek işlemleri açıklar (bkz. [şema biçimi](SchemaFormat.md)).  Veri dosyası, dizine eklenecek nesneleri ve öznitelik değerlerini numaralandırır (bkz. [veri biçimi](DataFormat.md)).  Oluşturma işlemi başarılı olduğunda, çıkış dizini dosyası istenen işlemleri destekleyen giriş verilerinin sıkıştırılmış bir gösterimini içerir.  

Bir açıklama dizesi, daha sonra **describe_index** komutu kullanılarak bir ikili dizin tanımlamak için isteğe bağlı olarak belirtilebilir.  

Varsayılan olarak, Dizin yerel makinede oluşturulur.  Yerel derlemeler, Azure ortamının dışında 10.000 ' e kadar nesne içeren veri dosyalarıyla sınırlıdır.  --Remote bayrağı belirtildiğinde, dizin, belirtilen boyuttaki geçici olarak oluşturulan bir Azure VM 'sinde oluşturulur.  Bu, büyük dizinlerin Azure sanal makineleri ile daha fazla bellekle verimli bir şekilde oluşturulmasına olanak sağlar.  Oluşturma işlemini yavaşlatan sayfalama önlemek için, giriş veri dosyası boyutu olarak RAM miktarının 3 katı olan bir VM kullanmanızı öneririz.  Kullanılabilir VM boyutları listesi için bkz. [Sanal makinelerin boyutları](../../../articles/virtual-machines/virtual-machines-windows-sizes.md).

> [!TIP] 
> Daha hızlı derlemeler için, olasılığa göre veri dosyasındaki nesneleri önceden sıralama.

<a name="build_grammar-command"></a>

## <a name="build_grammar-command"></a>build_grammar komutu

**Build_grammar** komutu, XML 'de belirtilen bir dilbilgisini bir ikili dilbilgisi dosyasına derler.  Elde edilen dilbilgisi dosyası, doğal dil sorgularının yorumlamalarını oluşturmak için bir dizin dosyası ile birlikte kullanılabilir.

`kes.exe build_grammar <xmlFile> <grammarFile>`

| Parametre       | Açıklama               |
|-----------------|---------------------------|
| `<xmlFile>`     | Giriş XML dilbilgisi belirtim yolu |
| `<grammarFile>` | Derlenmiş çıkış dilbilgisi yolu         |

Bu dosyalar, Azure bloblarına yerel dosya yolları veya URL yolları tarafından belirtilebilir.  Dilbilgisi belirtimi, ağırlıklı doğal dil ifadeleri kümesini ve bunların anlam yorumlamalarını tanımlar (bkz. [dilbilgisi biçimi](GrammarFormat.md)).  Oluşturma işlemi başarılı olduğunda, çıkış dilbilgisi dosyası hızlı kodu çözme özelliğini etkinleştirmek için dilbilgisi belirtiminin ikili gösterimini içerir.

<a name="host_service-command"/>

## <a name="host_service-command"></a>host_service komutu

**Host_service** komutu yerel makinede kes hizmetinin bir örneğini barındırır.

`kes.exe host_service <grammarFile> <indexFile> [options]`

| Parametre       | Açıklama                |
|-----------------|----------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu         |
| `<indexFile>`   | Giriş ikili dizin yolu           |
| `--port <port>` | Yerel bağlantı noktası numarası.  Varsayılan: 8000 |

Bu dosyalar, Azure bloblarına yerel dosya yolları veya URL yolları tarafından belirtilebilir.  Web hizmeti, http://localhost:&lt;p ort&gt;/olarak barındırılır.  Desteklenen işlemlerin listesi için bkz. [Web API 'leri](WebAPI.md) .

Azure ortamının dışında, yerel olarak barındırılan hizmetler Dizin dosyalarıyla 1 MB, saniye başına 10 istek ve toplam 1000 çağrı ile sınırlıdır.  Bu sınırlamaları aşmak için, Azure VM içinde **host_service** çalıştırın veya **Deploy_service**kullanarak bir Azure bulut hizmetine dağıtın.

<a name="deploy_service-command"/>

## <a name="deploy_service-command"></a>deploy_service komutu

**Deploy_service** komutu, bir Azure bulut hizmetine kes hizmetinin bir örneğini dağıtır.

`kes.exe deploy_service <grammarFile> <indexFile> <serviceName> <vmSize>[options]`

| Parametre       | Açıklama                  |
|-----------------|------------------------------|
| `<grammarFile>` | Giriş ikili dilbilgisi yolu           |
| `<indexFile>`   | Giriş ikili dizin yolu             |
| `<serviceName>` | Hedef bulut hizmeti adı |
| `<vmSize>`      | Bulut hizmeti VM 'sinin boyutu     |
| `--slot <slot>` | Bulut hizmeti yuvası: "hazırlama" (varsayılan), "üretim" |

Bu dosyalar, Azure bloblarına yerel dosya yolları veya URL yolları tarafından belirtilebilir.  Hizmet adı, önceden yapılandırılmış bir Azure bulut hizmetini belirtir (bkz. [bulut hizmeti oluşturma ve dağıtma](../../../articles/cloud-services/cloud-services-how-to-create-deploy-portal.md)).  Bu komut, belirtilen boyuttaki VM 'Leri kullanarak KES hizmetini belirtilen Azure bulut hizmetine otomatik olarak dağıtır.  Performansı önemli ölçüde azaltan sayfalama oluşmasını önlemek için, giriş dizini dosya boyutundan 1 GB daha RAM 'e sahip bir VM kullanmanızı öneririz.  Kullanılabilir VM boyutlarının listesi için bkz. [Cloud Services boyutları](../../../articles/cloud-services/cloud-services-sizes-specs.md).

Varsayılan olarak, hizmet hazırlama ortamına dağıtılır ve isteğe bağlı olarak--slot parametresi aracılığıyla geçersiz kılınır.  Desteklenen işlemlerin listesi için bkz. [Web API 'leri](WebAPI.md) .

<a name="describe_index-command"/>

## <a name="describe_index-command"></a>describe_index komutu

**Describe_index** komutu, şema ve açıklama dahil olmak üzere bir dizin dosyası hakkında bilgi verir.

`kes.exe describe_index <indexFile>`

| Parametre     | Açıklama      |
|---------------|------------------|
| `<indexFile>` | Giriş dizini yolu |

Bu dosya, bir yerel dosya yolu veya bir Azure Blob 'un URL yolu ile belirtilebilir.  Çıkış açıklaması dizesi, **build_index** komutunun--Description parametresi kullanılarak belirtilebilir.

<a name="describe_grammar-command"/>

## <a name="describe_grammar-command"></a>describe_grammar komutu

**Describe_grammar** komutu, ikili dilbilgisi oluşturmak için kullanılan özgün dilbilgisi belirtimini çıktı.

`kes.exe describe_grammar <grammarFile>`

| Parametre       | Açıklama      |
|-----------------|------------------|
| `<grammarFile>` | Dilbilgisi yolunu gir |

Bu dosya, bir yerel dosya yolu veya bir Azure Blob 'un URL yolu ile belirtilebilir.

