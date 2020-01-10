---
title: Ortak FabricClient özel durumları oluşturuldu
description: Uygulama ve küme yönetim işlemlerini gerçekleştirirken FabricClient API 'Leri tarafından oluşturulabilecek ortak özel durumları ve hataları açıklar.
author: oanapl
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: oanapl
ms.openlocfilehash: 9ad3097a490d4728e05ea90652c17c24b79cac2c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75457926"
---
# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>FabricClient API 'Leriyle çalışırken ortak özel durumlar ve hatalar
[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API 'leri, küme ve uygulama yöneticilerinin bir Service Fabric uygulama, hizmet veya küme üzerinde yönetim görevleri gerçekleştirmesini sağlar. Örneğin, uygulama dağıtımı, yükseltme ve kaldırma, bir kümenin sistem durumunu denetleme veya bir hizmeti test etme. Uygulama geliştiricileri ve küme yöneticileri, Service Fabric kümesini ve uygulamalarını yönetmek için Araçlar geliştirmek üzere FabricClient API 'Lerini kullanabilir.

FabricClient kullanılarak gerçekleştirilebilecek birçok farklı türde işlem vardır.  Her yöntem hatalı giriş, çalışma zamanı hataları veya geçici altyapı sorunları nedeniyle hatalar için özel durumlar oluşturabilir.  Belirli bir yöntem tarafından hangi özel durumların oluşturulduğu hakkında bilgi edinmek için API başvuru belgelerine bakın. Ancak, birçok farklı [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) API 'si tarafından oluşturulabilecek bazı özel durumlar vardır. Aşağıdaki tabloda, FabricClient API 'Leri genelinde ortak olan özel durumlar listelenmektedir.

| Özel durum | Oluşturulduğu zaman |
| --- |:--- |
| [System. Fabric. FabricObjectClosedException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricobjectclosedexception) |[FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesi kapalı durumda. Kullanmakta olduğunuz [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesini atın ve yeni bir [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) nesnesi örneği oluşturun. |
| [System. TimeoutException](https://docs.microsoft.com/dotnet/core/api/system.timeoutexception) |İşlem zaman aşımına uğradı. İşlemin tamamlanmasını MaxOperationTimeout 'den fazla sürerse [OperationTimeout](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) döndürülür. |
| [System. UnauthorizedAccessException](https://docs.microsoft.com/dotnet/core/api/system.unauthorizedaccessexception) |İşlem için erişim denetimi başarısız oldu. E_ACCESSDENIED döndürülür. |
| [System. Fabric. FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) |İşlem gerçekleştirilirken bir çalışma zamanı hatası oluştu. FabricClient metotlarından herhangi biri, [Fabricexception](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception)oluşturabilir, [hata kodu](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception.ErrorCode) özelliği özel durumun tam nedenini gösterir. Hata kodları [Fabricerrorcode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) numaralandırması içinde tanımlanmıştır. |
| [System. Fabric. Fabricgeçişli Entexception](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception) |İşlem, bazı tür geçici bir hata durumu nedeniyle başarısız oldu. Örneğin, bir çoğaltma çekirdeğini geçici olarak erişilemediğinden bir işlem başarısız olabilir. Geçici özel durumlar, yeniden denenebilecek başarısız işlemlere karşılık gelir. |

Bir [Fabricexception](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception)içinde döndürülebilecek bazı yaygın [fabricerrorcode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) hataları:

| Hata | Koşul |
| --- |:--- |
| CommunicationError |Bir iletişim hatası işlemin başarısız olmasına neden oldu, işlemi yeniden deneyin. |
| Invalidcredentialtype |Kimlik bilgisi türü geçersiz. |
| InvalidX509FindType |X509FindType geçersiz. |
| InvalidX509StoreLocation |X509 depolama konumu geçersiz. |
| InvalidX509StoreName |X509 Mağazası adı geçersiz. |
| InvalidX509Thumbprint |X509 sertifikası parmak izi dizesi geçersiz. |
| Invalidprotectionlevel |Koruma düzeyi geçersiz. |
| InvalidX509Store |X509 sertifika deposu açılamıyor. |
| Invalidsubjectname |Konu adı geçersiz. |
| Invalidallodilimcommonnamelist |Ortak ad listesi dizesinin biçimi geçersiz. Bu, virgülle ayrılmış bir liste olmalıdır. |

