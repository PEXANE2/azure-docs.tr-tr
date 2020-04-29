---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/26/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a67ad4c5010cf93ff55123013a35c697ce5971f8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77597885"
---
Azure dosya paylaşımları, paylaşılan bir depolama havuzunu temsil eden üst düzey nesneler olan *depolama hesaplarına*dağıtılır. Bu depolama havuzu, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları, kuyrukları veya tabloları gibi diğer depolama kaynaklarını dağıtmak için kullanılabilir. Bir depolama hesabına dağıtılan tüm depolama kaynakları, bu depolama hesabı için uygulanan limitleri paylaşır. Bir depolama hesabının geçerli sınırlarını görmek için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../articles/storage/files/storage-files-scale-targets.md).

Azure dosya dağıtımları için kullanacağınız iki ana depolama hesabı türü vardır: 
- **Genel amaçlı sürüm 2 (GPv2) depolama hesapları**: GPv2 depolama hesapları, Azure dosya paylaşımlarını standart/sabit disk tabanlı (HDD tabanlı) donanımda dağıtmanıza olanak tanır. GPv2 depolama hesapları, Azure dosya paylaşımlarını depolamanın yanı sıra blob kapsayıcıları, kuyrukları veya tabloları gibi diğer depolama kaynaklarını da depolayabilirler. 
- **FileStorage depolama hesapları**: FileStorage depolama hesapları, Azure dosya paylaşımlarını Premium/katı hal disk tabanlı (SSD tabanlı) donanımda dağıtmanıza olanak tanır. FileStorage hesapları yalnızca Azure dosya paylaşımlarını depolamak için kullanılabilir; başka depolama kaynakları (blob kapsayıcıları, kuyruklar, tablolar vb.) bir FileStorage hesabında dağıtılabilir.

Azure portal, PowerShell veya CLı içinde karşılaşabileceğiniz birkaç farklı depolama hesabı türü vardır. İki depolama hesabı türü, BlockBlobStorage ve BlobStorage depolama hesapları Azure dosya paylaşımları içeremez. Görebileceğiniz diğer iki depolama hesabı türü, her ikisi de Azure dosya paylaşımları içerebilen genel amaçlı sürüm 1 (GPv1) ve klasik depolama hesaplarıdır. GPv1 ve klasik depolama hesaplarında Azure dosya paylaşımları bulunabilir, ancak Azure dosyalarının birçok yeni özelliği yalnızca GPv2 ve FileStorage depolama hesaplarında kullanılabilir. Bu nedenle, yalnızca Yeni dağıtımlar için GPv2 ve FileStorage depolama hesapları kullanmayı ve ortamınızda zaten varsa GPv1 ve klasik depolama hesaplarını yükseltmeyi öneririz.  