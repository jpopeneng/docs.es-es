---
title: '\ (Operador)'
ms.date: 07/20/2015
f1_keywords:
- vb.\
- '\'
helpviewer_keywords:
- division operator [Visual Basic], integer
- integer division operator [Visual Basic]
- zero, division by zero
- arithmetic operators [Visual Basic], division
- division [Visual Basic], by zero
- backslash (\) [Visual Basic]
- '\ operator [Visual Basic]'
- integer quotient
- math operators [Visual Basic]
- quotients, integer
- truncation [Visual Basic], integer division
ms.assetid: 4b0ee347-950c-45c9-8e23-54bc85df208e
ms.openlocfilehash: cf2dc66532925d56cea6fd141f44a245bc2dd8dd
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873399"
---
# <a name="-operator-visual-basic"></a>\ (Operador, Visual Basic)

Divide dos números y devuelve un resultado de número entero.  
  
## <a name="syntax"></a>Sintaxis  
  
```vb  
expression1 \ expression2  
```  
  
## <a name="parts"></a>Partes  

 `expression1`  
 Obligatorio. Cualquier expresión numérica.  
  
 `expression2`  
 Obligatorio. Cualquier expresión numérica.  
  
## <a name="supported-types"></a>Tipos admitidos  

 Todos los tipos numéricos, incluidos los tipos de punto flotante y sin signo y `Decimal` .  
  
## <a name="result"></a>Resultado  

 El resultado es el cociente entero de `expression1` divide by `expression2` , que descarta cualquier resto y solo conserva la parte entera. Esto se conoce como *truncamiento*.  
  
 El tipo de datos del resultado es un tipo numérico adecuado para los tipos de datos de `expression1` y `expression2` . Vea las tablas "aritmética de enteros" en [tipos de datos de resultados de operadores](data-types-of-operator-results.md).  
  
 El [operador/(Visual Basic)](floating-point-division-operator.md) devuelve el cociente completo, que conserva el resto de la parte fraccionaria.  
  
## <a name="remarks"></a>Comentarios  

 Antes de realizar la división, Visual Basic intenta convertir cualquier expresión numérica de punto flotante en `Long` . Si `Option Strict` es `On` , se produce un error del compilador. Si `Option Strict` es `Off` , <xref:System.OverflowException> es posible si el valor está fuera del intervalo del tipo de [datos Long](../data-types/long-data-type.md). La conversión a `Long` también está sujeta a *redondeo bancario*. Para obtener más información, vea "partes fraccionarias" en [funciones de conversión de tipos](../functions/type-conversion-functions.md).  
  
 Si `expression1` o `expression2` se evalúa como [Nothing](../nothing.md), se trata como cero.  
  
## <a name="attempted-division-by-zero"></a>División intentada por cero  

 Si `expression2` se evalúa como cero, el `\` operador produce una <xref:System.DivideByZeroException> excepción. Esto es así para todos los tipos de datos numéricos de los operandos.  
  
> [!NOTE]
> El `\` operador se puede *sobrecargar*, lo que significa que una clase o estructura puede volver a definir su comportamiento cuando un operando tiene el tipo de esa clase o estructura. Si el código usa este operador en una clase o estructura de este tipo, asegúrese de entender su comportamiento redefinido. Para obtener más información, consulta [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Ejemplo  

 En el ejemplo siguiente se usa el `\` operador para realizar la división de enteros. El resultado es un entero que representa el cociente entero de los dos operandos, con el resto descartado.  
  
 [!code-vb[VbVbalrOperators#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#18)]  
  
 Las expresiones del ejemplo anterior devuelven los valores 2, 3, 33 y-22, respectivamente.  
  
## <a name="see-also"></a>Consulte también

- [\\= (Operador)](integer-division-assignment-operator.md)
- [Operador/(Visual Basic)](floating-point-division-operator.md)
- [Option Strict (instrucción)](../statements/option-strict-statement.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Prioridad de operador en Visual Basic](operator-precedence.md)
- [Lista de operadores según funcionalidad](operators-listed-by-functionality.md)
- [Operadores aritméticos en Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
