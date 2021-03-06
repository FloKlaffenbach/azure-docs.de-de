---
title: 'Eav4- und Easv4-Serie: Azure Virtual Machines'
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Eav4- und Easv4-Serie.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 96cd7b4f4627e7c0335df9c745d8193a3dac7044
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77492619"
---
# <a name="eav4-and-easv4-series"></a>Eav4- und Easv4-Serie

Die Eav4-Serie und die Easv4-Serie verwenden den AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz in einer Multithreadkonfiguration mit bis zu 256 MB L3-Cache, wodurch bei der Ausführung der meisten arbeitsspeicheroptimierten Workloads mehr Optionen zur Verfügung stehen. Die Eav4-Serie und die Easv4-Serie verfügen über die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die Ev3- und die Esv3-Serie.

## <a name="eav4-series"></a>Eav4-Serie

ACU: 230-260

Storage Premium Nicht unterstützt

Storage Premium-Zwischenspeicherung: Nicht unterstützt

Die Größen der Eav4-Serie basieren auf dem AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz, der mittels Boosting einen maximalen Takt von 3,35 GHz erreichen und SSD Premium verwenden kann. Die Größen der Eav4-Serie sind ideal für arbeitsspeicherintensive Unternehmensanwendungen. Datenträgerspeicher wird separat zu virtuellen Computern abgerechnet. Um SSD Premium zu verwenden, verwenden Sie die Größen der Easv4-Serie. Für Easv4-Größen gelten die gleichen Preise und Verbrauchseinheiten für die Abrechnung wie bei der Eav3-Serie.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (temporärer Speicher): IOPS/MBit/s Lesen/MBps Schreiben | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
| -----|-----|-----|-----|-----|-----|-----|
| Standard\_E2a\_v4|2|16|50|4|3000/46/23|2/1000 |
| Standard\_E4a\_v4|4|32|100|8|6000/93/46|2/2000 |
| Standard\_E8a\_v4|8|64|200|16|12000/187/93|4/4000 |
| Standard\_E16a\_v4|16|128|400|32|24000/375/187|8 / 8000 |
| Standard\_E20a\_v4|20|160|500|32|30.000/468/234|8 / 10.000 |
| Standard\_E32a\_v4|32|256|800|32|48000/750/375|8/16000 |
| Standard\_E48a\_v4 <sup>**</sup> |48|384|1200|32| | |
| Standard\_E64a\_v4 <sup>**</sup> |64|512|1600|32| | |
| Standard\_E96a\_v4 <sup>**</sup> |96|672|2400|32| | |

<sup>**</sup>  Diese Größen befinden sich in der Vorschauphase. Wenn Sie daran interessiert sind, diese größeren Größen zu testen, registrieren Sie sich unter [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview).

## <a name="easv4-series"></a>Easv4-Serie

ACU: 230-260

Storage Premium Unterstützt

Storage Premium-Zwischenspeicherung: Unterstützt

Die Größen der Easv4-Serie basieren auf dem AMD-Prozessor EPYC<sup>TM</sup> 7452 mit 2,35 GHz, der mittels Boosting einen maximalen Takt von 3,35 GHz erreichen und SSD Premium verwenden kann. Die Größen der Easv4-Serie sind ideal für arbeitsspeicherintensive Unternehmensanwendungen.

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBit/s) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_E2as_v4|2|16|32|4|4000/32 (50)|3200/48|2/1000 |
| Standard_E4as_v4|4|32|64|8|8000/64 (100)|6400/96|2/2000 |
| Standard_E8as_v4|8|64|128|16|16000/128 (200)|12800/192|4/4000 |
| Standard_E16as_v4|16|128|256|32|32.000/255 (400)|25600/384|8 / 8000 |
| Standard_E20as_v4|20|160|320|32|40.000/320 (500)|32000/480|8 / 10.000 |
| Standard_E32as_v4|32|256|512|32|64.000/510 (800)|51200/768|8/16000 |
| Standard_E48as_v4 <sup>**</sup> |48|384|768|32|  | |
| Standard_E64as_v4 <sup>**</sup> |64|512|1024|32| | |
| Standard_E96as_v4 <sup>**</sup> |96|672|1344|32| | |  

<sup>**</sup>  Diese Größen befinden sich in der Vorschauphase. Wenn Sie daran interessiert sind, diese größeren Größen zu testen, registrieren Sie sich unter [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview).

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Andere Größen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.