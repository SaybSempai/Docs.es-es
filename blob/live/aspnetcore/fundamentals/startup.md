---
title: "Inicio de la aplicación de ASP.NET Core"
author: ardalis
description: "Descubra cómo la clase de inicio de ASP.NET Core configura servicios y la canalización de solicitud de la aplicación."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="acc98-103">Inicio de la aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acc98-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="acc98-104">Por [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="acc98-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="acc98-105">La `Startup` clase configura servicios y la canalización de solicitud de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="acc98-106">La clase de inicio</span><span class="sxs-lookup"><span data-stu-id="acc98-106">The Startup class</span></span>

<span data-ttu-id="acc98-107">Uso de aplicaciones de ASP.NET Core un `Startup` (clase), que se denomina `Startup` por convención.</span><span class="sxs-lookup"><span data-stu-id="acc98-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="acc98-108">La `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="acc98-108">The `Startup` class:</span></span>

* <span data-ttu-id="acc98-109">Puede incluir opcionalmente un [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método para configurar los servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="acc98-110">Debe incluir un [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método para crear la canalización de procesamiento de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="acc98-111">`ConfigureServices`y `Configure` el tiempo de ejecución llama al iniciarse la aplicación:</span><span class="sxs-lookup"><span data-stu-id="acc98-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="acc98-112">Especifique el `Startup` clase con la [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método:</span><span class="sxs-lookup"><span data-stu-id="acc98-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="acc98-113">El `Startup` constructor de clase acepta dependencias definidas por el host.</span><span class="sxs-lookup"><span data-stu-id="acc98-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="acc98-114">Un uso común de [inyección de dependencia](xref:fundamentals/dependency-injection) en el `Startup` clase consiste en Insertar [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) para configurar servicios de entorno:</span><span class="sxs-lookup"><span data-stu-id="acc98-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="acc98-115">Alternativa al insertar `IHostingStartup` consiste en utilizar un enfoque basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="acc98-115">An alternative to injecting `IHostingStartup` is to use a conventions-based approach.</span></span> <span data-ttu-id="acc98-116">La aplicación puede definir independiente `Startup` clases para los entornos diferentes (por ejemplo, `StartupDevelopment`), y la clase de inicio correspondiente se selecciona en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="acc98-116">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="acc98-117">Se establece una prioridad de la clase cuyo sufijo de nombre coincide con el entorno actual.</span><span class="sxs-lookup"><span data-stu-id="acc98-117">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="acc98-118">Si la aplicación se ejecuta en el entorno de desarrollo e incluye tanto una `Startup` clase y un `StartupDevelopment` (clase), el `StartupDevelopment` se utiliza la clase.</span><span class="sxs-lookup"><span data-stu-id="acc98-118">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="acc98-119">Para obtener más información, consulte [trabajar con varios entornos](xref:fundamentals/environments#startup-conventions).</span><span class="sxs-lookup"><span data-stu-id="acc98-119">For more information see [Working with multiple environments](xref:fundamentals/environments#startup-conventions).</span></span>

<span data-ttu-id="acc98-120">Para obtener más información acerca de `WebHostBuilder`, consulte el [hospedaje](xref:fundamentals/hosting) tema.</span><span class="sxs-lookup"><span data-stu-id="acc98-120">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/hosting) topic.</span></span> <span data-ttu-id="acc98-121">Para obtener información sobre cómo controlar errores durante el inicio, consulte [control de excepciones de inicio](xref:fundamentals/error-handling#startup-exception-handling).</span><span class="sxs-lookup"><span data-stu-id="acc98-121">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="acc98-122">El método ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="acc98-122">The ConfigureServices method</span></span>

<span data-ttu-id="acc98-123">El [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) método es:</span><span class="sxs-lookup"><span data-stu-id="acc98-123">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="acc98-124">Opcional.</span><span class="sxs-lookup"><span data-stu-id="acc98-124">Optional.</span></span>
* <span data-ttu-id="acc98-125">Lo llama el host de web antes de la `Configure` método para configurar los servicios de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-125">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="acc98-126">Donde [opciones de configuración](xref:fundamentals/configuration/index) se establecen por convención.</span><span class="sxs-lookup"><span data-stu-id="acc98-126">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="acc98-127">Agregar servicios al contenedor de servicios pone a disposición de la aplicación y de la `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="acc98-127">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="acc98-128">Los servicios se resuelven a través de [inyección de dependencia](xref:fundamentals/dependency-injection) o de [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span><span class="sxs-lookup"><span data-stu-id="acc98-128">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="acc98-129">Puede configurar el host de web algunos servicios antes `Startup` se llaman a métodos.</span><span class="sxs-lookup"><span data-stu-id="acc98-129">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="acc98-130">Los detalles están disponibles en la [hospedaje](xref:fundamentals/hosting) tema.</span><span class="sxs-lookup"><span data-stu-id="acc98-130">Details are available in the [Hosting](xref:fundamentals/hosting) topic.</span></span> 

<span data-ttu-id="acc98-131">Para las características que requieren el programa de instalación sustancial, hay `Add[Service]` métodos de extensión en [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span><span class="sxs-lookup"><span data-stu-id="acc98-131">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="acc98-132">Una aplicación web típica registra los servicios de Entity Framework, identidad y MVC:</span><span class="sxs-lookup"><span data-stu-id="acc98-132">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a><span data-ttu-id="acc98-133">Servicios disponibles en el inicio</span><span class="sxs-lookup"><span data-stu-id="acc98-133">Services available in Startup</span></span>

<span data-ttu-id="acc98-134">El host de web proporciona algunos servicios que están disponibles para el `Startup` constructor de clase.</span><span class="sxs-lookup"><span data-stu-id="acc98-134">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="acc98-135">La aplicación agrega servicios adicionales a través de `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="acc98-135">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="acc98-136">A continuación, están disponibles en el host y la aplicación servicios `Configure` y a lo largo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-136">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="acc98-137">El método Configure</span><span class="sxs-lookup"><span data-stu-id="acc98-137">The Configure method</span></span>

<span data-ttu-id="acc98-138">El [configurar](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) método se utiliza para especificar la forma en que la aplicación responde a las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="acc98-138">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="acc98-139">La canalización de solicitudes está configurada mediante la adición de [middleware](xref:fundamentals/middleware) componentes a un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instancia.</span><span class="sxs-lookup"><span data-stu-id="acc98-139">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="acc98-140">`IApplicationBuilder`está disponible para el `Configure` método, pero no está registrado en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="acc98-140">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="acc98-141">Hospedaje crea un `IApplicationBuilder` y lo pasa directamente a `Configure` ([origen de referencia](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span><span class="sxs-lookup"><span data-stu-id="acc98-141">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="acc98-142">El [plantillas ASP.NET Core](/dotnet/core/tools/dotnet-new) configurar la canalización con compatibilidad para una página de excepción para desarrolladores, [BrowserLink](http://vswebessentials.com/features/browserlink), ASP.NET MVC, archivos estáticos y las páginas de error:</span><span class="sxs-lookup"><span data-stu-id="acc98-142">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="acc98-143">Cada `Use` método de extensión agrega un componente de middleware a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="acc98-143">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="acc98-144">Por ejemplo, el `UseMvc` método de extensión agrega la [middleware enrutamiento](xref:fundamentals/routing) a la canalización de solicitud y configura [MVC](xref:mvc/overview) como controlador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="acc98-144">For instance, the `UseMvc` extension method adds the [routing middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="acc98-145">Servicios adicionales, como `IHostingEnvironment` y `ILoggerFactory`, también se pueden especificar en la firma del método.</span><span class="sxs-lookup"><span data-stu-id="acc98-145">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="acc98-146">Cuando se especifica, se insertan servicios adicionales si están disponibles.</span><span class="sxs-lookup"><span data-stu-id="acc98-146">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="acc98-147">Para obtener más información sobre cómo usar `IApplicationBuilder`, consulte [Middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="acc98-147">For more information on how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="acc98-148">Métodos útiles</span><span class="sxs-lookup"><span data-stu-id="acc98-148">Convenience methods</span></span>

<span data-ttu-id="acc98-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) y [configurar](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) métodos útiles que pueden usarse en lugar de especificar un `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="acc98-149">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="acc98-150">Varias llamadas a `ConfigureServices` anexar entre sí.</span><span class="sxs-lookup"><span data-stu-id="acc98-150">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="acc98-151">Varias llamadas a `Configure` usar la última llamada de método.</span><span class="sxs-lookup"><span data-stu-id="acc98-151">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a><span data-ttu-id="acc98-152">Filtros de inicio</span><span class="sxs-lookup"><span data-stu-id="acc98-152">Startup filters</span></span>

<span data-ttu-id="acc98-153">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) para configurar el middleware al principio o al final de una aplicación [configurar](#the-configure-method) canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="acc98-153">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="acc98-154">`IStartupFilter`es útil para garantizar que se ejecuta un middleware antes o después de agregar bibliotecas al principio o al final de la canalización de procesamiento de solicitudes de la aplicación de middleware.</span><span class="sxs-lookup"><span data-stu-id="acc98-154">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="acc98-155">`IStartupFilter`implementa un método único, [configurar](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), que recibe y devuelve un `Action<IApplicationBuilder>`.</span><span class="sxs-lookup"><span data-stu-id="acc98-155">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="acc98-156">Un [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) define una clase para configurar la canalización de solicitud de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-156">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="acc98-157">Para obtener más información, consulte [crear una canalización de middleware con IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="acc98-157">For more information, see [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="acc98-158">Cada `IStartupFilter` implementa middlewares uno o más en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="acc98-158">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="acc98-159">Los filtros se invocan en el orden en que se agregaron al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="acc98-159">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="acc98-160">Los filtros pueden producir middleware antes o después de pasar el control al siguiente filtro, por lo tanto anexa al principio o al final de la canalización de aplicación.</span><span class="sxs-lookup"><span data-stu-id="acc98-160">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="acc98-161">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) se muestra cómo registrar un middleware con `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="acc98-161">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="acc98-162">La aplicación de ejemplo incluye un middleware que establece un valor de opciones de un parámetro de cadena de consulta:</span><span class="sxs-lookup"><span data-stu-id="acc98-162">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="acc98-163">El `RequestSetOptionsMiddleware` está configurado en el `RequestSetOptionsStartupFilter` clase:</span><span class="sxs-lookup"><span data-stu-id="acc98-163">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="acc98-164">El `IStartupFilter` está registrado en el contenedor de servicios en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="acc98-164">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="acc98-165">Cuando un parámetro de cadena de consulta para `option` es siempre el middleware procesa el valor asignado antes de que el middleware MVC representa la respuesta:</span><span class="sxs-lookup"><span data-stu-id="acc98-165">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![Ventana del explorador que muestra la página de índice representada.](startup/_static/index.png)

<span data-ttu-id="acc98-168">Orden de ejecución de middleware se establece por el orden de `IStartupFilter` registros:</span><span class="sxs-lookup"><span data-stu-id="acc98-168">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="acc98-169">Varias `IStartupFilter` las implementaciones pueden interactuar con los mismos objetos.</span><span class="sxs-lookup"><span data-stu-id="acc98-169">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="acc98-170">Si el orden es importante, ordenar sus `IStartupFilter` registros para que coincida con el orden en que se debe ejecutar sus middlewares de servicio.</span><span class="sxs-lookup"><span data-stu-id="acc98-170">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="acc98-171">Las bibliotecas pueden agregar middleware con uno o varios `IStartupFilter` implementaciones que se ejecuten antes o después de otro middleware de aplicación registrado con `IStartupFilter`.</span><span class="sxs-lookup"><span data-stu-id="acc98-171">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="acc98-172">Para invocar un `IStartupFilter` middleware antes un middleware agregado una biblioteca `IStartupFilter`, colocar el registro del servicio antes de que la biblioteca se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="acc98-172">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="acc98-173">Para invocar a continuación, colocar el registro del servicio después de agrega la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="acc98-173">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acc98-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="acc98-174">Additional resources</span></span>

* [<span data-ttu-id="acc98-175">Hospedar aplicaciones de WPF</span><span class="sxs-lookup"><span data-stu-id="acc98-175">Hosting</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="acc98-176">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="acc98-176">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="acc98-177">Middleware</span><span class="sxs-lookup"><span data-stu-id="acc98-177">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="acc98-178">Registro</span><span class="sxs-lookup"><span data-stu-id="acc98-178">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="acc98-179">Configuración</span><span class="sxs-lookup"><span data-stu-id="acc98-179">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="acc98-180">Clase StartupLoader: método FindStartupType (origen de referencia)</span><span class="sxs-lookup"><span data-stu-id="acc98-180">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)