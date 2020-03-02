---
title: Azure dosya paylaşımlarını sağlamaya yönelik hususlar.
description: Azure Dosya Eşitleme ile kullanmak üzere Azure dosya paylaşımlarını sağlayın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 8cb398d1b1ec14f52d9c5fa5c122dc2e4ba4376d
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209434"
---
Azure dosya paylaşma, Bulutta Azure depolama hesabında depolanır.
Burada başka bir performans konuları düzeyi vardır.

Çok sayıda kullanıcı ve/veya uygulama tarafından kullanılan yüksek düzeyde etkin paylaşımlar varsa, iki Azure dosya paylaşımı bir depolama hesabının performans sınırına ulaşabilir.

En iyi yöntem, depolama hesaplarını her bir dosya paylaşımıyla dağıtmaktır.
Birden çok Azure dosya paylaşımını aynı depolama hesabında havuza alabilirsiniz, bu durumda arşiv paylaşımlarınız olabilir veya bunlar içinde düşük günlük etkinlikleri bekleyebilir.

Bu konular, bulut erişimi (bir Azure VM aracılığıyla) Azure Dosya Eşitleme için uygulandıklarından daha fazlasını uygular. Bu paylaşımlar üzerinde yalnızca Azure Dosya Eşitleme kullanmayı planlıyorsanız, birkaçını tek bir Azure depolama hesabına göre gruplamak iyidir.

Paylaşımlarınızın bir listesini yaptıysanız, her bir paylaşımı bulundukları depolama hesabıyla eşlemeniz gerekir.

Önceki aşamada, uygun sayıda paylaşım belirlediniz. Bu adımda, depolama hesaplarının dosya paylaşımlarına eşlenme oluşturmuş olursunuz. Artık uygun sayıda Azure Depolama hesabını, bunlarda uygun sayıda Azure dosya paylaşımı ile dağıtın.

Her depolama hesabınızın bölgesinin aynı olduğundan ve zaten dağıttığınız depolama eşitleme hizmeti kaynağının bölgesiyle eşleştiğinden emin olun.

> [!CAUTION]
> Bir 100 TiB sınırı Azure dosya paylaşımının oluşturulması durumunda bu paylaşımda yalnızca yerel olarak yedekli depolama veya bölge yedekli depolama yedekliliği seçenekleri kullanılabilir. 100 TiB dosya paylaşımlarını kullanmadan önce depolama yedeklerinizin ihtiyaçlarını göz önünde bulundurun.

Azure dosya paylaşımları yine de varsayılan olarak 5 TiB sınırı ile oluşturulmuştur. Yeni depolama hesapları oluştururken, [100 TiB limitleriyle Azure dosya paylaşımlarına izin veren depolama hesapları oluşturma kılavuzunu](../articles/storage/files/storage-files-how-to-create-large-file-share.md)izlediğinizden emin olun.

Bir depolama hesabı dağıtmanın bir diğer değerlendirmesi, Azure depolama alanınızı yedeklemidir. Bkz: [Azure Storage artıklık seçenekleri](../articles/storage/common/storage-redundancy.md).

Kaynaklarınızın adları da önemlidir. Örneğin, bir Azure depolama hesabına HR departmanı için birden çok paylaşım gruplandırdıysanız depolama hesabını uygun şekilde adlandırın. Benzer şekilde, Azure dosya paylaşımlarınızı adlandırırken, şirket içi karşılıkları için kullanılan adlara benzer adlar kullanmanız gerekir.