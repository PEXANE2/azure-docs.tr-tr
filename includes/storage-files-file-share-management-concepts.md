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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597885"
---
Azure dosya paylaşımları, paylaşılan bir depolama havuzunun temsil edildiği üst düzey nesneler olan *depolama hesaplarına*dağıtılır. Bu depolama havuzu, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları, kuyruklar veya tablolar gibi diğer depolama kaynaklarını dağıtmak için kullanılabilir. Bir depolama hesabına dağıtılan tüm depolama kaynakları, bu depolama hesabı için geçerli olan sınırları paylaşır. Bir depolama hesabının geçerli sınırlarını görmek için [Azure Dosyaları ölçeklenebilirliği ve performans hedefleri'ne](../articles/storage/files/storage-files-scale-targets.md)bakın.

Azure Dosyaları dağıtımları için kullanacağınız iki ana depolama hesabı türü vardır: 
- **Genel amaçlı sürüm 2 (GPv2) depolama hesapları : GPv2**depolama hesapları, Azure dosya paylaşımlarını standart/sabit disk tabanlı (HDD tabanlı) donanıma dağıtmanıza olanak tanır. GPv2 depolama hesapları, Azure dosya paylaşımlarını depolamanın yanı sıra blob kapsayıcıları, kuyruklar veya tablolar gibi diğer depolama kaynaklarını da depolayabilir. 
- **FileStorage depolama hesapları**: FileStorage depolama hesapları, Azure dosya paylaşımlarını premium/solid-state disk tabanlı (SSD tabanlı) donanıma dağıtmanıza olanak tanır. FileStorage hesapları yalnızca Azure dosya paylaşımlarını depolamak için kullanılabilir; FileStorage hesabında başka depolama kaynakları (blob kapsayıcıları, kuyruklar, tablolar, vb.) dağıtılamaz.

Azure portalında, PowerShell'de veya CLI'de karşılaşabileceğiniz başka depolama hesabı türleri de vardır. İki depolama hesabı türü, BlockBlobStorage ve BlobStorage depolama hesapları, Azure dosya paylaşımları içeremez. Görebileceğiniz diğer iki depolama hesabı türü, her ikisi de Azure dosya paylaşımları içerebilen genel amaçlı sürüm 1 (GPv1) ve klasik depolama hesaplarıdır. GPv1 ve klasik depolama hesapları Azure dosya paylaşımları içerse de, Azure Dosyaları'nın yeni özelliklerinin çoğu yalnızca GPv2 ve FileStorage depolama hesaplarında kullanılabilir. Bu nedenle, yalnızca yeni dağıtımlar için GPv2 ve FileStorage depolama hesaplarını kullanmanızı ve ortamınızda zaten varsa GPv1 ve klasik depolama hesaplarını yükseltmenizi öneririz.  