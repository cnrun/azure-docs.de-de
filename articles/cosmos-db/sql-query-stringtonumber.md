---
title: StringToNumber in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion StringToNumber in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5ca8d0c4a6d244823dda6f0f79a3cf5c743a12a9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78296421"
---
# <a name="stringtonumber-azure-cosmos-db"></a>StringToNumber (Azure Cosmos DB)
 Gibt den Ausdruck übersetzt in eine Zahl zurück. Wenn der Ausdruck nicht übersetzt werden kann, wird „undefined“ zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql
StringToNumber(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck, der als JSON-Number-Ausdruck analysiert werden soll. Zahlen müssen in JSON im Integer- oder Gleitkommaformat angegeben werden. Ausführliche Informationen zum JSON-Format finden Sie unter [json.org](https://json.org/).  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Ausdruck für eine Zahl oder „undefined“ zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `StringToNumber` veranschaulicht. 

Leerzeichen sind ausschließlich vor oder nach der Zahl zulässig.

```sql
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Hier ist das Resultset.  
  
```json
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

In JSON muss eine gültige Zahl entweder eine ganze Zahl oder eine Gleitkommazahl sein.

```sql
SELECT   
    StringToNumber("0xF")
```  
  
 Hier ist das Resultset.  
  
```json
{{}}
```  

Der übergebene Ausdruck wird als Zahlenausdruck analysiert. Die folgenden Eingaben werden nicht als Zahlentyp ausgewertet und geben daher „undefined“ zurück: 

```sql
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Hier ist das Resultset.  
  
```json
{{}}
```  

## <a name="remarks"></a>Bemerkungen

Der Index wird von dieser Systemfunktion nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
