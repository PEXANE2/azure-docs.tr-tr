---
title: Azure dosya paylaşımlarını sağlamada dikkat edilmesi gerekenler.
description: Azure Dosya Eşitleme ile kullanılmak üzere Azure dosya paylaşımlarını sağlama. Geçiş dokümanları arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 8cb398d1b1ec14f52d9c5fa5c122dc2e4ba4376d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78209434"
---
Azure dosya paylaşımı bulutta bir Azure depolama hesabında depolanır.
Burada performans konuları başka bir düzey dir.

Çok aktif paylaşımlarınız varsa - birçok kullanıcı ve/veya uygulama tarafından kullanılan paylaşımlar, iki Azure dosya paylaşımı bir depolama hesabının performans sınırına ulaşabilir.

En iyi yöntem, depolama hesaplarını her biri bir dosya paylaşımıyla dağıtmaktır.
Arşiv paylaşımlarınız olması veya günlük etkinliklerin izinden düşmesi durumunda, birden çok Azure dosya paylaşımını aynı depolama hesabına birleştirebilirsiniz.

Bu hususlar, doğrudan bulut erişimine (Azure VM aracılığıyla) Azure Dosya Eşitlemesine uyguladıklarından daha fazla uygulanır. Bu paylaşımlarda yalnızca Azure Dosya Eşitlemi kullanmayı planlıyorsanız, birkaç tanesini tek bir Azure depolama hesabına gruplandırmak sorun değil.

Paylaşımlarınızın bir listesini yaptıysanız, her paylaşımın içinde kalacakları depolama hesabıyla eşlemesi gerekir.

Önceki aşamada, uygun hisse sayısını belirlediniz. Bu adımda, paylaşımları dosyalamak için depolama hesaplarının eşlemesini oluşturdunuz. Artık uygun sayıda Azure depolama hesabı nı, içinde uygun sayıda Azure dosya paylaşımı yla dağıtın.

Depolama hesaplarınızın her birinin bölgesinin aynı olduğundan ve zaten dağıtmış olduğunuz Depolama Eşitleme Hizmeti kaynağının bölgesiyle eşleştiğinden emin olun.

> [!CAUTION]
> 100 TiB sınırazure dosya paylaşımı oluşturursanız, bu paylaşım yalnızca yerel olarak yedekli depolama veya bölge yedekli depolama artıkları seçeneklerini kullanabilir. 100 TiB dosya paylaşımını kullanmadan önce depolama artıklığı gereksinimlerinizi göz önünde bulundurun.

Azure dosya paylaşımları varsayılan olarak 5 TiB sınırıyla oluşturulur. Yeni depolama hesapları oluşturduğunuzdan, Azure [dosya paylaşımına 100 TiB sınırı yla izin veren depolama hesapları oluşturmak için kılavuzu izlediğinden](../articles/storage/files/storage-files-how-to-create-large-file-share.md)emin olun.

Bir depolama hesabı dağıtırken göz önünde bulundurulması gereken bir diğer husus da Azure depolama alanınızın artıklığıdır. Bakınız: [Azure Depolama artıklık seçenekleri.](../articles/storage/common/storage-redundancy.md)

Kaynaklarınızın adları da önemlidir. Örneğin, İk departmanı için birden çok paylaşımı bir Azure depolama hesabına gruplandırmanız gerekiyorsa, depolama hesabını uygun şekilde adlandırmanız gerekir. Benzer şekilde, Azure dosya paylaşımlarınızı adlandırırken, şirket içi benzerleri için kullanılanadlara benzer adlar kullanmalısınız.