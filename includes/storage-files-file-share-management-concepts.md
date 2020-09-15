---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/26/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8d26b6c92462abc13c42257d2c6e571156eacc0f
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90563474"
---
Azure dosya paylaşımları, paylaşılan bir depolama havuzunu temsil eden üst düzey nesneler olan *depolama hesaplarına*dağıtılır. Bu depolama havuzu, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları, kuyrukları veya tabloları gibi diğer depolama kaynaklarını dağıtmak için kullanılabilir. Bir depolama hesabına dağıtılan tüm depolama kaynakları, bu depolama hesabı için uygulanan limitleri paylaşır. Bir depolama hesabının geçerli sınırlarını görmek için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../articles/storage/files/storage-files-scale-targets.md).

Azure dosya dağıtımları için kullanacağınız iki ana depolama hesabı türü vardır: 
- **Genel amaçlı sürüm 2 (GPv2) depolama hesapları**: GPv2 depolama hesapları, Azure dosya paylaşımlarını standart/sabit disk tabanlı (HDD tabanlı) donanımda dağıtmanıza olanak tanır. GPv2 depolama hesapları, Azure dosya paylaşımlarını depolamanın yanı sıra blob kapsayıcıları, kuyrukları veya tabloları gibi diğer depolama kaynaklarını da depolayabilirler. 
- **FileStorage depolama hesapları**: FileStorage depolama hesapları, Azure dosya paylaşımlarını Premium/katı hal disk tabanlı (SSD tabanlı) donanımda dağıtmanıza olanak tanır. FileStorage hesapları yalnızca Azure dosya paylaşımlarını depolamak için kullanılabilir; başka depolama kaynakları (blob kapsayıcıları, kuyruklar, tablolar vb.) bir FileStorage hesabında dağıtılabilir. Yalnızca FileStorage hesapları, SMB ve NFS dosya paylaşımlarını dağıtabilir.

Azure portal, PowerShell veya CLı içinde karşılaşabileceğiniz birkaç farklı depolama hesabı türü vardır. İki depolama hesabı türü, BlockBlobStorage ve BlobStorage depolama hesapları Azure dosya paylaşımları içeremez. Görebileceğiniz diğer iki depolama hesabı türü, her ikisi de Azure dosya paylaşımları içerebilen genel amaçlı sürüm 1 (GPv1) ve klasik depolama hesaplarıdır. GPv1 ve klasik depolama hesaplarında Azure dosya paylaşımları bulunabilir, ancak Azure dosyalarının birçok yeni özelliği yalnızca GPv2 ve FileStorage depolama hesaplarında kullanılabilir. Bu nedenle, yalnızca Yeni dağıtımlar için GPv2 ve FileStorage depolama hesapları kullanmayı ve ortamınızda zaten varsa GPv1 ve klasik depolama hesaplarını yükseltmeyi öneririz.  