---
title: Polymorfism i Reliable Actors Framework
description: Bygg hierarkier av .NET-gränssnitt och-typer i Reliable Actors Framework för att återanvända funktioner och API-definitioner.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.custom: devx-track-csharp
ms.openlocfilehash: 7552609fc4c85cdd5033e67b1bab7fb58cb6ebf1
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89016571"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfism i Reliable Actors Framework
Med Reliable Actors Framework kan du skapa aktörer med många av de metoder som du kan använda i objektorienterad design. En av dessa tekniker är polymorfism, som gör att typer och gränssnitt kan ärvas från mer generaliserade föräldrar. Arv i Reliable Actors Framework följer i allmänhet .NET-modellen med några ytterligare begränsningar. I händelse av Java/Linux följer Java-modellen.

## <a name="interfaces"></a>Gränssnitt
Reliable Actors Framework kräver att du definierar minst ett gränssnitt som ska implementeras av din aktörs typ. Det här gränssnittet används för att generera en proxy-klass som kan användas av klienter för att kommunicera med dina aktörer. Gränssnitt kan ärva från andra gränssnitt så länge alla gränssnitt som implementeras av en aktörs typ och alla dess föräldrar är härledda från IActor (C#) eller skådespelare (Java). IActor (C#) och skådespelare (Java) är de plattforms oberoende bas gränssnitten för aktörer i ramverken .NET och Java. Det klassiska polymorfism-exemplet som använder former kan därför se ut ungefär så här:

![Gränssnitts-hierarki för form aktörer][shapes-interface-hierarchy]

## <a name="types"></a>Typer
Du kan också skapa en hierarki av aktörs typer som är härledda från den bas aktörs klass som tillhandahålls av plattformen. När det gäller former kan du ha en Base `Shape` -eller `ShapeImpl` (Java)-typ:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

Under typer av `Shape` (C#) eller `ShapeImpl` (Java) kan åsidosätta metoder från basen.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Notera `ActorService` attributet för aktörs typen. Det här attributet visar det pålitliga aktörs ramverket att den automatiskt ska skapa en tjänst för värdbaserade aktörer av den här typen. I vissa fall kanske du vill skapa en bastyp som enbart är avsedd för delnings funktioner med under typer och som aldrig kommer att användas för att instansiera konkreta aktörer. I dessa fall bör du använda `abstract` nyckelordet för att indikera att du aldrig kommer att skapa en aktör utifrån den typen.

## <a name="next-steps"></a>Nästa steg
* Se [hur Reliable Actors Framework utnyttjar Service Fabric plattform](service-fabric-reliable-actors-platform.md) för att tillhandahålla tillförlitlighet, skalbarhet och konsekvent tillstånd.
* Lär dig mer om [aktörens livs cykel](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
