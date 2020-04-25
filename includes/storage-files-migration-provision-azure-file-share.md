---
title: Azure dosya paylaşımlarını sağlama konuları
description: Azure Dosya Eşitleme ile kullanmak üzere Azure dosya paylaşımlarını sağlayın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: d48baba5ee60a2bf5a4cb5e4d1ce840fce8eec43
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82143613"
---
Azure dosya paylaşma, Bulutta Azure depolama hesabında depolanır.
Burada başka bir performans konuları vardır.

Yüksek düzeyde etkin paylaşımlarınız (çok sayıda kullanıcı ve/veya uygulama tarafından kullanılan paylaşımlar) varsa, iki Azure dosya paylaşımı bir depolama hesabının performans sınırına ulaşabilirler.

En iyi yöntem, depolama hesaplarını her bir dosya paylaşımıyla dağıtmaktır.
Birden çok Azure dosya paylaşımını aynı depolama hesabında havuza alabilirsiniz, bu durumda arşiv paylaşımlarınız olabilir veya bunlar içinde düşük günlük etkinlikleri bekleyebilir.

Bu konular, bulut erişimine (bir Azure VM aracılığıyla) Azure Dosya Eşitleme kıyasla daha fazlasını uygular. Yalnızca bu paylaşımlar üzerinde Azure Dosya Eşitleme kullanmayı planlıyorsanız, birkaçını tek bir Azure depolama hesabına göre gruplamak iyidir.

Paylaşımlarınızın bir listesini yaptıysanız, her bir paylaşımı bulunacağı depolama hesabıyla eşlemeniz gerekir.

Önceki aşamada, uygun paylaşım sayısını belirlediniz. Bu adımda, depolama hesaplarının dosya paylaşımlarına eşlenme oluşturmuş olursunuz. Artık Azure depolama hesaplarını uygun sayıda Azure dosya paylaşımına dağıtın.

Her depolama hesabınızın bölgesinin aynı olduğundan ve zaten dağıttığınız depolama eşitleme hizmeti kaynağının bölgesiyle eşleştiğinden emin olun.

> [!CAUTION]
> 100-TiB sınırına sahip bir Azure dosya paylaşımının oluşturulması durumunda bu paylaşımda yalnızca yerel olarak yedekli depolama veya bölgesel olarak yedekli depolama yedekliliği seçenekleri kullanılabilir. 100-TiB dosya paylaşımlarını kullanmadan önce depolama yedeklerinizin ihtiyaçlarını göz önünde bulundurun.

Azure dosya paylaşımları yine de varsayılan olarak 5-TiB sınırı ile oluşturulmuştur. Yeni depolama hesapları oluşturmakta olduğunuz için, [100-TiB limitleriyle Azure dosya paylaşımlarına izin veren depolama hesapları oluşturma kılavuzunu](../articles/storage/files/storage-files-how-to-create-large-file-share.md)izlediğinizden emin olun.

Bir depolama hesabı dağıttığınızda, Azure Storage 'ın yedekliliğe göre başka bir dikkat edin. Bkz. [Azure Storage artıklık seçenekleri](../articles/storage/common/storage-redundancy.md).

Kaynaklarınızın adları da önemlidir. Örneğin, bir Azure depolama hesabına HR departmanı için birden çok paylaşım gruplandırdıysanız depolama hesabını uygun şekilde adlandırın. Benzer şekilde, Azure dosya paylaşımlarınızı adlandırırken, şirket içi karşılıkları için kullanılan adlara benzer adlar kullanmanız gerekir.