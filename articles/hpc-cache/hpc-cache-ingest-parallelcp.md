---
title: Azure HPC Önbellek veri yutma - paralel kopyalama komut dosyası
description: Azure HPC Önbelleğinde verileri Blob depolama hedefine taşımak için paralel kopyalama komut dosyası nasıl kullanılır?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 90e05ad3d42b1009b631630fe476669a9f418d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74166888"
---
# <a name="azure-hpc-cache-data-ingest---parallel-copy-script-method"></a>Azure HPC Önbellek veri yutma - paralel kopyalama komut dosyası yöntemi

Bu makalede, ``parallelcp`` komut dosyası oluşturmak ve verileri Azure HPC Önbelleği ile kullanılmak üzere bir Blob depolama kapsayıcısına taşımak için kullanmak için yönergeler verir.

Azure HPC Önbelleğiniz için verileri Blob depolama alanına taşıma hakkında daha fazla bilgi edinmek için [verileri Azure Blob depolama alanına taşı'nı](hpc-cache-ingest.md)okuyun.

## <a name="create-the-parallelcp-script"></a>Paralelcp komut dosyası oluşturma

Aşağıdaki komut dosyası yürütülebilir `parallelcp`ekler. (Bu komut dosyası Ubuntu için tasarlanmıştır; başka ``parallel`` bir dağıtım kullanıyorsanız, ayrı olarak yüklemeniz gerekir.)

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM
#!/bin/bash

display_usage() {
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n"
}

if [  \$# -le 1 ] ; then
    display_usage
    exit 1
fi

if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then
    display_usage
    exit 0
fi

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

## <a name="parallel-copy-example"></a>Paralel kopya örneği

Bu örnek, Azure HPC ``glibc`` Önbelleğinde kaynak dosyaları kullanarak derlemek için paralel kopya komut dosyasını kullanır.

Kaynak dosyalar Azure HPC Önbellek montaj noktasında önbelleğe alınır ve nesne dosyaları yerel sabit diskte depolanır.

Bu örnek, paralelleştirme kazanmak ``-j`` için ``make`` seçenekle paralel kopya komut dosyasını kullanır.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/cache1 hpccache
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```
